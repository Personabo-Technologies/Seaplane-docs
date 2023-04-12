<div>

<div>

<div>

<div>

-   ![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)
-   Global Coordination Service
-   Coordination Directories

<div>

On this page

</div>

<div>

<div>

# Coordination Directories

</div>

**Available through:** *CLI, Python SDK, Rust SDK, API*

Directories are important tools for controlling the global behaviour of
information in all of the Seaplane Global Coordination Services. You can
use directories to set up business rules to restrict data to a
geographical area or specific provider.

## Creating Directories​

To create a directory, prepend the directory name and a `/` to your key
or lock name. For example:

-   `my-directory/my-key` automatically creates the `my-directory`
    directory.
-   `eu/my-eu-key` automatically creates the `eu` directory.
-   `aws/my-aws-key` automatically creates the `aws` directory.

Directories by themselves are nothing more than a way to sort your data.
They contain no information about data location or providers used. You
can read more about how to apply restrictions to a directory in
coordination restrictions.

### Nested Directories​

The Seaplane Global Coordination Services support the use of nested
directories. Similar to adding a directory to your existing key-value
pair, you can add nested directories by prepending your directory with
another directory name and `/`. For example:

-   `my-directory/my-nested-directory/my-key` automatically creates
    `my-nested-directory` inside `my-directory`.
-   `eu/ams/my_key` automatically creates the `ams` directory inside the
    `eu` directory.
-   `us/ca/sjc/my_keys` automatically creates the `sjc` directory inside
    the `ca` directory inside the `us` directory.

You can create multiple nested directories in one line (as long as you
stay below 128 bytes total key size!).

</div>

<div>

<div>

**Tags:**

-   global coordination service
-   restrictions

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
