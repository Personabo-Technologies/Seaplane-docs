<div>

<div>

<div>

<div>

-   ![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)
-   Managed Global Compute
-   Managed Global Compute Introduction

<div>

On this page

</div>

<div>

<div>

# Managed Global Compute Introduction

</div>

**Available through:** *CLI, Python SDK, Rust SDK, API*

## What is Seaplane Managed Global Compute​

Seaplane Managed Global Compute (MGC) is Seaplane\'s product to deploy
containerized workloads. However, it\'s important to point out that MGC
includes a ton of additional services beyond global compute.

Just like all of Seaplane\'s products, MGC enables users to access
global infrastructure while managing unnecessary infrastructure
complexity for you. Deploying any workload on MGC automatically deploys
that workload on the best available resources spread across
public-cloud, bare metal and edge resources.

Seaplane handles everything needed to make that workload available to
the world. This includes routing, load-balancing and disaster tolerance
features --- such as automatic rescheduling in instances of cloud
outages.

Seaplane supports the use of any standard OCI image. Container images
may be hosted on the Seaplane container registry or any standard
container registry of your choice.

## Automated routing and load balancing​

MCG automatically creates and handles all routing and load balancing for
your workloads. Each workload is assigned a workload URL by Seaplane
that you can use as the entry point to your container. Depending on
where your users are located, that URL will route to different
containers running all over the globe.

## Automated rescheduling​

MCG continuously optimizes the size and location of your workload. This
means that the underlying infrastructure adjusts to better accommodate
your user traffic. If one of the underlying cloud providers suffers an
outage, Seaplane Managed Global Compute reacts by sending new users to
available resources while immediately rescheduling the downed workload
to another resource in the region.

## Automated horizontal and vertical scaling​

Based on traffic patterns, MCG automatically scales workloads
horizontally and vertically. When traffic increases in a location, MGC
spins up more workloads to support the increased user traffic.
Additionally, every time MGC reschedules a workload it evaluates the
resources used and scales the available resources accordingly.

For example, let\'s say a workload uses only half its available CPU.
When MCG reschedules that workload, it evaluates usage, and could decide
to scale down the resources available to that workload to be more cost
efficient. You only pay for what you use (if allowed by your minimum
deployment configuration).

Alternatively, if a workload uses nearly all the resources available to
it, MGC could decide to increase the CPU and RAM available to that
workload (if allowed by your budget constraints).

## Disaster avoidance, no need for recovery​

Depending on your budget and disaster tolerance, MGC goes a step beyond
disaster recovery to disaster avoidance. When deploying workloads on
Seaplane, you set the minimum and the maximum (minimum = disaster
tolerance, maximum = budget) number of containers to always run. If one
of the underlying cloud providers suffers an outage, MCG automatically
reroutes traffic to the next available resource and reschedules the
downed workload to another resource in the region.

## You are in control​

MCG gives you fine-grained controls to manage when, how, and where your
workload runs. For example, per formation (a group of containers) you
control:

-   The minimum and maximum required deployment size (disaster tolerance
    and budget)
-   Allowed and denied cloud providers
-   Allowed and denied regions (these are regulatory regions and not
    cloud regions, for more information see seaplane regions)

By default (and unless you instruct otherwise), Seaplane MCG deploys
globally and across resources to provide the best possible end-user
experience.

</div>

<div>

<div>

**Tags:**

-   introduction
-   compute

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
