<div>

<div>

<div>

<div>

-   ![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)
-   Seaplane Hello World

<div>

On this page

</div>

<div>

<div>

# Seaplane Hello World

</div>

This tutorial walks you through your first few interactions with
Seaplane. It includes:

-   Creating an account
-   Configuring your command line interface (CLI)
-   Running your first Seaplane command

After completing this tutorial, you\'ll have a fully configured Seaplane
account on your machine ready to take on more complex deployments. If
you run into trouble, Seaplane support is always here to help!

## Creating and Setting Up Your Account​

Follow the steps outlined in this support article to set up and
configure your Seaplane account. Make sure you complete all the steps in
order, starting with creating an account and ending with generating an
API key.

-   Create an account
-   Add billing details
-   Create a tenant
-   Create an API key

## Downloading and Installing the CLI​

Seaplane can be controlled with various interfaces including a CLI,
Python SDK, Rust SDK, and more. In this tutorial, we\'ll be using the
Seaplane CLI. Follow the steps in this support article to get started
(if you haven\'t already!).

## Running Your First Seaplane Command​

To confirm that your Seaplane account is properly configured and ready
to roll, we\'re going to create a new key-value pair using the *Seaplane
Metadata Key-Value Store*. To create a new \"hello world\" key-value
pair, run the following command in your terminal:
`seaplane metadata set hello world`.

This will create a new key-value pair with the values `hello` and
`world` in the *Seaplane Metadata Key-Value Store*. Your terminal should
return a success message after executing the command, but just to be
sure let\'s go ahead and give it a quick test.

To retrieve your key-value pair (and confirm that everything is in
working order) run the command `seaplane metadata get hello --decode`.
We include `--decode` so the output will be in ASCII and not a string of
hexadecimal characters.

After running `seaplane metadata get hello --decode` you should get the
output `world` for a proper `hello` `world` pair.

And that\'s it! You have successfully created a Seaplane account and
configured the command line interface. If you want to keep learning, or
if you just like kicking the tires, take a look at our tutorial for
deploying a containerized Django app on Seaplane.

</div>

<div>

<div>

**Tags:**

-   getting started
-   hello world
-   coordination
-   Metadata Key-Value Store

</div>

</div>

<div>

<div>

</div>

<div>

Last updated on **Dec 22, 2022** by **fokkedekker**

</div>

</div>

</div>

</div>

</div>

</div>
