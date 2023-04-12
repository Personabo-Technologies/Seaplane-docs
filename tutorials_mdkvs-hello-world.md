<div>

<div>

<div>

<div>

-   ![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)
-   Metadata Key-Value Store Hello World

<div>

On this page

</div>

<div>

<div>

# Metadata Key-Value Store Hello World

</div>

In this tutorial, we will take you through your first steps with the
Metadata Key-Value Store (MDKVS). We will cover all the basic operations
such as read, write, delete and restrict. You can read more about all of
these concepts in the documentation and API reference.

## Prerequisites​

To follow along you need access to a Seaplane account. You can create
one here.

## Authentication​

Before we start, make sure you are properly authenticated. All examples
below assume you completed the authentication steps using the examples
below.

<div>

-   CLI
-   cURL
-   Python

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

## Setting a key​

Create your first key-value pair using one of the code snippets below.
In these examples `hello` represents the key and `world` represents the
value.

The MDKVS requires keys and values to be base64 encoded. The CLI and SDK
handle the encoding automatically, for the `cURL` request we are
providing a function to encode/decode your strings. You can read more
about base64 encoding here

<div>

-   CLI
-   cURL
-   Python

<div>

<div>

<div>

<div>

    seaplane metadata set hello world

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

</div>

<div>

<div>

<div>

    # function to base64 encode the key and value
    function encode {
        perl -MMIME::Base64=encode_base64url -ne 'print encode_base64url($_),"\n"' 
    }

    # base64 encode key and value
    key=$(printf "hello" | encode)
    value=$(printf "world" | encode)

    # set key value pair
    curl "https://metadata.cplane.cloud/v1/config/base64:${key}" \
        --header "Authorization: Bearer ${TOKEN}" \
        --request PUT \
        --data "${value}"

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

</div>

<div>

<div>

<div>

    from seaplanekit import sea
    from seaplanekit.model import KeyValue

    # set a key-value pair
    sea.metadata.set(
      key_value=KeyValue(b"hello", b"world")
    )

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

</div>

</div>

</div>

## Getting a key​

Retrieve the key you just created using one of the code snippets below.

<div>

-   CLI
-   cURL
-   Python

<div>

<div>

Use the `--decode` or `-D` flag to automatically decode your key-value
pair.

<div>

<div>

    seaplane metadata get hello --decode

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

</div>

<div>

<div>

<div>

    # function to base64 encode the key
    function encode {
        perl -MMIME::Base64=encode_base64url -ne 'print encode_base64url($_),"\n"' 
    }

    # function to base64 decode the returned key value pair
    function decode {
        perl -MMIME::Base64=decode_base64url -ne 'print decode_base64url($_),"\n"'
    }

    # base64 encode key
    key=$(printf "hello" | encode)

    # request key-value pair
    result=$(curl "https://metadata.cplane.cloud/v1/config/base64:${key}" \
        --header "Authorization: Bearer ${TOKEN}" \
        --request GET)

    # decode the result 
    echo $result | jq .value | decode

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

</div>

<div>

<div>

<div>

    from seaplanekit import sea
    from seaplanekit.model import Key

    # get key-value pair
    sea.metadata.get(key=Key(b'hello'))

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

</div>

</div>

</div>

## Deleting a key​

Delete the key we just created using one of the code snippets below.

<div>

-   CLI
-   cURL
-   Python

<div>

<div>

<div>

<div>

    seaplane metadata delete hello

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

</div>

<div>

<div>

<div>

    # function to base64 encode the key
    function encode {
        perl -MMIME::Base64=encode_base64url -ne 'print encode_base64url($_),"\n"' 
    }

    # base64 encode key
    key=$(printf "hello" | encode)

    # delete a key-value pair
    curl "https://metadata.cplane.cloud/v1/config/base64:${key}" \
        --header "Authorization: Bearer ${TOKEN}" \
        --request DELETE

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

</div>

<div>

<div>

<div>

    from seaplanekit import sea
    from seaplanekit.model import Key

    # get key-value pair
    sea.metadata.delete(key=Key(b'hello'))

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

</div>

</div>

</div>

## Using Directories​

MDKVS supports the use of directories. You can use directories to
restrict groups of key-value pairs. To learn more about restrictions
have a look at the documentation here.

In this example, you are restricting your keys to North America. First,
you are going to create a directory with a key-value pair and then
restrict that directory. Seaplane uses the ISO Alpha2 codes to indicate
countries and regions. You can learn more about them here. In this
example, we are restricting to North America indicated by the ISO code
`XN`.

-   The directory name is `my-us-keys`
-   The key is `hello`
-   The value is `world`

<div>

-   CLI
-   cURL
-   Python

<div>

<div>

### Creating the directory with a key-value pair​

<div>

<div>

    seaplane metadata set my-us-keys/hello world

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

### Restricting the directory to stay inside North-America​

<div>

<div>

    seaplane restrict set config my-us-keys --region XN

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

### Restriction enforcement​

It might take a few seconds for all data to move to the new location,
especially if you store a lot of keys in your directory. You can check
the status of a restriction using the following command.

If the state is `pending` it means Seaplane is still moving your keys.
`enforced` means all your keys inside your directory are restricted.

<div>

<div>

    seaplane restrict list -D

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

</div>

<div>

### Creating the directory with a key-value pair​

<div>

<div>

    # function to base64 encode the key and value
    function encode {
        perl -MMIME::Base64=encode_base64url -ne 'print encode_base64url($_),"\n"' 
    }

    # base64 encode key and value
    key=$(printf "my-us-keys/hello" | encode)
    value=$(printf "world" | encode)

    # set key value pair
    curl "https://metadata.cplane.cloud/v1/config/base64:${key}" \
        --header "Authorization: Bearer ${TOKEN}" \
        --request PUT \
        --data "${value}"

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

### Restricting the directory to stay inside the North-America​

<div>

<div>

    # function to base64 encode the key and value
    function encode {
        perl -MMIME::Base64=encode_base64url -ne 'print encode_base64url($_),"\n"' 
    }

    # base64 encode the direcotry
    key=$(printf "my-us-keys" | encode)

    # set allowed region
    curl "https://metadata.cplane.cloud/v1/restrict/<API>/base64:${directory}/" \
    --header "Authorization: Bearer ${TOKEN}" \
    --data '{
              "regions-allowed" : ["XN"]' \
    --request PUT

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

### Restriction enforcement​

It might take a few seconds for all data to move to the new location,
especially if you store a lot of keys in your directory. You can check
the status of a restriction using the following command.

If the state is `pending` it means Seaplane is still moving your keys.
`enforced` means all your keys inside your directory are restricted.

<div>

<div>

    # function to base64 encode the key and value
    function encode {
        perl -MMIME::Base64=encode_base64url -ne 'print encode_base64url($_),"\n"' 
    }

    # base64 encode the direcotry
    key=$(printf "my-us-keys" | encode)

    # get the pregion restrictions for the directory
    curl "https://metadata.cplane.cloud/v1/restrict/config/base64:${directory}/" \
    --header "Authorization: Bearer ${TOKEN}" \
    --request GET

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

</div>

<div>

Coming Soon!

</div>

</div>

</div>

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

Last updated on **Mar 16, 2023** by **fokkedekker**

</div>

</div>

</div>

</div>

</div>

</div>
