<div>

<div>

<div>

<div>

-   ![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)
-   Python Django - Global API Deployment

<div>

On this page

</div>

<div>

<div>

# Python Django - Global API Deployment

</div>

## Introduction​

This tutorial will teach you how to deploy a Django based web-app
serving PDFs on Seaplane. Following the steps below, you\'ll be able to
deploy your API globally on multi-cloud and multi-region infrastructure
in just a few minutes.

## Prerequisites​

For this tutorial you'll need:

-   A Django container --- you can download our test container here to
    follow along.
-   A working knowledge of Docker containers.
-   A working Seaplane account --- you can read more about account
    creation and set up here.
-   The Seaplane CLI installed on your machine --- you can read more
    about the Seaplane CLI here.

The Seaplane Managed Global Database is currently in a closed beta, so
to make this tutorial accessible for everyone, we'll be pulling the PDFs
directly from the container. If you\'re interested in using our Managed
Global Database, you can request early access here

## The API​

For the purposes of this tutorial, we created a simple Django webserver
that serves the Seaplane Global Compute One-pager PDF. It provides the
user with an interface to request the PDF then serves the file directly
from the container. The code can be found here.

## Containerizing Your API​

With our Django app in hand, let\'s create a container to deploy on
Seaplane. A dockerfile is already prepared as part of the demo app. To
build the image, run the Docker build command in the repo directory:

`docker buildx build -t django-demo-app:latest .`

<div>

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)caution

</div>

<div>

**Working on an M1 Mac?**

ARM-based images are not currently supported on Seaplane. If you are
working on an ARM-based system like an M1 Mac, you need to build the
image using X86 architecture by adding `--platform linux/amd64`.

</div>

</div>

Now that we have our Docker image, let's do a quick test to make sure
everything is in working order. Run it on your local machine using the
command `docker run -dp 80:80 django-demo-app .` (this won\'t work on M1
Macs as they do not support X86 workloads).

Open the URL in your browser to view the result `http://localhost:80/`.

If everything was done correctly, you should see an interface with a
single button. When you click that button, the app will serve the
Seaplane one-pager.

## Deploying on Seaplane​

Time for the best part! Let's deploy our newly created image using the
Seaplane Managed Global Compute platform. By default, our platform
deploys your application wherever your users are. If you have users in
Tokyo, Paris, and San Diego your app will automatically deploy in Tokyo,
Paris, and San Diego using the most performant combination of clouds,
bare metal providers, and edge.

To deploy, you'll need both the Seaplane CLI and an API key. Follow
these steps to register your account and these steps to set up the CLI
if you haven\'t done so already.

Next, you need to log in to the *Seaplane Container Registry*. First,
create a file called `seaplane-api-key.txt` and copy in your API key.
You can get your API key from the Flightdeck.

To log in run the following command in the directory where you saved
your API-key.

<div>

<div>

    cat seaplane-api-key.txt | docker login \
      -u docker \
      --password-stdin https://registry.cplane.cloud

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

For the rest of the steps, we will need the `tenant-id` and the
`tenant-subdomain` through Flightdeck.

Logged in and with your `tenant-id` in hand you are ready to upload your
container image. Run the following command and replace
`<YOUR-TENANT-ID>` with the `tenant-id`.

<div>

<div>

    $ docker tag \
      django-demo-app:latest \
      registry.cplane.cloud/<YOUR-TENANT-ID>/django-demo-app:latest

    $ docker push \
      registry.cplane.cloud/<YOUR-TENANT-ID>/django-demo-app:latest

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

<div>

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)info

</div>

<div>

In Seaplane a *flight* is a logical container and a *flight plan* is a
set of rules for our *flight* to adhere to. A *formation* can be thought
of as your application or service. *Formations* are made up
of *flights*.

</div>

</div>

Now we're going to execute our first Seaplane compute command. First, we
are going to create a local *flight plan* based on the image we just
uploaded. To keep things simple you can leave all configurable details
empty which will set them to their default values.

<div>

<div>

    seaplane flight plan \
      --name django-demo-flight \
      --image registry.cplane.cloud/<YOUR-TENANT-ID>/django-demo-app:latest

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

To deploy our image we are going to add it to a *formation* and launch
it globally.

Depending on your business rules and requirements we can add flags to
allow or deny specific regions and providers.

-   `--providers-allowed` and `--providers-denied` to include or exclude
    specific providers
-   `--region-allowed` and `--regions-denied` to include or exclude
    specific regions

We\'ll keep things simple and leave these with their default values.
This allows the workload to scale indefinitely and be deployed anywhere
on any provider.

<div>

<div>

    seaplane formation create \
      --name django-demo-formation \
      --include-flight-plan django-demo-flight \
      --launch

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

There are a few things happening now. We created a new *formation*,
added our local *flight plan* to it with the `-—include-flight-plan`
flag. Finally, we used the `-—launch` command to launch the *formation*.
If everything is working as intended, you should receive the remote
*formation* URL from the Seaplane CLI.

We can get a list of all running *formations* through the CLI by
executing `seaplane formation list`. You can see our newly created
*formation* listed as deployed (*in air*) i.e., running globally in this
case.

<div>

<div>

    $ seaplane formation list
    LOCAL ID  NAME                       LOCAL  DEPLOYED (GROUNDED)   DEPLOYED (IN AIR)   TOTAL CONFIGURATIONS
    96c84c57  django-demo-formation      1      0                    1                   1

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

Now that we\'ve launched the application, we can see it by visiting the
formation URL `django-demo-flight.<tenant-subdomain>.on.cplane.cloud`.
The seaplane platform will take some time to distribute the
configuration globally, so if you see errors such as:

<div>

<div>

    upstream connect error or disconnect/reset before headers. reset reason: connection failure, transport failure reason: delayed connect error: 113

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

you may have to wait a bit longer for the system to propogate changes.
And just like that, our app is live and serves PDFs to our users
wherever in the world they may be.

Thank you for flying with Seaplane!

<div>

<div>

</div>

</div>

</div>

<div>

<div>

**Tags:**

-   Django
-   compute
-   tutorial

</div>

</div>

<div>

<div>

</div>

<div>

Last updated on **Mar 16, 2023** by **fokkedekker**

</div>

</div>

</div>

</div>

</div>

</div>
