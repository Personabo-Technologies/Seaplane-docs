<div>

<div>

<div>

<div>

-   ![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)
-   Global Coordination Service
-   Locks
-   Getting Started With Locks

<div>

On this page

</div>

<div>

<div>

# Getting Started With Locks

</div>

**Available through:** *CLI, Python SDK, Rust SDK, API*

The Seaplane Locks service provides three primary operations.

## Acquire a Lock​

Acquiring a lock creates a new lock in the Seaplane Locks service if it
is not already held by something else. To acquire a new lock you need:

-   A name for your lock
-   A client ID
-   A time to live

You will receive a lock ID and a value for the sequencer if the lock was
successful. For more information about releasing a lock with code
examples, see the API documentation.

## Renew a Lock​

Renewing a lock allows you to update the time to live or, in other
words, keep the lock alive. To renew a lock you need the following
pieces of information:

-   The existing lock name
-   The existing lock ID
-   A new TTL

It\'s useful to store this information when you acquire a new lock, but
if you don\'t have access to it you could acquire it by listing all your
locks. For more information, see: Getting information about a held lock.
For more information about releasing a lock with code examples, see the
API documentation.

## Release a Lock​

Seaplane automatically releases any lock that reaches its time to live.
In order to release a lock before its time to live expires, you can use
the release lock functionality. To release a lock manually, you need the
following pieces of information:

-   The existing lock name
-   The existing lock ID

Once you release a lock, you can no longer renew it or list it. For more
information about releasing a lock with specific code examples, see the
API documentation.

## Getting Information Around a Held Lock​

In general, only the holder of the lock has the lock ID which allows
them to release/renew the lock. However, there is an escape hatch which
may come in handy. Listing a lock returns all locks in the given
directory including all information Seaplane has about them i.e.,
`lock name`, `lock id`, `client-id`, `client-ip`, `TTL`.

You can list all locks in your account or all the locks in directories.
For more information on listing locks, see the API documentation.

</div>

<div>

<div>

**Tags:**

-   getting started
-   global coordination service
-   locks

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
