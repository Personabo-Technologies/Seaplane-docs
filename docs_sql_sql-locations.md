<div>

<div>

<div>

<div>

-   ![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)
-   Managed Global Database
-   Locations and Geofencing

<div>

<div>

# Locations and Geofencing

</div>

This document describes how to use locations and geofences to restrict
your data or create linearizable semantics between locations.

Across the various table types, *MGSQL* support two location attributes
`geo-fences` and `regions`.

-   We use `seaplane_region` to specify a given set of regions where
    *MGSQL* guarantees linearizable semantics. Adding a
    `seaplane_region` does not restrict data to the location.
-   We use `seaplane_geofence` to restrict data to one or more
    locations. When a `seaplane_geofence` is provided Seaplane will
    never store data of the restricted resource outside of the geofence.

Seaplane supports the following countries and grouped geo-regions.

<div>

<table><thead><tr><th>2-letter code</th><th>Full Name</th></tr></thead><tbody><tr><td>AE</td><td>United Arab Emirates</td></tr><tr><td>AU</td><td>Australia</td></tr><tr><td>BE</td><td>Belgium</td></tr><tr><td>BH</td><td>Bahrain</td></tr><tr><td>BR</td><td>Brazil</td></tr><tr><td>CA</td><td>Canada</td></tr><tr><td>CH</td><td>Switzerland</td></tr><tr><td>CL</td><td>Chile</td></tr><tr><td>DE</td><td>Germany</td></tr><tr><td>ES</td><td>Spain</td></tr><tr><td>FI</td><td>Finland</td></tr><tr><td>FR</td><td>France</td></tr><tr><td>GB</td><td>United Kingdom of Great Britain and Northern Ireland</td></tr><tr><td>HK</td><td>Hong Kong</td></tr><tr><td>ID</td><td>Indonesia</td></tr><tr><td>IE</td><td>Republic of Ireland</td></tr><tr><td>IN</td><td>India</td></tr><tr><td>IT</td><td>Italy</td></tr><tr><td>JP</td><td>Japan</td></tr><tr><td>KR</td><td>Korea, Republic of</td></tr><tr><td>NL</td><td>Netherlands</td></tr><tr><td>PL</td><td>Poland</td></tr><tr><td>RO</td><td>Romania</td></tr><tr><td>SE</td><td>Sweden</td></tr><tr><td>SG</td><td>Singapore</td></tr><tr><td>TW</td><td>Taiwan</td></tr><tr><td>US</td><td>United States of America</td></tr><tr><td>ZA</td><td>South Africa</td></tr><tr><td></td><td><strong>In addtion to countries the following grouped geo-regions are available</strong></td></tr><tr><td>XA</td><td>Asia</td></tr><tr><td>XC</td><td>People's Republic of China</td></tr><tr><td>XE</td><td>Europe</td></tr><tr><td>XF</td><td>Africa</td></tr><tr><td>XN</td><td>North America</td></tr><tr><td>XO</td><td>Oceania</td></tr><tr><td>XQ</td><td>Antartica</td></tr><tr><td>XS</td><td>South America</td></tr><tr><td>XU</td><td>The UK</td></tr></tbody></table>

</div>

</div>

<div>

<div>

**Tags:**

-   sql
-   locations
-   geofencing

</div>

</div>

<div>

<div>

![](data:image/svg+xml;base64,PHN2Zz48Zz48cGF0aD48L3BhdGg+PC9nPjwvc3ZnPg==)Edit
this page

</div>

<div>

Last updated on **Dec 22, 2022** by **fokkedekker**

</div>

</div>

</div>

</div>

</div>

</div>
