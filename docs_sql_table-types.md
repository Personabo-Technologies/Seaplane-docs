<div>

<div>

<div>

<div>

-   ![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)
-   Managed Global Database
-   Table Types

<div>

On this page

</div>

<div>

<div>

# Table Types

</div>

*Seaplane Managed Global SQL* (*MGSQL*) supports three special table
types that give you control of where your data lives. This document
describes how to use them and the distinct benefits of each one.

Table types are set during creation and can not be modified. As such, we
encourage you to think ahead and plan accordingly. Our team of experts
is always available at support@seaplane.io if you need any help.

You can use the cheat sheet below to help you decide which table type to
use.

<div>

<table><thead><tr><th><strong>Table Type</strong></th><th><strong>Fault Tolerance</strong></th><th><strong>Latency</strong></th><th><strong>When to use?</strong></th></tr></thead><tbody><tr><td>Global</td><td>2 Failures</td><td>High latency writes; high tail latencies for reads depending on querier's region.</td><td>Data that requires high availability</td></tr><tr><td>Regional</td><td>1 Failure</td><td>Low in target region. High elsewhere.</td><td>Data that requires low-latency responses and is generally accessed from specific regions</td></tr><tr><td>Row level geofence</td><td>1 Failure</td><td>Low for rows within the querier's region; high for rows outside the querier's region</td><td>Data that requires low-latency responses and row-level geofencing</td></tr></tbody></table>

</div>

## Global Table​

The *Global Table* provides the highest fault tolerance of all table
types. *Global tables* have a minimum of five nodes across the Seaplane
network, but the increase in nodes comes at the cost of high write
latencies and high tail latencies for reads.

You can geofence *Global Tables* for compliance purposes using the
`seaplane_geofence` attribute. Adding a geofence guarantees that the
tables stay inside the geofenced region. To learn more about geofencing
have a look at the MGSQL geofencing documentation. Seaplane
automatically moves the tables around (within the specified geofence)
based on end-user traffic patterns to improve global performance.

You can create a *Global Table* as follows using a storage parameter:

<div>

<div>

    CREATE TABLE table_name(...) WITH (
        seaplane_table_type=global,
        seaplane_geofence='country/us,region/xe,country/jp'
    );

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

## Regional Table​

*Regional Tables* provide better in-region performance than *Global
Tables* but at the expense of availability. They always have a minimum
of three nodes deployed on the Seaplane network.

For *Regional Tables*, you need to specify at least one region. Regions
are specified using the `seaplane_regions` attribute. *Regional Tables*
provide linearizable semantics within the given set of regions.

You can geofence *Regional Tables* for compliance purposes using the
`seaplane_geofence` attribute. Adding a geofence guarantees that the
tables stay inside the geofenced region. Without a geofence, *MGSQL*
might move tables around the globe to better match end-user traffic
patterns and increase performance. To learn more about currently
available geofencing locations, have a look at the MGSQL geofencing
documentation.

<div>

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)note

</div>

<div>

-   We use `seaplane_region` to specify a given set of regions where
    *MGSQL* guarantees linearizable semantics. Adding a
    `seaplane_region` does not restrict data to the location.
-   We use `seaplane_geofence` to restrict data to one or more
    locations. When a `seaplane_geofence` is provided Seaplane will
    never store data of the restricted resource outside of the geofence.

</div>

</div>

You can create a *Regional Table* as follows using a storage parameter:

<div>

<div>

    CREATE TABLE table_name(...) WITH (
        seaplane_table_type=regional,
        seaplane_regions='region/xe,country/de'
        seaplane_geofence='country/us,region/xe'
    );

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

## Row-Level-Geofence Table​

*Row-level-geofence tables* provide row-level geofencing of your data.
You\'ll need to indicate the available locations using the
`seaplane_geofence` parameter on table creation. For each new row, you
indicate where your data should reside. For example, in the table below
data in the row with `ID=1` and `ID=2` resides in Japan, and data in the
row with `ID=3` resides in Europe. To learn more about currently
available geofence locations, have a look at the geofence documentation.

<div>

<table><thead><tr><th>ID</th><th>...</th><th>...</th><th>seaplane_geofence</th></tr></thead><tbody><tr><td>1</td><td>...</td><td>...</td><td>country/jp</td></tr><tr><td>2</td><td>...</td><td>...</td><td>country/jp</td></tr><tr><td>3</td><td>...</td><td>...</td><td>region/xe</td></tr></tbody></table>

</div>

Seaplane automatically creates a partitioned table for each
*row-level-geofence table* in the specified location upon creation.

You can create a row_level_geofence table as follows using a storage
parameter:

<div>

<div>

    CREATE TABLE table_name(...) WITH (
        seaplane_table_type=row_level_geofence,
        seaplane_geofence='region/xe,country/jp'
    );

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

### Row-Level-Geofence Primary Keys​

*Row-level-Geofence* tables are partitioned tables. This means that you
need to add the `seaplane_geofence` column to any of the unique columns
of the table, including the primary key. You can create a primary key on
a *row-level-geofence* table as follows.

<div>

<div>

    CREATE TABLE table_name (
        id BIGSERIAL,
        ...
        CONSTRAINT primary_key PRIMARY KEY (id, seaplane_geofence)
        ) WITH (
            seaplane_table_type=row_level_geofence,
            seaplane_geofence='region/xe, country/bh'
        );

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

Similarly, add the partition expression (`seaplane_geofence`) for any
unique column. Keep in mind that duplicate values can exist in the
various geo-fenced regions. However, the combination of geo-fence and
column value are guaranteed to be unique.

<div>

<div>

    CREATE TABLE table_name (
        column_name TEXT, 
        ...
        CONSTRAINT unique_column_name UNIQUE (column, seaplane_geofence)
        ) WITH (
            seaplane_table_type=row_level_geofence,
            seaplane_geofence='region/xe, country/bh'
        );

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

## Examples​

These are fairly abstract concepts, so let\'s take a look at some
examples to better understand table types and how they intersect with
the `seaplane_region` and `seaplane_geofence` parameters.

<div>

<div>

    CREATE TABLE table_name(...) WITH (
        seaplane_table_type=global,
    );

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

Starting with something simple, the code above creates a *Global Table*
without any restrictions. *MGSQL* can (and will) place this table
anywhere in the world to best match end-user traffic patterns. This
table has a minimum of 5 nodes and provides strong fault tolerance.

<div>

<div>

    CREATE TABLE table_name(...) WITH (
        seaplane_table_type=global,
        seaplane_geofence='region/xe'
    );

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

This example is similar to the table we created in our first example,
but with one key difference. Setting the `seaplane_geofence` parameter
to `region/xe` means your data never leaves Europe.

<div>

<div>

    CREATE TABLE table_name(...) WITH (
        seaplane_table_type=regional,
        seaplane_regions='country/jp,country/de'
    );

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

The table above provides linearizable semantics within Germany
(`country/de`) and Japan (`country/jp`). However, *MGSQL* is not
restricted from placing data outside of those regions to better match
end-user traffic patterns.

<div>

<div>

    CREATE TABLE table_name(...) WITH (
        seaplane_table_type=regional,
        seaplane_regions='country/jp,country/de',
        seaplane_geofence='country/jp,country/de'
    );

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

The table above provides linearizable semantics within Germany
(`country/de`) and Japan (`country/jp`) and data will never reside
outside Germany or Japan.

<div>

<div>

    CREATE TABLE table_name(...) WITH (
        seaplane_table_type=row_level_geofence,
        seaplane_geofence='region/xe, country/jp'
    );

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

The code above creates a row*level_geofence table where each row is
restricted to either Europe or Japan depending on the value in the
`seaplane_geofence` column. \_MGSQL* does not place any data outside
these regions.

You can add a record to the table you just created by running the
following query. This new data point is restricted to Japan indicated by
`country/jp`. To add the record to Europe instead, replace `country/jp`
with `region/xe`.

<div>

<div>

    INSERT INTO table_name (column1, column2, seaplane_geofence) VALUES ('column1-value', 'column2-value', 'country/jp');

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

</div>

<div>

<div>

**Tags:**

-   sql
-   table types
-   regional table
-   regional by row table

</div>

</div>

<div>

<div>

![](data:image/svg+xml;base64,PHN2Zz48Zz48cGF0aD48L3BhdGg+PC9nPjwvc3ZnPg==)Edit
this page

</div>

<div>

Last updated on **Jan 3, 2023** by **Fokke Dekker**

</div>

</div>

</div>

</div>

</div>

</div>
