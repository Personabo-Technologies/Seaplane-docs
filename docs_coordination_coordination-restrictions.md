<div>

<div>

<div>

<div>

-   ![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)
-   Global Coordination Service
-   Restrictions

<div>

On this page

</div>

<div>

<div>

# Restrictions

</div>

**Available through:** *CLI, Python SDK, Rust SDK, API*

This document describes how to set restrictions on providers or regions
used by the Seaplane Global Coordination Services (Coordination APIs)
including the Metadata Key-Value Store (Metadata KVS) and Locks.

By default, the Coordination APIs store data globally, across providers,
for the best end-user performance. With the restriction API, you can
override the default behavior and restrict the use of locations or
providers. For the best performance, we recommended using restrictions
only when absolutely necessary for regulatory purposes (GDPR compliance,
for example).

Note that having too many restrictions can impact performance. For
example:

-   Data restricted to a single country could increase latency for users
    across the globe.
-   Data restricted to a single cloud provider could become unavailable
    when that provider suffers an outage.

Restrictions are applied to a directory and are specific to an API. For
example, restrictions set to a directory called /prod/ in the Metadata
KVS do not affect any directory within the Locks service.

Restrictions can exist without the affected directory existing i.e., you
can set up restrictions before creating your directories. For example,
you can create a restriction on the `eu` directory to limit the
geographic location of the data in `eu` to the European Union. Creating
the directory and adding a key automatically applies the predefined
restrictions. Similarly, deleting a directory does not remove a
restriction.

For more information and code examples, see the restrictions API docs.

## Restriction State​

The restriction state indicates if a restriction is pending or enforced.
Introducing a new restriction takes some time as the data moves to
comply with the new rule. During this time, the restriction is
`pending`. Once completed, the restriction state is `enforced`.

## Supported Regions​

<div>

<table><thead><tr><th>2-letter code</th><th>Full Name</th></tr></thead><tbody><tr><td>AE</td><td>United Arab Emirates</td></tr><tr><td>AU</td><td>Australia</td></tr><tr><td>BE</td><td>Belgium</td></tr><tr><td>BH</td><td>Bahrain</td></tr><tr><td>BR</td><td>Brazil</td></tr><tr><td>CA</td><td>Canada</td></tr><tr><td>CH</td><td>Switzerland</td></tr><tr><td>CL</td><td>Chile</td></tr><tr><td>DE</td><td>Germany</td></tr><tr><td>ES</td><td>Spain</td></tr><tr><td>FI</td><td>Finland</td></tr><tr><td>FR</td><td>France</td></tr><tr><td>GB</td><td>United Kingdom of Great Britain and Northern Ireland</td></tr><tr><td>HK</td><td>Hong Kong</td></tr><tr><td>ID</td><td>Indonesia</td></tr><tr><td>IE</td><td>Republic of Ireland</td></tr><tr><td>IN</td><td>India</td></tr><tr><td>IT</td><td>Italy</td></tr><tr><td>JP</td><td>Japan</td></tr><tr><td>KR</td><td>Korea, Republic of</td></tr><tr><td>NL</td><td>Netherlands</td></tr><tr><td>PL</td><td>Poland</td></tr><tr><td>RO</td><td>Romania</td></tr><tr><td>SE</td><td>Sweden</td></tr><tr><td>SG</td><td>Singapore</td></tr><tr><td>TW</td><td>Taiwan</td></tr><tr><td>US</td><td>United States of America</td></tr><tr><td>ZA</td><td>South Africa</td></tr><tr><td></td><td><strong>In addtion to countries the following grouped geo-regions are available</strong></td></tr><tr><td>XA</td><td>Asia</td></tr><tr><td>XC</td><td>People's Republic of China</td></tr><tr><td>XE</td><td>Europe</td></tr><tr><td>XF</td><td>Africa</td></tr><tr><td>XN</td><td>North America</td></tr><tr><td>XO</td><td>Oceania</td></tr><tr><td>XQ</td><td>Antartica</td></tr><tr><td>XS</td><td>South America</td></tr><tr><td>XU</td><td>The UK</td></tr></tbody></table>

</div>

## Supported Providers​

<div>

<table><thead><tr><th>Full Name</th><th>Alias</th></tr></thead><tbody><tr><td>Amazon Web Services</td><td>aws</td></tr><tr><td>Microsoft Azure</td><td>azure</td></tr><tr><td>Digtial Ocean</td><td>digitalocean</td></tr><tr><td>Equinix</td><td>equinix</td></tr><tr><td>Google Cloud Platform</td><td>gcp</td></tr></tbody></table>

</div>

</div>

<div>

<div>

**Tags:**

-   getting started
-   global coordination service
-   key-value
-   restrictions

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
