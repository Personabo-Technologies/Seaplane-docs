<div>

<div>

<div>

<div>

<div>

On this page

</div>

<div>

# .NET global API deployment

In this tutorial we will show you how to deploy a .NET API on top of
Seaplane Global Compute. In the interest of keeping this tutorial simple
(and saving us some time) we'll be patterning our how-to after this AWS
doc on deploying a fairly standard .NET API.

## Prerequisites​

For this tutorial, you'll need:

-   Microsoft Visual Studio, which you can download here.
-   A working knowledge of Docker, which you can cultivate via their
    excellent documentation.
-   A Seaplane account, which you can request access to here. We're
    giving away free \$500 boarding passes for qualified projects, so
    please contact us to get yours!

Request a Seaplane Account

## Build (or Borrow) Your API​

First things first, we need something to deploy. If you already have an
API you want to deploy with Seaplane feel free to use that, but if
you're just interested in testing our platform, Microsoft Visual Studio
includes sample projects you can use. We'll be using one of those sample
projects for this tutorial.

<div>

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)tip

</div>

<div>

If you don't have Microsoft Visual Studio you can grab the project files
from our GitHub repo.

</div>

</div>

To use a Visual Studio sample project, open Visual Studio. Select Create
New Project and choose API from the list of options. We're going to name
our project *DemoNetCoreWebAPI* just like the AWS tutorial.

Creating the API in visual studio. Your interface might look different
depending on your operating system.

Press F5 (or FN + F5 if you're working on a mac) to run the project
locally. Once the build finishes, it should open a webpage with the
Swagger interface. Before we go any further, we'll confirm everything is
working by querying the API. Once we're sure everything is in working
order, we'll proceed to the next step.

## Containerize Your Workload​

Now that we have a sample API, let\'s go ahead and containerize the
workload. Navigate to the project directory --- generally, this is
`~/Projects/DemoNetCoreWebAPI`. Create a new file called `dockerfile`
and copy and paste the code below. Note: you may need to update the
aspnet and SDK versions depending on when you're following this tutorial
as we, unfortunately, cannot predict the future.

<div>

<div>

    FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
     WORKDIR /app
    EXPOSE 80
    EXPOSE 443

    FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
    WORKDIR /src
    COPY ["DemoNetCoreWebAPI/DemoNetCoreWebAPI.csproj", "DemoNetCoreWebAPI/"]
    RUN dotnet restore "DemoNetCoreWebAPI/DemoNetCoreWebAPI.csproj"
    COPY . .
    WORKDIR "/src/DemoNetCoreWebAPI"
    RUN dotnet build "DemoNetCoreWebAPI.csproj" -c Release -o /app/build

    FROM build AS publish
    RUN dotnet publish "DemoNetCoreWebAPI.csproj" -c Release -o /app/publish

    FROM base AS final
    WORKDIR /app
    COPY --from=publish /app/publish .
    ENTRYPOINT ["dotnet", "DemoNetCoreWebAPI.dll"]

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

Once executed, this code creates a new Docker image using the Visual
Studio project we borrowed earlier.

We create the docker image by running the following command inside this
directory.

<div>

<div>

    docker buildx build -t demo-api-net .

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

In the same directory, run the following command to spin up your newly
created container listening on port 80. We do this to quickly confirm
everything is working before we move on to the next step.

<div>

<div>

    docker run -dp 80:80 demo-api-net

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

We can also test the API by querying it using Curl.

<div>

<div>

    curl -X GET "http://localhost/WeatherForecast" -H  "accept: text/plain"

    [{"date":"2022-04-13T22:43:28.553927+00:00","temperatureC":38,"temperatureF":100,"summary":"Hot"},{"date":"2022-04-14T22:43:28.5546605+00:00","temperatureC":27,"temperatureF":80,"summary":"Scorching"},{"date":"2022-04-15T22:43:28.5546621+00:00","temperatureC":46,"temperatureF":114,"summary":"Sweltering"},{"date":"2022-04-16T22:43:28.5546622+00:00","temperatureC":36,"temperatureF":96,"summary":"Mild"},{"date":"2022-04-17T22:43:28.5546624+00:00","temperatureC":-14,"temperatureF":7,"summary":"Balmy"}]%

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

If you receive a list containing a JSON string of temperatures then
congratulations! Everything is working as intended and we're ready to
deploy our application on Seaplane.

# Deploying on Seaplane

We are ready to build, tag, and upload our newly created container to
the Seaplane Cloud Registry. To do that we need to run these three
commands in the directory with our dockerfile.

<div>

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)caution

</div>

<div>

**Running on an M1 Mac?**

ARM-based images are currently only supported on AWS. If you are working
on an ARM-based system like an M1 Mac and want to have access to all
available cloud resources you should build the image using X86
architecture using the `--platform linux/amd64` flag.

</div>

</div>

Make sure to change `<you_org_name>` with your organization\'s name i.e.
the name you used to sign up for Seaplane.

<div>

<div>

    docker buildx build -t demo-api-net:latest .

    docker tag demo-api-net:latest registry.cplane.cloud/<your_org_name>/demo-api-net:latest

    docker push registry.cplane.cloud/<your_org_name>/demo-api-net:latest

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

Now that our image is uploaded to the Seaplane registry we can build a
*flight*. You can read more about *flights*, *formations*, and other
Seaplane terminology in our docs, but to make a long story short:
a *formation* is an application or service which is made up of logical
containers we call *flights*. *Flights* (logical containers) *→
Formations* (application or service). What can we say? We like planes
around here.\`

First, we'll create a *flight plan* locally then we'll add it to a
*formation* in preparation for deployment. A *flight plan* is exactly
what it sounds like: a set of rules for our *flight* to adhere to. This
can include the number of instances, provider restrictions, location
restrictions, and much more. For now we'll keep it simple and only
change the name, leaving all other values set to their defaults.

<div>

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)tip

</div>

<div>

**Never deployed on Seaplane before?**

If you have never deployed a workload on Seaplane before, it might be
worth going over a few quick questions to help you decide what kind and
how many resources you want to dedicate to this workload. You can read
more about it here.

</div>

</div>

To add the *flight* to the *flight plan* we run the following command.
We use the `--name` tag to make it easy to identify the workload
internally, but that's entirely optional:

<div>

<div>

    seaplane flight plan --image "<your_org_name>/demo-api-net:latest" --name "net-api-example-flight"

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

To add the *flight* to a *formation* and deploy it globally, we run this
command:

<div>

<div>

    seaplane formation create --name net-api-example-formation --include-flight-plan net-api-example-flight --public-endpoints /=net-api-example-flight:80 --launch

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

Let's pause and take a closer look at the snippet above.

We created a new *formation* and named it `net-api-example-formation` .
We then added our previously created *flight plan* to that new
*formation* and added port 80 to the public endpoints that are available
on the `/` path. Finally, we launched the formation using`--launch`.

Seaplane will spin up a container globally with your API and return the
relevant URL, like this:

<div>

<div>

    The remote Formation Instance URL is <URL>

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

If done correctly, querying the URL using curl will return the same
result as it did locally, but because it's been deployed with Seaplane
it will now be globally available to all your users, no matter where in
the world they are.

<div>

<div>

    curl -X GET "<yourseaplane_url>/WeatherForecast" -H  "accept: text/plain"

    [{"date":"2022-05-04T18:46:34.9094484+00:00","temperatureC":48,"temperatureF":118,"summary":"Freezing"},{"date":"2022-05-05T18:46:34.9101944+00:00","temperatureC":5,"temperatureF":40,"summary":"Warm"},{"date":"2022-05-06T18:46:34.9101972+00:00","temperatureC":-16,"temperatureF":4,"summary":"Sweltering"},{"date":"2022-05-07T18:46:34.9101974+00:00","temperatureC":16,"temperatureF":60,"summary":"Bracing"},{"date":"2022-05-08T18:46:34.9101976+00:00","temperatureC":-15,"temperatureF":6,"summary":"Cool"}]%     

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

Seaplane will automatically adjust deployment size (machine spec) and
location based on user demand, so you don't need to select any regions
or provision any hardware. Seaplane will also take care of all the load
balancing and autoscaling (both horizontally and vertically) for you.

## Comparing Seaplane and AWS EC2​

This tutorial looks pretty similar to that AWS EC2 tutorial we mentioned
earlier. They both cover the same topic, they're both relatively short,
and they both look simple at first blush. However, where Seaplane really
is as simple as this tutorial implies, that EC2 tutorial skips over a
few important steps and uses a pre-configured file of 100+ lines of
YAML.

In fairness, Seaplane is a fully managed platform and EC2 is not. Nor
does it pretend to be! However, deceptively simple tutorials and
diagrams can give users the wrong impression about how much time, money,
and resources will be required to stand up their infrastructure using a
solution like EC2.

Take a look at the basic YAML template provided to set up 2 compute
instances in two availability zones with an internet gateway in between.
All that YAML (over 100 lines) and users still need to either configure
the gateway or add a load balancer on top. Now imagine if, instead of
two availability zones, you want to add a third availability zone, or an
entirely new region. The YAML is endless!

We're not trying to pick on AWS here. AWS has a lot of excellent
services, and every provider likes to keep tutorials simple. However,
it's important to know how much work needs to happen behind the scenes
to make these services work. If you don't have a large platform
engineering team with a lot of provider-specific devops experience, even
doing something simple --- like deploying a standard .NET API --- can
become really challenging really quickly. This is especially true if you
want to deploy in more than one region or on more than one provider.

We built Seaplane specifically to make that multi-region, multi-cloud
part easy. Load-balancing, scaling, VPNs, and redundancy are all
built-in features for every workload on Seaplane Managed Global Compute.

If you were following along with this tutorial your app is now available
across every region on every major public cloud with a single API call.
No YAML required.

<div>

<div>

    seaplane formation plan \
      --include-flight-plan "name=net-api-example-flight ,image=<your_org_name>/demo-api-net:latest" \
      --public-endpoint /=net-api-example-flight:80 \
      --launch

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

Request a Seaplane Account

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
