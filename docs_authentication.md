<div>

<div>

<div>

<div>

-   ![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)
-   Authentication

<div>

<div>

# Authentication

</div>

To get started with Seaplane, you\'ll need to set up a Seaplane account.
You can create your Seaplane account, and generate your first API key,
using our Flightdeck --- you can request access here. Once you have an
API key, you can configure it with the interface of your choice using
the examples below.

<div>

-   CLI
-   CURL
-   Python SDK

<div>

<div>

Open a terminal and run `seaplane account login`. Follow the steps
inside your terminal to complete the setup. For more detailed
instructions, see the CLI API-key configuration documentation

</div>

<div>

<div>

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)note

</div>

<div>

Flightdeck requires the `Content-Length` header to be set to `0`. cURL
does so by default but if you are using any other method make sure to
include it.

</div>

</div>

<div>

<div>

    API_KEY="your-api-key"

    # the auth token lasts for 1 minute
    TOKEN=$(curl  https://flightdeck.cplane.cloud/identity/token --request POST \
      --header "Authorization: Bearer $API_KEY")

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

</div>

<div>

By default, the Seaplane Python SDK manages token renewal for you.

<div>

<div>

    from seaplane import sea
    sea.config.set_api_key("YOUR-API-KEY")

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

If you prefer to handle token management yourself, you can override this
behavior by requesting and setting the token manually. Tokens are active
for \~1 minute. When you receive a `401 Unauthorized` return code you
should renew the token.

<div>

<div>

    from seaplane import sea
    sea.config.set_api_key("YOUR-API-KEY")

    # request token
    token = sea.auth.get_token()

    # set token
    sea.config.set_token(token)

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

</div>

</div>

</div>

</div>

<div>

<div>

**Tags:**

-   authentication
-   getting started

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
