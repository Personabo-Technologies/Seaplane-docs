<div>

<div>

<div>

<div>

-   ![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)
-   Global Coordination Service
-   Base64 Encoding

<div>

<div>

# Base64 Encoding

</div>

Seaplane requires you to encode (and decode) any values used for keys,
values, or directories in all of the Seaplane Global Coordination
Services (Metadata Key-Value Store and Locks). Most Seaplane interfaces,
such as the SDKs and CLI, have built-in encoders and decoders.

-   **Python SDK** - Automatically encodes and decodes values.
-   **CLI** - Automatically encodes values. Decoding is available
    through the `--decode` or `-D` flag.

If you are using the API directly --- for instance through cURL --- you
are responsible for encoding and decoding values.

The default Perl install on most Linux and Mac OS machines have built-in
support for base64 encoding and decoding. You can use the following two
functions for any cURL examples on the Seaplane documentation site.

<div>

<div>

    function encode {
        perl -MMIME::Base64=encode_base64url -ne 'print encode_base64url($_),"\n"' 
    }

    function decode {
        perl -MMIME::Base64=decode_base64url -ne 'print decode_base64url($_),"\n"'
    }

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

</div>

<div>

<div>

**Tags:**

-   base64
-   global coordination service

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
