<div>

<div>

<div>

<div>

-   ![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)
-   Multi-region Deployment with Seaplane Managed Global SQL

<div>

On this page

</div>

<div>

<div>

# Multi-region Deployment with Seaplane Managed Global SQL

</div>

In this guide, we are taking a look at how you can use Seaplane Managed
Global SQL (*MGSQL*) to create a multi-region SQL database to serve
global users without additional overhead. For the sake of simplicity,
this guide assumes the database tables involved do not contain any
personally identifiable information and do not need to comply with
regulations like CPRA or GDPR.

If you\'re interested in using Seaplane to comply with global data
privacy laws, check out Data Regulation Compliance with Seaplane SQL.

## The Scenario​

If you\'ve worked with multi-region databases before, you already know
what a hassle it can be to scale from one region to multiple regions. If
you\'ve managed to avoid multi-region databases, you\'ve still probably
seen those \"simple\" how-to diagrams from other providers with dozens
of arrows, labels, services, and upsells promising a global solution to
your data woes.

The reason these diagrams (and multi-region databases at large) are so
complicated comes down to supporting services. New regions need new
routing, load balancers, read replicas, failover strategies, and more.
The work extends well beyond picking a database, and in this guide we
won\'t even begin to cover the really messy stuff, like active-active
reader + writer deployments, which are a whole \'nother world of pain
involving data synchronization and consistency.

Fear not, it\'s *MGSQL* to the rescue! In this guide, we\'ll show you
how to take a standard, single-region Postgress database and transform
it into a global multi-region, fault-tolerant database without all the
usual overhead. It\'s multi-region made easy using the Postgres you know
and love.

To give our fictional project some basic guardrails, here are our
project requirements:

-   Create a higher fault tolerance and the ability to survive at least
    one failure
-   Provide linearizable results between our three main user hubs i.e
    Europe, North America and South-Africa
-   Provide low-latency results in our main user hubs

### The Database​

We are using the sample Postgress database from sqltutorial.org, but you
can use any other standard Postgress database.

### Prerequisites​

We assume you\'re familiar with the basic concepts of Seaplane Global
SQL, but if you need a quick refresher check out our documentation.

If you want to follow along, you\'ll need access to the *MGSQL* beta,
which you can request here

## Implementation​

Our example database consists of seven tables. The quickest and easiest
way to take advantage of *MGSQL\'s* global features is to turn our
example tables into global tables without any restrictions.

This automatically creates tables on at least five nodes in the Seaplane
network, and allows Seaplane to decide the best providers and locations
for you based on your end user traffic patterns. Basically, global
tables let Seaplane do all the heavy lifting for you.

<div>

<table><thead><tr><th><strong>Table Type</strong></th><th><strong>Fault Tolerance</strong></th><th><strong>Latency</strong></th></tr></thead><tbody><tr><td>Global</td><td>2 Failures</td><td>High latency writes; high tail latencies for reads depending on querier's region.</td></tr><tr><td>Regional</td><td>1 Failure</td><td>Low in target region. High elsewhere.</td></tr><tr><td>Row Level Geofence</td><td>1 Failure</td><td>Low for rows within the querier's region; high for rows outside the querier's region</td></tr></tbody></table>

</div>

However, observant readers might notice this creates a few problems with
our defined requirements. Global tables are great for fault tolerance,
but they don\'t provide linearizable results between regions, and there
is no easy way to control in-region latency performance. Regional
tables, however, do! They provide linearizable results in the provided
regions and are fault-tolerant to at least one failure. Exactly what we
need!

To turn our \"normal\" SQL tables into Seaplane regional SQL tables, all
we have to do is add three lines of code to our table create query.
Here\'s what that looks like for the `departments` table in our
database:

<div>

<div>

    CREATE TABLE departments (
        department_id SERIAL PRIMARY KEY,
        department_name CHARACTER VARYING (30) NOT NULL,
        location_id INTEGER,
        FOREIGN KEY (location_id) REFERENCES locations (location_id) ON UPDATE CASCADE ON DELETE CASCADE
    ) WITH (
        seaplane_table_type=regional,
        seaplane_regions='region/xe,region/xn, country/za'
    );

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

In the example above, we added two variables to our `with` class. The
first is the `seaplane_table_type`. As the name might suggest, this
controls the table type --- we selected the `regional` table type as the
most suitable for this use case.

The second argument sets the Seaplane region. In this instance, the
regions control the locations in which the database provides
linearizable results. For our example, we added `region/xe` for Europe,
`region/xn` for North America and `country/za` for South Africa. For a
complete list of locations have a look at our documenation.

Repeat the same steps for all tables to create a fully multi-region
database.

Once created, your database is accessible through a single URL
(`sql.cplane.cloud`) which selects the closest available node depending
on your location.

## Fault Tolerance​

In addition to improving regional performance, multi-region databases
are more fault tolerant in instances of failure. *MGSQL* in particular
has a high degree of fault tolerance right out of the box. How many
failures *MGSQL* can handle depends primarily on table type. Global
tables provide the highest degree of fault tolerance (all global tables
are deployed across a minimum of five separate failure domains) while
regional tables come in at a close second (all regional tables are
deployed across a minimum of three failure domains).

That said, *MGSQL* prioritizes consistency. If you\'re using a regional
table and two or more failures occur at the same time, your data might
become unavailable.

You can read more about fault tolerance in our documentation

## Disaster Recovery​

Just like fault tolerance, *MGSQL* has a high degree of disaster
recovery built in. If/when a disaster occurs, *MGSQL* will respond
automatically to get your database back in working order. For example,
let\'s say you have a regional table deployed in a German data center.
If that data center suddenly goes offline because an intern tripped over
an extension cord, then *MGSQL* will detect the outage and automatically
deploy a new node in a new German data center within a different failure
domain (while still respecting any geofences or restrictions). This will
restore the minimum number of required failure domains for each table
and ensure no interruption in service for you or your end users.

In the case of a regional table, if two or more data centers fail, the
database becomes temporarily unavailable.

The host where your database is accessible will always stay the same
throughout a disaster, e.g., `sql.cplane.cloud`. You can learn more
about disaster recovery in our documentation.

## In Review​

By turning all of our tables into Seaplane regional tables, we have
taken a single region database and expanded it into a multi-region
fault-tolerant database that provides linearizable results between our
main hubs (EU, US and SA).

The regional tables provide low in-region performance and can survive at
least one failure which significantly increases our fault-tolerance.

If you want to learn more about Seaplane Global SQL, or if you want to
try it yourself, reach out to us at support@seaplane.io!

</div>

<div>

<div>

**Tags:**

-   sql
-   global
-   multi region
-   postgress

</div>

</div>

<div>

<div>

</div>

<div>

Last updated on **Mar 16, 2023** by **fokkedekker**

</div>

</div>

</div>

</div>

</div>

</div>
