<div>

<div>

<div>

<div>

-   ![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)
-   Managed Global Compute
-   Managed Global Compute Terminology
-   What are formations?

<div>

On this page

</div>

<div>

<div>

# What are formations?

</div>

When using *Seaplane Global Managed Compute* (*GMC*) there are two key
terms you need to know: *flights* and *formations*. This document dives
into the technical details of *formations*. You can learn more about
*flights* here.

## Flights vs Formations​

Before we dive into the specific details of flights, it\'s important to
understand the difference between a *flight* and a *formation*.

A *flight* consists of a single container image. Under the hood,
Seaplane can spin up many container instances and spread them around the
globe or within the regions you specify (more on that later).

A *formation* consists of one or more flights. *Formations* can be
thought of as your high-level application or service. Formations include
the load balancer and entry-point (resource URL) to your service. This
is the entry point for all public and private traffic.

## Formations​

*Formations* are heart of your application when deployed on Seaplane.
They include your high-level application, load-balancer, and routing all
in one. Each formation has at least one resource URL that serves as the
entry point to your application for external traffic.

Resource URLs have the following structure:
`formaton-name.tenant-name.on.cplane.cloud`.

*Formations* can be made up of multiple *flights*. This ensures that all
flights within a formation adhere to the same set of rules defined by
that formation. For example, you can create a single formation with a
set of rules that ensures GDPR compliance. Any flight inside that
formation will adhere to the same high-level definitions and
restrictions defined by the formation.

### Formation-Flight Routing​

You can set the routing to each flight inside a single formation in the
formation settings.

For example:

-   `formaton-name.tenant-name.on.cplane.cloud/formation-a` routes to
    `flight A`.
-   `formaton-name.tenant-name.on.cplane.cloud/formation-b` routes to
    `flight B`.

Suppose you operate in the European Union and you want to prevent your
team members from deploying non-GDPR-compliant workloads. You can set up
a single GDPR-compliant formation and ask all engineers to deploy new
workloads to that formation.

A deployment would consist of three steps.

-   Create new GDPR-compliant formation with new flights
-   Deploy new formation
-   Destroy old non-compliant formation

Routing to individual flights is not supported yet, but it will be
available very soon. You can request early access here

#### Restrictions​

Restrictions are set on a formation level. This includes region
restrictions as well as provider restrictions. To learn more about
restrictions have a look at the compute restrictions documentation.

</div>

<div>

<div>

**Tags:**

-   formations
-   compute
-   terminology
-   compute-terminolog

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
