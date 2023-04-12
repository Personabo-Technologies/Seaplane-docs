<div>

<div>

<div>

<div>

-   ![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)
-   Global Coordination Service
-   Metadata Key-Value Store
-   Getting Started with the Metadata Key-Value Store

<div>

On this page

</div>

<div>

<div>

# Getting Started with the Metadata Key-Value Store

</div>

**Available through:** *CLI, Python SDK, Rust SDK, API*

The Metadata KVS supports 4 primary operations `set`, `get`, `update`,
and `delete`.

### Setting a key-value pair​

You can set a key-value pair through any of the available interfaces by
supplying it with a key and a value. For more information see setting a
key-value pair.

<div>

-   CLI
-   cURL
-   Python

<div>

<div>

<div>

<div>

    seaplane metadata set <KEY> <VALUE>

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

For larger values such as public keys, large text files, or blobs of
data you can use `@-` to load values from STDIN.

<div>

<div>

    seaplane metadata set <key> @- < <file>

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

Alternatively, you can use `@<path/to/my/file>` to directly load values
from a file.

<div>

<div>

    seaplane metadata set <key> @<path/to/my/file>

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
    key=$(printf "<KEY>" | encode)
    value=$(printf "<VALUE>" | encode)

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
      key_value=KeyValue(b"<KEY>", b"<VALUE>")
    )

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

</div>

</div>

</div>

### Retrieving a key-value pair​

You can retrieve a key-value pair through any of the available
interfaces by supplying it with a key. For more information, see get a
key-value pair.

<div>

-   CLI
-   cURL
-   Python

<div>

<div>

Keys and values are sent to the API base64 encoded. If you have values
that can be printed to a terminal safely, use the `--decode` or `-D`
flag to automatically decode your key-value pair.

<div>

<div>

    seaplane metadata get <KEY> --decode

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
    key=$(printf "<KEY>" | encode)

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
    sea.metadata.get(key=Key(b'<KEY>'))

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

</div>

</div>

</div>

### Deleting a key-value pair​

Deleting a key-value pair permanently removes it from the coordination
service. All key-value pairs are strongly consistent and deployed on
multiple cloud services to survive outages and data loss. However,
Seaplane does not keep a record of deleted key-value pairs. Executing a
delete query on the Metadata KVS permanently removes the key-value pair.

For more information see, deleting a key-value pair.

<div>

-   CLI
-   cURL
-   Python

<div>

<div>

<div>

<div>

    seaplane metadata delete <KEY>

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
    key=$(printf "<KEY>" | encode)

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
    sea.metadata.delete(key=Key(b'<KEY>'))

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

</div>

</div>

</div>

## Working with the command line interface (CLI)​

<div>

<div>

</div>

</div>

</div>

<div>

<div>

**Tags:**

-   getting started
-   coordination

</div>

</div>

<div>

<div>

![](data:image/svg+xml;base64,PHN2Zz48Zz48cGF0aD48L3BhdGg+PC9nPjwvc3ZnPg==)Edit
this page

</div>

<div>

Last updated on **Jan 3, 2023** by **fokkedekker**

</div>

</div>

</div>

</div>

</div>

</div>
