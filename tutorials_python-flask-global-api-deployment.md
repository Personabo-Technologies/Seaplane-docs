<div>

<div>

<div>

<div>

<div>

On this page

</div>

<div>

# Python Flask - Global API Deployment

## Introduction​

This tutorial will teach you how to deploy an HTTPS API created with
Flask on Seaplane's Managed Global Compute platform. You will deploy
your API globally on multi-cloud and multi-region infrastructure in a
couple of minutes.

## Prerequisites​

For this tutorial you'll need:

-   Python-flask as well as flask-RESTful
-   A working knowledge of Docker containers
-   A Seaplane account, which you request access for here

If you don't want to write your own code, but you do want to test our
platform, you can download the code or the compiled Docker image
directly from our Github repo here and skip to the section "Deploying on
Seaplane."

The Seaplane Managed Global Database is currently in a closed beta, so
to keep this tutorial accessible we'll be pulling the image directly
from the container. If you are interested in our Managed Global
Database, you can request early access here.

Request a Seaplane Account

## The API​

For the purposes of this tutorial, we created a simple API using flask
and the flask-RESTful Python package to serve images directly from a
container based on HTTP status codes. It works like HTTP Cats but
instead of serving cat pictures it serves pictures of seaplanes,
naturally.

<div>

<div>

    from flask import Flask, send_file
    from flask_restful import reqparse, abort, Api, Resource
    import os


    app = Flask(__name__)
    api = Api(app)

    # all available status codes and corresponding images
    STATUS_CODES = {
        "200" : "./seaplane_200.jpg",
        "404" : "./seaplane_404.jpg",
        "500" : "./seaplane_500.jpg",
    }


    # check to make sure the requested status code is available. Otherwise return 404
    def abort_if_status_doesnt_exist(status_code):
        if status_code not in STATUS_CODES:
            abort(404, message="Status code {} doesn't exist".format(status_code))

    # return the requested status code
    class ResponseCodes(Resource):
        def get(self, status_code):

            # check to make sure the status code exists 
            abort_if_status_doesnt_exist(status_code)

            # return the requested image
            return send_file(STATUS_CODES[status_code], mimetype="image/jpg")

    # API routing
    api.add_resource(ResponseCodes, '/statuscode/<status_code>')

    if __name__ == '__main__':
        app.run(host='0.0.0.0', debug=False, port=80)

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

## Containerizing your API​

Now that we have our API, we'll go ahead and containerize our workload
using Docker.

First, we create a file for all Python requirements as
`requirements.txt` . For our example it's just flask and flask-RESTful,
but we'll also specify the version to ensure the application works
properly.

<div>

<div>

    flask_restful == 0.3.9
    flask == 2.1.1

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

Next we need to create our Dockerfile. We'll load the base Python image
from the Docker registry, copy our files, and install the required
packages into our Dockerfile.

<div>

<div>

    # Base image
    FROM python:3.8.5-slim-buster

    # set base dir
    WORKDIR /usr/app/src

    # Copy files
    COPY . /src

    # Install dependencies
    RUN pip install -r /src/requirements.txt

    # start the api
    CMD [ "python3", "/src/api.py"]

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

Make sure all the files are in the same directory, then run the Docker
build command `docker buildx build -t demo-api-python .`

<div>

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)caution

</div>

<div>

**Working on an M1 Mac?**

ARM-based images are currently only supported on AWS. If you are working
on an ARM-based system like an M1 Mac and want to have access to all
available cloud resources you can build the image using X86 architecture
by adding `--platform linux/amd64`.

</div>

</div>

Now that we have our Docker image, let's do a quick test to make sure
everything is in working order. Run it on your local machine using the
command `docker run -dp 80:80 demo-api-python .` (this won\'t work on M1
Macs as they do not support X86 workloads).

Open the URL in your browser to view the result
`http://localhost:80/statuscode/500`

If everything was done correctly, you should be able to query the API
with status code 500 and get the 500 internal server error image in
return.

## Deploying on Seaplane​

Time for the best part! Let's deploy our newly created image using the
Seaplane Managed Global Compute platform. By default, our platform will
deploy your application wherever your users are. If you have users in
Tokyo, Paris, and San Diego your app will automatically deploy in Tokyo,
Paris, and San Diego using the most performant combination of clouds,
bare metal providers, and edge.

<div>

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)info

</div>

<div>

**Never Deployed On Seaplane Before?**

If you have never deployed a workload on Seaplane before, it might be
worth going over a few quick questions to help you decide what kind and
how many resources you want to dedicate to this workload. You can read
more about it here.

</div>

</div>

To deploy you'll need both the Seaplane CLI and an API key, which you
can read more about configuring here. Don't worry, you only need to
configure the Seaplane CLI once!

Upload your container image to the Seaplane cloud registry using the
following commands in the directory containing your Dockerfile.

<div>

<div>

    docker buildx build  -t demo-api-python:latest .

    docker tag demo-api-python:latest registry.cplane.cloud/<your_org_name>/demo-api-python:latest

    docker push registry.cplane.cloud/<your_org_name>/demo-api-python:latest

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

Now we're going to create our first Seaplane API call. First, we are
going to create a local *flight plan* based on the image we just
uploaded.

You can read more about *flights*, *formations*, and other Seaplane
terminology in our Terminology doc, but to make a long story short:
a *flight* is a logical container and a *flight plan* is a set of rules
for our *flight* to adhere to. This can include the number of instances,
provider restrictions, location restrictions, and more. For now though
we'll keep it simple and only change the name, leaving all other values
set to their defaults.

<div>

<div>

    seaplane flight plan --image "registry.cplane.cloud/<your_org>/demo-api-python" --name "python-example-api-flight"

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

To deploy our image we are going to add it to a *formation* and launch
it to be available globally. In Seaplane, a *formation* can be thought
of as your application or service. *Formations* are made up
of *flights*. *Flights* (logical containers) *→ Formations* (application
or service).

Depending on your answers to Some Questions to Ask Before You Deploy\...
you can use the flags below to configure your workload to your liking.

-   `--providers_allowed` and `--providers_denied` to include or exclude
    specific providers
-   `--region_allowed` and `--regions_denied` to include or exclude
    specific regions
-   `--minimum` and `--maximum` to set the minimum and the maximum
    number of instances for your workloads

For now, we will keep things simple and keep all these to their default
values. Allowing the workload to be deployed anywhere on any provider
and scale indefinitely.

<div>

<div>

    seaplane formation create --name "python-api-demo-formation" --include-flight-plan "python-example-api-flight" --public-endpoints "/"="python-example-api-flight":"80" --launch

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

There are a couple of things happening here. We created a new
*formation*, added our local *flight plan* to it with the
---include-flight-plan flag, and used the ---public-endpoints flag to
open port 80 for external traffic. Finally, we used the ---launch
command to launch the *formation*. If everything is working as expected
you should receive the remote *formation* URL from the Seaplane CLI.

We can get a list of all running *formations* through the CLI by
executing seaplane formation list. You can see our newly create
*formation* listed as deployed (*in air*) i.e running globally in this
case.

<div>

<div>

    seaplane formation list
    LOCAL ID  NAME                       LOCAL  DEPLOYED (GROUNDED)   DEPLOYED (IN AIR)   TOTAL CONFIGURATIONS
    96c84c57  python-api-demo-formation  1      0                    1                   1

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

We can query our newly created API through our browser by visiting
`<your_remote_formation_url>/statuscode/200` or any of the other
supported status codes (200,404,500).

And just like that, our app is live and serves seaplane-themed response
codes to our users wherever in the world they may be.

If you want to read more tutorials like this one, keep an eye on our
documentation or join our discord. For updates on our latest releases,
upcoming launches, and general industry news, subscribe to our
newsletter.

To participate in our ongoing user research, or get early access to
future releases contact us below.

Until next time, fly safe!

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
