<div>

<div>

<div>

<div>

-   ![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)
-   Managed Global Database
-   Managed Global Database Introduction

<div>

On this page

</div>

<div>

<div>

# Managed Global Database Introduction

</div>

*Seaplane Managed Global SQL* (*MGSQL*) is Seaplane\'s global relational
database offering based on Postgres 11. *MGSQL* functions like any other
Postgres database, meaning it is fully wire compatible with Postgres
11.2, but with some powerful additions.

*MGSQL* provides you with a singular entry point `sql.cplane.cloud` that
routes to the nearest available cluster relative to where you are in the
world. You can learn more about connecting to *MGSQL* here.

## Powerful Features for Global Users​

*MGSQL* extends standard Postgres with powerful features designed for
global users and end users. Like all Seaplane products, *MGSQL* deploys
your database globally on the best combination of edge, bare metal, and
public cloud resources. *MGSQL* even examines your end-user traffic
patterns to determine the best deployment locations based on cost and
performance.

However not all data can or should be accessible everywhere, so *MGSQL*
also allows you to restrict data locations by table or by row to comply
with data privacy regulations. Deployments will still optimize your
infrastructure mix for you, but only within the geofenced locations you
define. You can learn more about geofencing, applying business rules,
and *MGSQL\'s* various table types here.

### Multi-Region and Multi-Cloud​

By default, *MGSQL* deploys your database globally across clouds and
edge, automatically picking and choosing resources and locations to
minimize latency for your end users. *MGSQL* currently supports over 35
locations which you can read about here

## Global Database, World-Class Performance​

\"Global\" might imply \"slow\" (there is a lot of ground to cover) but
*MGSQL* does not compromise on performance. We tested the database with
the following queries using Sysbench from hosts operated on AWS in the
EU and Japan:

<div>

<div>

    -- insert
    INSERT INTO users (username, email, address, seaplane_geofence) VALUES ('generated-name', 'generated-email', 'address', '<country/jp or region/xe>');

    -- select count
    SELECT COUNT(*) FROM users WHERE seaplane_geofence='<country/jp or region/xe>';

    -- select string
    SELECT * FROM users WHERE username LIKE '%john%' AND seaplane_geofence='<country/jp or region/xe>';

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

We ran each query at least 1000 times. Each query consistently scored
well below 25ms for the 95th percentile.

<div>

<table><thead><tr><th>query</th><th>user location location</th><th>db host location</th><th># of queries</th><th>avg latency</th><th>95th percentile</th></tr></thead><tbody><tr><td>insert</td><td>EU</td><td>EU</td><td>1392</td><td>21.56</td><td>21.89</td></tr><tr><td>select point</td><td>EU</td><td>EU</td><td>1199</td><td>25.03</td><td>24.74</td></tr><tr><td>select count</td><td>EU</td><td>EU</td><td>1629</td><td>18.42</td><td>18.95</td></tr><tr><td>select string</td><td>EU</td><td>EU</td><td>1632</td><td>18.39</td><td>18.95</td></tr><tr><td>insert</td><td>JP</td><td>JP</td><td>1126</td><td>26.65</td><td>17.95</td></tr><tr><td>select point</td><td>JP</td><td>JP</td><td>1394</td><td>21.52</td><td>21.11</td></tr><tr><td>select count</td><td>JP</td><td>JP</td><td>1887</td><td>15.9</td><td>15.83</td></tr><tr><td>select string</td><td>JP</td><td>JP</td><td>1347</td><td>22.28</td><td>21.89</td></tr></tbody></table>

</div>

## Get Access​

*MGSQL* is currently in beta, but you can request early access here

## Use Cases​

-   Data Regulation Compliance with Seaplane SQL
-   Going multi-region with Seaplane Managed Global SQL (Coming Soon!)

</div>

<div>

<div>

**Tags:**

-   sql
-   introduction
-   managed global database

</div>

</div>

<div>

<div>

![](data:image/svg+xml;base64,PHN2Zz48Zz48cGF0aD48L3BhdGg+PC9nPjwvc3ZnPg==)Edit
this page

</div>

<div>

Last updated on **Mar 16, 2023** by **fokkedekker**

</div>

</div>

</div>

</div>

</div>

</div>
