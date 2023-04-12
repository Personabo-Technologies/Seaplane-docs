<div>

<div>

<div>

<div>

-   ![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)
-   Managed Global Compute
-   Container Registry
-   Registry Authentication

<div>

On this page

</div>

<div>

<div>

# Registry Authentication

</div>

## Introduction​

This document outlines how to authenticate yourself with the *Seaplane
Container Registry* (SCR). The SCR is required to use *Seaplane Managed
Global Compute* (MGC).

To log in, you need Docker installed and running on your machine. If you
don\'t have Docker installed already, you can always download the latest
version from the Docker website.

Before you can upload your images, you need to log in to the SCR, which
you can do through Docker. First, in your working directory, create a
new txt file and name it `seaplane-api-key.txt`. Then, copy your
Seaplane API key (which you can find in the API Keys section of
Flightdeck) and paste it into `seaplane-api-key.txt`.

Once you\'ve created your txt file and copied over your Seaplane API
key, you can use the command below to log in to the SCR via Docker.

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

Once you\'ve logged in successfully, you should receive a
`Login Succeeded` message. Now you should be ready to use the SCR and
start flying with Seaplane!

<div>

<div>

</div>

</div>

</div>

<div>

<div>

**Tags:**

-   registry
-   authentication
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
