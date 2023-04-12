<div>

<div>

<div>

<div>

-   ![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)
-   Global Coordination Service
-   Metadata Key-Value Store
-   Metadata Key-Value Store Intro

<div>

On this page

</div>

<div>

<div>

# Metadata Key-Value Store Intro

</div>

**Available through:** *CLI, Python SDK, Rust SDK, API*

The Seaplane Metadata Key-Value Store (Metadata KVS) coordinates the
activities of your global application's component parts. It is a
distributed, reliable key-value store with strong consistency
guarantees. The Metadata KVS works by setting and reading keys and
values, like a simple database or dictionary data structure. The
Metadata Key-Value Store stores any data you\'d like up, to 1 MB per
record.

## What are key-value pairs​

Key-value pairs are the combination of a key and a value stored in the
Metadata KVS. The key is used to query the Metadata KVS to retrieve,
change, set or delete the value. You can store anything you\'d like in
the key, up to a maximum of 128 bytes. You can store anything you\'d
like in the value, up to a maximum of 1MB.

The keys support the use of directories. You can add a new directory by
prepending the key with your directory name. For example,
`my-directory/my-key` automatically creates the directory called
`my-directory`. For more information about directories, see Coordination
directories.

## Restrictions​

The Metadata KVS supports the use of provider and location restrictions.
For more information, see coordination restrictions.

</div>

<div>

<div>

**Tags:**

-   intro
-   global coordination service
-   key-value

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
