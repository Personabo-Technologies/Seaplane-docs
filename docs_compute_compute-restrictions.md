<div>

<div>

<div>

<div>

-   ![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)
-   Managed Global Compute
-   Restrictions

<div>

On this page

</div>

<div>

<div>

# Restrictions

</div>

**Available through:** *CLI, Python SDK, Rust SDK, API*

This document details how to use restrictions with *Seaplane Global
Managed Compute* (*MGC*). It aims to explain the high-level concepts
around restrictions. For specific implementation examples have a look at
the API specification.

## Restriction Types​

Managed Global Compute supports three types of restrictions. In the
sections below we will go into more detail for each specific restriction
type.

-   Region restrictions
-   Provider restrictions
-   Architecture restrictions

There are three restriction modifiers available for each restriction
type.

1.  **Require** - Seaplane is required to deploy your workload here. For
    example, `require: region/xu` ensures your workload is always
    deployed in the UK.
2.  **Allow** - Seaplane is allowed to deploy your workload here. For
    example, `allow: region/xu` allows but does not guarantee a workload
    is deployed in the UK.
3.  **Deny** - Seaplane is not allowed to deploy your workload here. For
    example, `deny: region/xu` excludes the UK from the possible
    deployment locations.

<div>

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)caution

</div>

<div>

Setting too strict restrictions can impact the performance of your
application.

</div>

</div>

## Region Restrictions​

Region restrictions affect the regions or countries in which workloads
are *allowed*, *denied* or *required* to run. Seaplane uses Alpha-2
codes to identify regions. For more information about Seaplane regions
have a look at the regions documentation..

Managed Global Compute supports the following regions:

<div>

<table><thead><tr><th>2-letter code</th><th>Full Name</th></tr></thead><tbody><tr><td>AE</td><td>United Arab Emirates</td></tr><tr><td>AU</td><td>Australia</td></tr><tr><td>BE</td><td>Belgium</td></tr><tr><td>BH</td><td>Bahrain</td></tr><tr><td>BR</td><td>Brazil</td></tr><tr><td>CA</td><td>Canada</td></tr><tr><td>CH</td><td>Switzerland</td></tr><tr><td>CL</td><td>Chile</td></tr><tr><td>DE</td><td>Germany</td></tr><tr><td>ES</td><td>Spain</td></tr><tr><td>FI</td><td>Finland</td></tr><tr><td>FR</td><td>France</td></tr><tr><td>GB</td><td>United Kingdom of Great Britain and Northern Ireland</td></tr><tr><td>HK</td><td>Hong Kong</td></tr><tr><td>ID</td><td>Indonesia</td></tr><tr><td>IE</td><td>Republic of Ireland</td></tr><tr><td>IN</td><td>India</td></tr><tr><td>IT</td><td>Italy</td></tr><tr><td>JP</td><td>Japan</td></tr><tr><td>KR</td><td>Korea, Republic of</td></tr><tr><td>NL</td><td>Netherlands</td></tr><tr><td>PL</td><td>Poland</td></tr><tr><td>RO</td><td>Romania</td></tr><tr><td>SE</td><td>Sweden</td></tr><tr><td>SG</td><td>Singapore</td></tr><tr><td>TW</td><td>Taiwan</td></tr><tr><td>US</td><td>United States of America</td></tr><tr><td>ZA</td><td>South Africa</td></tr><tr><td></td><td><strong>In addtion to countries the following grouped geo-regions are available</strong></td></tr><tr><td>XA</td><td>Asia</td></tr><tr><td>XC</td><td>People's Republic of China</td></tr><tr><td>XE</td><td>Europe</td></tr><tr><td>XF</td><td>Africa</td></tr><tr><td>XN</td><td>North America</td></tr><tr><td>XO</td><td>Oceania</td></tr><tr><td>XQ</td><td>Antartica</td></tr><tr><td>XS</td><td>South America</td></tr><tr><td>XU</td><td>The UK</td></tr></tbody></table>

</div>

## Provider Restrictions​

Provider restrictions affect the providers that *MGC* is *allowed*,
*denied* or *required* to use. Managed Global Compute supports the
following providers:

<div>

<table><thead><tr><th>Full Name</th><th>Alias</th></tr></thead><tbody><tr><td>Amazon Web Services</td><td>aws</td></tr><tr><td>Microsoft Azure</td><td>azure</td></tr><tr><td>Digtial Ocean</td><td>digitalocean</td></tr><tr><td>Equinix</td><td>equinix</td></tr><tr><td>Google Cloud Platform</td><td>gcp</td></tr></tbody></table>

</div>

## Architecture Restrictions​

Architecture restrictions affect the architectures a deployment on *MGC*
can use. Currently we support `X86` and `ARM` workloads.

## Order of evaluation​

Restrictions are evaluated from negative to positive (*deny*, *allow*,
*require*). The default state i.e when no restrictions are in place is
*allow* everything. Below are two real-world examples to help you better
understand the order of evaluation.

### example 1​

Suppose you want to restrict a workload to run anywhere in Europe but
not in Germany. You can achieve this by setting up the following two
restrictions.

-   Deny `country/de` (Germany)
-   Allow `region/xe` (Europe)

In the order of evaluation Seaplane first looks for *deny*. There is one
that denies the workload to run in Germany. Step two Seaplane looks at
the *allow* restrictions. There is one that allows the workload to run
in the European Union. In the third and final step Seaplane looks at the
*require* restrictions. There are none.

The API request to launch a formation with these restrictions would look
something like this. For more specific use cases and the full API
reference have a look at the *MGC* documentation

<div>

<div>

    curl "https://compute.cplane.cloud/v1/formations/<FORMATION-NAME>" \
        --header "Authorization: Bearer ${TOKEN}" \
        --data '{ "flights": [
          {
            "name": "<FLIGHT-NAME>",
            "image": "<IMAGE-URL>",
            "architecture": [
              "amd64"
            ],
            "regions_denied" : [
                "country/de"
            ],
            "regions_allowed" : [
                "region/xe"
            ],
            "api_permission": false 
          }]}' \
        --request POST

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

### example 2​

Suppose you have a workload that needs to run in North-America, is
allowed to run in Europe but not allowed to run in the UK. You can
achieve this by setting up three restrictions.

-   Deny `region/xu` (UK)
-   Allow `region/xe` (Europe)
-   Require `region/xn` (North-America)

In the order of evaluation Seaplane first looks for *deny*. There is one
that denies the workload to run in the UK. Step two Seaplane looks at
the *allow* restrictions. There is one that allows the workload to run
in Europe. Finally, Seaplane looks at the *require* restrictions. There
is one that requires the workload to run in North-America.

The API request to launch a formation with these restrictions would look
something like this. For more specific use cases and the full API
reference have a look at the *MGC* documentation

<div>

<div>

    curl "https://compute.cplane.cloud/v1/formations/<FORMATION-NAME>" \
        --header "Authorization: Bearer ${TOKEN}" \
        --data '{ "flights": [
          {
            "name": "<FLIGHT-NAME>",
            "image": "<IMAGE-URL>",
            "architecture": [
              "amd64"
            ],
            "regions_denied" : [
                "country/xu"
            ],
            "regions_allowed" : [
                "region/xe"
            ],
            "regions_required" : [
                "region/xn"
            ],
            "api_permission": false 
          }]}' \
        --request POST

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

If you have specific questions about how to set up restrictions don\'t
hesitate to contact our support.

</div>

<div>

<div>

**Tags:**

-   managed global compute
-   compute
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
