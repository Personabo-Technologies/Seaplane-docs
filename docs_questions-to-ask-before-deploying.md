<div>

<div>

<div>

<div>

<div>

<div>

# Before you deploy

</div>

If You've Never deployed on Seaplane Before there are some things to
consider before you deploy. Things like local data regulations, cost,
and provider restrictions.

By default, our platform will deploy your application wherever your
users are. If you have users in Tokyo, Paris, and San Diego your app
will automatically deploy in Tokyo, Paris, and San Diego using the most
performant combination of clouds, bare metal providers, and edge.

Additional configuration is simple to implement --- no really, **these
are all done with flags you'll pass along with your API call** --- so
don't think of these as engineering questions (or additional engineering
lifts) but as business considerations.

Here are some questions to ask before you deploy:

1.  How important is resilience? Is this a workload that absolutely
    cannot go down for any amount of time, or can you tolerate some
    amount of risk?
2.  Are there any cloud providers that your business cannot use? Some
    organizations have strict regulations around which cloud providers
    they can and cannot use due to contractual obligations, security
    concerns, or compliance requirements.
3.  Does this workload need to stay within certain regulatory zones? To
    comply with local data privacy laws (like GDPR, CCPA, or the IPDPB)
    some workloads need to be restricted to specific countries or
    regions.
4.  What are your budgetary constraints? Is there a number you can't go
    over, or is money no object?

Here's how to configure the platform, based on your answers:

1.  Applications deployed with Seaplane are resilient by default. When
    there's an outage, Seaplane Autopilot automatically finds the next
    best resource to run your workload. Rescheduling can take a few
    seconds, so if it's absolutely critical that your application never
    goes down you can tell Seaplane to always run multiple instances of
    your workload. You can also upgrade any critical workloads to the
    First Class tier for priority rescheduling during times of outage.
    You can read more about resilience in our documentation here and
    here.
2.  While Seaplane deploys on every major public cloud by default, we do
    offer additional controls in case your organization has requirements
    around which providers it can and cannot use. You can read more
    about provider restrictions in our documentation here, but generally
    we recommend only restricting providers when absolutely necessary.
3.  Seaplane runs global and regional anycast networks to restrict
    workloads to specific countries and regulatory zones. This gives you
    all the knobs and levers you need to comply with regulations like
    GDPR, CCPA, ITA-2000, and more.
4.  Seaplane Managed Global Compute comes in three tiers: Coach,
    Business, and First Class. Workloads that require more compute
    resources require higher tiers to run, and the First Class tier is
    the only tier that offers consistent access to hyper-local edge
    compute. You can read about our pricing model on our blog, see the
    breakdown on our pricing page, and learn how to set your budget in
    our documentation.

</div>

<div>

<div>

**Tags:**

-   getting started

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
