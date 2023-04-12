<div>

<div>

<div>

<div>

-   ![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)
-   Managed Global Compute
-   Container Registry
-   Image Management

<div>

On this page

</div>

<div>

<div>

# Image Management

</div>

This document explains how to use the *Seaplane Container Registry*
(*SCR*) to upload and manage images for use with *Seaplane Managed
Global Compute* (*MGC*)

## Uploading images​

Follow the steps below to build, tag, and push your images to the *SCR*,
but don\'t forget to log in before following along! You can read more
about *SCR* authentication here.

### Building images​

To build your image, use the command below --- making sure to replace
`<app-name>` with your actual application name.

<div>

<div>

    docker buildx build --platform linux/amd64 -t <app-name> .

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

Build your images for X86!

<div>

<div>

<div>

<div>

ARM-based images are not currently supported on Seaplane. If you are
working on an ARM-based system like an M1 Mac, you need to build the
image using X86 architecture by adding \`\--platform linux/amd64\`.

</div>

</div>

</div>

</div>

### Tagging images​

To tag your image, use the command below. This time you\'ll need to
replace both `<tenant-id>` with your tenant ID and `<app-name>` with
your application name. You can find your tenant ID under the Tenants
section in *Flightdeck*.

<div>

<div>

    docker tag <app-name> registry.cplane.cloud/<tenant-id>/<app-name>

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

### Pushing images​

To push your tagged image to the *SCR*, use the command below. Depending
on the size of your image, this might take a bit so give it a few
minutes. Don\'t forget to eplace `<tenant-id>` with your tenant ID and
`<app-name>` with your application name.

<div>

<div>

    docker push registry.cplane.cloud/<tenant-id>/<app-name>

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

With your image successfully pushed to the *SCR* you are now ready to
deploy it on Seaplane!

</div>

<div>

<div>

**Tags:**

-   registry
-   pushing images
-   compute
-   uploading images

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
