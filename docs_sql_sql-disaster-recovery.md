<div>

<div>

<div>

<div>

-   ![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)
-   Managed Global Database
-   Disaster Recovery

<div>

On this page

</div>

<div>

<div>

# Disaster Recovery

</div>

This support article explains the concept of disaster recovery and how
it relates to Seaplane Managed Global SQL (*MGSQL*).

While we\'re going to use the term \"disaster recovery\" for the sake of
clarity, here at Seaplane a better term for how we manage failures and
outages is probably \"disaster resistance.\"

What we mean by \"disaster resistance\" is that the user (you) should
not be responsible for handling failures. In fact, we don\'t think you
should experience any disaster at all. Instead, we\'ve built *MGSQL* to
be capable of handling disaster recovery for you, automatically, so you
don\'t have to worry about manually responding to outages. An ounce of
prevention is worth a pound of cure, after all!

## Database URL​

The central access point to your database (`sql.cplane.cloud`) never
changes. While *MGSQL* might be busy in the background ensuring the
availability of your databases, users never have to update the database
URL. During outages, *MGSQL* routes traffic to the next available
resources without requiring you to update your DNS or your applications.
One less thing to worry about!

## Rerouting Traffic​

As we mentioned previously, *MGSQL* routes all requests through a single
access point (`sql.cplane.cloud`) to the closest available resources. If
an outage occurs, new requests are automatically routed to the closest
remaining resource.

Depending on the location of your data, this can increase latency and
overall speed for two reasons. First, the latency can increase due to
the increased distance between the requester and the database node.
Second, the overall speed of the database could be affected due to the
increased load on the available nodes.

The recovery time objective (RTO) for all table types is approximately
15 seconds.

## Redeploying Nodes​

If an outage occurs, *MGSQL* automatically redeploys in new failure
domains until the minimum required number of failure domains for each
table is satisfied. For example, if you have a database with a global
table, *MGSQL* ensures a minimum of five available failure domains at
all times. For regional and row-level-geofence tables that number is
three.

The time needed to redeploy to a new failure domain during an outage is
dependent on the size of your database, but is generally in the order of
five minutes.

*MGSQL* can and will redeploy to new failure domains (data centers)
during outages as long as the remaining data centers have a majority.
For example, for regional and row-level-geofence tables that means two
out of the three failure domains should be available. For global tables
that means three out of five failure domains should be available.

If the remaining data centers do not have a majority, *MGSQL* needs to
re-establish contact with one of the downed data centers. Alternatively,
you could bring up a new data center from a snapshot. You can learn more
about bringing back snapshots here (coming soon).

</div>

<div>

<div>

**Tags:**

-   SQL
-   Disaster Recovery
-   DR

</div>

</div>

<div>

<div>

![](data:image/svg+xml;base64,PHN2Zz48Zz48cGF0aD48L3BhdGg+PC9nPjwvc3ZnPg==)Edit
this page

</div>

<div>

Last updated on **Mar 1, 2023** by **Paige**

</div>

</div>

</div>

</div>

</div>

</div>
