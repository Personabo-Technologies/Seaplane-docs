<div>

<div>

<div>

<div>

-   ![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)
-   Managed Global Database
-   Fault Tolerance

<div>

On this page

</div>

<div>

<div>

# Fault Tolerance

</div>

Seaplane Managed Global SQL (*MGSQL*) has a high degree of fault
tolerance built in. This support article explains how Seaplane manages
fault tolerance and how the various supported table types affect fault
tolerance, availability, and consistency.

## Consistency vs Availability​

*MGSQL* prioritizes consistency over availability. If \"enough\" data
centers suffer an outage, your table will become unavailable in order to
guarantee consistency. The number of failures each table can endure (as
in, how many data centers going down consitutes \"enough\") before
becoming unavailable depends on the table type.

## Table Types and Fault Tolerance​

The table type defines the number of failure domains where your table is
deployed which in turn impacts the fault tolerance and availability of
your data.

<div>

<table><thead><tr><th>Table type</th><th>Number of failure domains</th><th>Fault tolerance</th></tr></thead><tbody><tr><td>Global</td><td>5</td><td>Two failures</td></tr><tr><td>Regional</td><td>3</td><td>One failure</td></tr><tr><td>Regional by row</td><td>3 (in each region)</td><td>One failure</td></tr></tbody></table>

</div>

## Global Table Fault Tolerance​

Global tables provide the highest fault tolerance of all tables. They
are deployed over a minimum of five failure domains and can survive up
to two failures. During failures, *MGSQL* automatically routes the
traffic to the remaining available data centers. This could affect the
performance for local users. You can learn more about rerouting and
disaster recovery here.

To better understand how *MGSQL* handles outages, let\'s have a look at
an example. Assume you have the following table configured:

<div>

<div>

    CREATE TABLE table_name(...) WITH (
        seaplane_table_type=global
    );

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

This table has no `seaplane_geofence` restrictions, meaning *MGSQL* can
place the data anywhere on the planet to optimize for cost and
performance. Based on fictional user traffic, *MGSQL* decides to house
this table in Europe across five independent data centers. Disaster
strikes and one of the data centers suffers an outage.

Your database is unaffected, the remaining failure domains maintain a
majority and operate as usual.

A second data center suffers an outage. While the latency may increase
for some users as they need to connect to the next available resource
(which may be further away from them), the availability remains
unchanged.

But, as luck would have it, one more data center suffers an outage.

*MGSQL* prioritizes consistency over availability and as the remaining
data centers no longer have a majority, your table becomes unavailable
until *MGSQL* can redeploy your table across data centers in new failure
domains. To do so, *MGSQL* needs to re-establish contact with at least
one of the data centers as it needs a majority to bring up new nodes.
Alternatively, you could bring back your database from a snapshot. You
can learn more about bringing back snapshots here (coming soon).

### Regional Table Fault Tolerance​

Regional tables provide fault tolerance up to one failure. They have
three failure domains across the Seaplane network.

Let\'s assume you have the following table set up in *MGSQL*. The
provided regions in `seaplane_regions` guarantee linerizable results
between the US (`country/us`) and the EU (`region/xe`).

<div>

<div>

    CREATE TABLE table_name(...) WITH (
        seaplane_table_type=regional,
        seaplane_regions='region/xe,country/us'
    );

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

Adding the regions to the `seaplane_region` field does not guarantee the
failure domains are placed in these regions, but it does make it more
likely. For the sake of this example, let\'s assume *MGSQL* placed the
database across three independent data centers in the EU.

Disaster strikes once more and one of the data centers in the EU suffers
an outage. Regional tables can survive one disaster so your table\'s
availability is unaffected. However, the latency for some users could
increase until *MGSQL* can replace the downed data center.

As luck would have it, another data center suffers an outage at the same
time. The remaining data centers (one) no longer have a majority and
your table becomes unavailable until *MGSQL* can replace the affected
data centers. Before it can do so, *MGSQL* needs to reestablish contact
with one of the affected data centers as it needs a majority to bring up
new nodes. Alternatively, you could restore from a snapshot. You can
learn more about bringing back snapshots here (coming soon).

### Row-Level-Geofence Table Fault Tolerance​

Row-level-geofence tables provide fault tolerance up to one failure just
like regional tables. But unlike regional tables, they provide three
failure domains in each geofence. In practice, this means that they can
survive a single failure in a single geofence region or multiple
failures so long as those failures are spread between different regions.

Assume you have the following row-level-geofence table configured:

<div>

<div>

    CREATE TABLE table_name(...) WITH (
        seaplane_table_type=row_level_geofence,
        seaplane_geofence='rregion/xe,country/us,country/za'
    );

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

Theoretically, this table can survive three outages as long as each
outage occurs in a different region, i.e., one in the EU, one in the US,
and one in South Africa.

Data becomes unavailable if more than one outage occurs in a single
region. For example, if the EU suffers two outages, the EU data becomes
unavailable. The US and South African data, however, remain unaffected
and can be accessed.

## Geofences and Fault Tolerance​

Geofences can impact the disaster tolerance of your table. While they do
not directly influence the number of failure domains per table, setting
geofence rules that are too strict can affect *MGSQL\'s* ability to
redeploy a node during an outage. For example, if you create a table
that is geofenced to a small country with only a single data center, it
may be difficult or even impossible to find another resource. If that
data center becomes unavailable, so will your table.

</div>

<div>

<div>

**Tags:**

-   Fault Tolerance
-   FT

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
