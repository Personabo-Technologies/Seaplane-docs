<div>

<div>

<div>

<div>

-   ![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)
-   Seaplane Introduction

<div>

On this page

</div>

<div>

<div>

# Seaplane Introduction

</div>

Seaplane is a global control plane that uses the optimal combination of
public clouds, bare metal providers, and edge resources to deliver
applications when and where they are needed to end users all over the
world. Seaplane operates a bit like a CDN, but for a much richer set of
workloads and full-stack applications.

Seaplane currently has solutions for compute, data, coordination and
more. For a complete list of Seaplane products, take a look at our
documentation below. If we're missing something please let us know or
have a look at our upcoming beta products. If you see something you
like, be sure to request early access!

## Why Seaplane?​

Instead of deploying your app to a single zone, region, or cloud
provider, with Seaplane you deploy your app to the Seaplane Global
Network, which consists of multiple clouds (AWS, GCP, Azure), edge, and
bare metal providers. Our control plane then automatically deploys the
app where it is needed within that network based on end-user demand and
cost.

Seaplane abstracts away unnecessary infrastructure complexity and only
exposes what you need to run your applications. You can manage high
level expressions of operational constraints to standardize your
deployments across teams, ensure data residency compliance, or exclude
specific providers. Basically, you have as much control as you need
without the YAML you don\'t.

Here's what that looks like for a standard compute workload:

Suppose you have a compute workload that serves users in the US and
Europe. Without Seaplane, deploying that workload on your average cloud
provider will go something like this.

-   Pick a cloud provider.
-   Provision infrastructure in at least two regions --- likely one in
    the US and one in Europe.
-   Set up load balancing, routing, autoscaling, fail-over solutions,
    etc.
-   Set up monitoring (and hope there aren't any major outages).

Alternatively, you could use Seaplane and reduce all those steps into a
single command:

<div>

<div>

    seaplane formation create \
        --name "my-app-formation" \
        --include-flight-plan "my-compute-workload" \
        --public-endpoints "/"="my-compute-workload":"80"\
         --launch

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

When executed, Seaplane deploys your workload globally when and where
it's needed. Load balancing, routing, autoscaling and rescheduling is
all taken care of for you while Seaplane continuously optimizes the
underlying infrastructure based on end user demand and cost. If one of
Seaplane's underlying cloud providers suffers an outage, your workloads
will be automatically rescheduled.

All Seaplanes products are deployed on the same global control plane,
meaning our services all benefit from being multi-region, multi-cloud,
and disaster resistant. Have a look at the product-specific
documentation if you want to learn more about individual offerings.

## You are in control​

Every business has to contend with its own set of rules, restrictions,
and regulations --- especially businesses that operate across regions,
countries, and continents. Seaplane gives users the power to institute
compliance on the organizational, rather than individual, level.

For example, you can restrict most Seaplane products to run only in
specified regions, countries, or clouds to comply with legislation like
the GDPR, CCPA, and PIPEDA. For more information have a look at the
restrictions section of our documentation.

Thank you for flying with Seaplane!

</div>

<div>

<div>

**Tags:**

-   learning
-   seaplane
-   introduction

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
