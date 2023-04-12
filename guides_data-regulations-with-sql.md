<div>

<div>

<div>

<div>

-   ![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)
-   Data Regulation Compliance with Seaplane SQL

<div>

On this page

</div>

<div>

<div>

# Data Regulation Compliance with Seaplane SQL

</div>

## Introduction​

In this guide, we\'ll walk you through using the Seaplane Managed Global
Database to comply with current and future data regulations like the
GDPR, CCPA, and PIPEDA. At the end of this guide, you\'ll have
everything you need to set up your first multi-region database with
Seaplane.

If you want to skip ahead, you can check out the complete table
structure in the toggle below. That said, we encourage everyone (even
experts!) to keep reading and learn more about our team\'s design
decisions and use cases.

Complete Database Structure

<div>

<div>

<div>

<div>

    -- create the stores table - row_level_geofence
    CREATE TABLE stores (
    id BIGSERIAL,  
    name TEXT,
    address TEXT,
    phone_number TEXT,
    country TEXT,
    creation_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    CONSTRAINT stores_pk  PRIMARY KEY (id,seaplane_geofence)
    ) with (
          seaplane_table_type=row_level_geofence, 
          seaplane_geofence='country/us, region/xe'
      );

    -- create the users table - row_level_geofence
    CREATE TABLE users (
    id BIGSERIAL,
    username TEXT,
    email TEXT,
    address TEXT,
    creation_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    CONSTRAINT users_pk  PRIMARY KEY (id,seaplane_geofence)
    ) with (
          seaplane_table_type=row_level_geofence, 
          seaplane_geofence='country/us, region/xe'
      );

    -- create payment table - row_level_geofence
    CREATE TABLE payment_info (
    id BIGSERIAL,
    fk_users uuid,
    cc_number TEXT,
    ccv INT,
    cc_expiration DATE,
    creation_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    billing_zipcode TEXT,
    CONSTRAINT payment_info_pk  PRIMARY KEY (id,seaplane_geofence)
    ) with (
          seaplane_table_type=row_level_geofence, 
          seaplane_geofence='country/us, region/xe'
      );

    -- create orders table  - row_level_geofence
    CREATE TABLE orders (
    id BIGSERIAL,
    fk_users uuid,
    order_date TIMESTAMP,
    fk_stores uuid,
    CONSTRAINT order_pk  PRIMARY KEY (id,seaplane_geofence)
    ) with (
          seaplane_table_type=row_level_geofence, 
          seaplane_geofence='country/us, region/xe'
      );

    -- create order items table - row_level_geofencew
    CREATE TABLE order_items (
    id BIGSERIAL,
    fk_orders uuid,
    fk_stores uuid,
    fk_available_items uuid,
    quantity INT,
    CONSTRAINT order_item_pk  PRIMARY KEY (id,seaplane_geofence)
    ) with (
          seaplane_table_type=row_level_geofence, 
          seaplane_geofence='country/us, region/xe'
      );

    -- create available items table - global
    CREATE TABLE available_items (
    id BIGSERIAL PRIMARY KEY,
    name TEXT,
    price_eu FLOAT,
    price_US FLOAT
    ) WITH (seaplane_table_type=global);

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

</div>

</div>

## The Scenario​

For this guide, we\'ll be using a hypothetical scenario to best
highlight how Seaplane can be used to comply with global data laws.

*Company \"Good Dairy Products Reno\", runs a milk business serving
customers in the United States, but they have plans to expand their
business into Europe. They want to keep things as simple as possible
while still complying with local European data regulations, specifically
the General Data Protection Regulation (GDPR). Luckily for them, and for
us, Good Dairy is using the Seaplane Managed Global Database to create a
multi-region database that is both resilient, fast, and compliant with
all applicable data regulations*

<div>

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)caution

</div>

<div>

This guide does not constitute legal advice! You should always consult a
lawyer or other legal expert for advice on complying with international
data laws.

</div>

</div>

**Requirements**

The requirements for this project include, in no particular order:

1.  All data must remain available in the event of a single availablity
    zone failure.
2.  US data can be stored anywhere.
3.  European data can only be stored in the European Union.
4.  Data needs to be flexible and able to move from one region to
    another.
5.  Low complexity, similar to a normal (non-global) Postgress.
6.  Low latency queries, sub 50ms at least.

## Availability​

When designing your database, it\'s important to think about data
availability from the outset. How important is your data? What happens
during an outage? Maybe you\'re fine with some downtime, but because
you\'re considering Seaplane as a solution, you probably want to do
better than that! ;)

<div>

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)info

</div>

<div>

Seaplane supports two types of regional data. *Regional Tables* and
*row-level-geofence*.

*Regional Tables* --- as the name might suggest --- provide linearizable
semantics within a given set of regions. When only 1 region is
specified, they provide low-latency access within that region. Tables
with *row-level-geofence* data are split over multiple regions where
each row lives in its specified region.

Seaplane automatically optimizes where *Regional_Tables* are deployed
based on end user demand. This can dramatically improve your
application\'s performance, though you do lose some of the flexibility
of row-level restrictions

</div>

</div>

The table type you choose will impact the availability of your data.
Based on the requirements we previously discussed, a
*row-level-geofence* table is the best choice for Good Dairy based on
their requirements. Don\'t worry though, we\'ll go into more detail for
each requirement below.

While we use row-level-geofence tables for the majority of this project,
there is one Regional Table in our schema. The `available_products`
table does not contain any personally identifiable information (PII), so
we don't need to worry about data localization on a row-by-row basis.
Instead, we'll just make the available_products table a Regional Table
and tell Seaplane where we generally expect requests to originate (US
and Europe, in this example). Seaplane then automatically deploys the
table in the specified regions, but may move the data anywhere in the
world to match end user demand. Being able to optimize data location for
better performance is a huge benefit of using Seaplane, but because
Regional Tables --- without restrictions --- have the ability to move
freely, they aren't a good choice for tables with PII or any data that
needs to stay within a certain region or regulatory zone.

<div>

<table><thead><tr><th><strong>Table Type</strong></th><th><strong>Fault Tolerance</strong></th><th><strong>Latency</strong></th></tr></thead><tbody><tr><td>Global</td><td>2 Failures</td><td>High latency writes; high tail latencies for reads depending on querier's region.</td></tr><tr><td>Regional</td><td>1 Failure</td><td>Low in target region. High elsewhere.</td></tr><tr><td>Row Level Geofence</td><td>1 Failure</td><td>Low for rows within the querier's region; high for rows outside the querier's region</td></tr></tbody></table>

</div>

<div>

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)tip

</div>

<div>

-   We recommend *global* tables for data that requires high
    availability
-   We recommend *regional* tables for data that requires low-latency
    responses and is generally accessed from specific regions
-   We recommend *row-level-geofence* tables for data that requires
    low-latency responses and row-level geofencing

</div>

</div>

Storage restrictions can be set on the *global* and *regional* tables
via the *seaplane_geofence* parameter. When this parameter is set,
Seaplane guarantees that we will only store that table\'s data within
the geofenced zone/region. *seaplane_geofence* is not necessary for
*row-level-geofence* tables, as rows are automatically geofenced based
on the value of the *seaplane_geofence* column.

## Database Design and Regionality​

The database consists of six tables. The diagram below provides an
overview of all our tables and their relationships.

As we said earlier, we\'ll be using a *row-level-geofence* table. The
*row-level-geofence* table has a few distinct benefits over *Regional
Tables* that better serve Good Dairy\'s specific requirements.

-   Single tables are easier to maintain *(requirement 5)*
-   Better portability of data in case it moves from one region to
    another *(requirement 4)*
-   More flexibility to create new regions without the need to create
    new tables
-   Only one table to query
-   Low query response time for rows in the target region *(requirement
    6)*

<div>

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)note

</div>

<div>

In our example, we\'re taking \"anywhere\" to mean the US. However,
we\'re working on adding \"everywhere\" or `*` as an option to the
Seaplane Managed Global Database. This will allow the SQL service to
find the best possible location, globally, based on user demand. Fewer
restrictions almost always mean better performance. If you are
interested in the everywhere *row-level-geofence* feature, let us know
by emailing support@seaplane.io for early access.

</div>

</div>

To create a *row-level-geofence* table in Seaplane, all we have to do is
specify the table type and the regions it should support at the end of
create query. In this example, we use `country/us` for the US and
`region/xe` for Europe. For a complete overview of all Seaplane regions
have a look at the Seaplane regions documentation.

<div>

<div>

    with (
            seaplane_table_type=row_level_geofence, 
            seaplane_geofence='country/us, region/xe'
        );

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

A complete create query will look something like this. Note that
seaplane_geofence has to be part of the primary key. More on that later.

<div>

<div>

    CREATE TABLE stores (
      id BIGSERIAL,
      name TEXT,
      addres TEXT,
      phone_number TEXT,
      country TEXT,
      creation_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
      CONSTRAINT restaurant_pk  PRIMARY KEY (id ,seaplane_geofence)
    ) with (
            seaplane_table_type=row_level_geofence, 
            seaplane_geofence='country/us, region/xe'
        );

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

For each *row-level-geofence* table, Seaplane automatically creates a
partitioned table and one table with the regional data in each specified
region.

<div>

<div>

                            List of relations
     Schema |            Name             |       Type        | Owner
    --------+-----------------------------+-------------------+-------
     public | seaplane_stores_us          | table             | fokke
     public | seaplane_stores_xe          | table             | fokke
     public | stores                      | partitioned table | fokke

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

### Using IDs As Primary Keys​

For multi-region tables, like the example above, we have to include the
`seaplane_geofence` as part of the primary key (PK). We use a composite
key consisting of `seaplane_geofence` and `id` to ensure unique primary
keys.

<div>

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)caution

</div>

<div>

By following this guide we can create GDPR-compliant tables, however
it\'s important to remember that you as the developer are still
responsible for associating each row with the correct region. However,
this does not limit engineers from running queries across regions, which
may break GDPR restrictions. In other words, make sure you have all the
necessary data compliance policies and processes in place for your
developers to follow.

</div>

</div>

## Data Portability​

One of Good Dairy\'s requirements is that data needs to be portable.
Fortunately, moving regions is easy! If you want to move from one region
to another, all you have to do is update the `seaplane_geofence` field
for the affected rows. Once updated, Seaplane automatically moves the
data to the desired region. For example, the query below moves user with
`id` 1 from the US to Europe. This satisfies *requirement 4*

<div>

<div>

    UPDATE users SET seaplane_geofence = 'region/xe' WHERE id = 1;

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

However, user data is often spread out over multiple tables. In order to
move all associated data, be sure to move all records that are linked to
our user record through the PK/FK relations.

## Low Complexity​

The Seaplane Managed Global Database is fully wire-compatible with
PostgresSQL v11.2 and is generally syntax/feature compatible. While
there is some added complexity in choosing table types and deciding
where data should live, the overall complexity is minimal.

Knowing that, we\'ll go ahead and categorize the overall added
complexity as negligible, satisfying *requirement 5*. If you\'re
skeptical (no hard feelings), we encourage you to try it for yourself!

## Low-latency Query Responses --- Sub 50ms​

Seaplane regional tables support low-latency query responses. We tested
the tables created in this example from within Europe and the US. As
expected, US rows perform well in the US, and European rows perform well
in Europe.

We tested the database with the following queries using Sysbench from
hosts operated on AWS in the EU and the US:

<div>

<div>

    -- insert
    INSERT INTO users (username, email, address, seaplane_geofence) VALUES ('generated-name', 'generated-email', 'address', '<country/us or region/xe>');

    -- select count
    SELECT COUNT(*) FROM users WHERE seaplane_geofence='<country/us or region/xe>';

    -- select string
    SELECT * FROM users WHERE username LIKE '%john%' AND seaplane_geofence='<country/us or region/xe>';

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

We ran each query at least 1000 times. Predictably, the database
performed well (sub 50ms) in the required regions with the 95th
percentile scoring well below 25ms in most cases.

<div>

<table><thead><tr><th>query</th><th>user location location</th><th>db host location</th><th># of queries</th><th>avg latency</th><th>95th percentile</th></tr></thead><tbody><tr><td>insert</td><td>EU</td><td>EU</td><td>1392</td><td>21.56</td><td>21.89</td></tr><tr><td>select point</td><td>EU</td><td>EU</td><td>1199</td><td>25.03</td><td>24.74</td></tr><tr><td>select count</td><td>EU</td><td>EU</td><td>1629</td><td>18.42</td><td>18.95</td></tr><tr><td>select string</td><td>EU</td><td>EU</td><td>1632</td><td>18.39</td><td>18.95</td></tr><tr><td>insert</td><td>US</td><td>US</td><td>1126</td><td>26.65</td><td>17.95</td></tr><tr><td>select point</td><td>US</td><td>US</td><td>1394</td><td>21.52</td><td>21.11</td></tr><tr><td>select count</td><td>US</td><td>US</td><td>1887</td><td>15.9</td><td>15.83</td></tr><tr><td>select string</td><td>US</td><td>US</td><td>1347</td><td>22.28</td><td>21.89</td></tr></tbody></table>

</div>

## Summary​

Let\'s take a moment to review. *Good Dairy Products Ryūgasaki* asked us
to build a GDPR-compliant database that was fast, easy to use, and
provided a GDPR-compliant structure.

To fulfill those requirements, we used the Seaplane Managed Global
Database to create *row-level-geofence* tables that allow our users to
comply with every relevant data regulation without compromising on speed
(sub 50ms queries), availability (1 failure fault tolerance), or
unnecessary complexity (fully Postgres compatible with minor additions).

If you\'re ready to try Seaplane for yourself, you can request access
here or, if you still have questions, feel free to email our support at
support@seaplane.io

Thank you for flying with Seaplane!

<div>

<div>

</div>

</div>

</div>

<div>

<div>

**Tags:**

-   sql
-   data
-   gdpr
-   data regulations

</div>

</div>

<div>

<div>

</div>

<div>

Last updated on **Mar 29, 2023** by **fokkedekker**

</div>

</div>

</div>

</div>

</div>

</div>
