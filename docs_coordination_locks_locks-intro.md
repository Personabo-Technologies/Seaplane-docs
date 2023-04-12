<div>

<div>

<div>

<div>

-   ![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)
-   Global Coordination Service
-   Locks
-   Locks Intro

<div>

On this page

</div>

<div>

<div>

# Locks Intro

</div>

**Available through:** *CLI, Python SDK, Rust SDK, API*

## What Are Locks​

Locks are a way for globally distributed systems to \"lock\" a resource
so only one entity can edit or use that resource at the same time. You
can think of it as a database lock, but on a much broader scale. Where a
database lock locks access to a single table or database row, the
Seaplane locks service allows you to lock any resource.

Seaplane locks have five key components which we\'ll cover here:

-   Name
-   Lock ID
-   TTL (time to live)
-   Client ID
-   Sequencer

<div>

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)caution

</div>

<div>

It\'s important to note that the locks service is purely advisory. The
locks themselves do not protect any actual data or state --- it\'s up to
your clients to cooperatively respect the locks. If you can\'t acquire
\"my-database-lock\", then don\'t access the database!

</div>

</div>

## Guarantees and Reasoning About Locks​

Distributed locks suffer from many of the same issues as distributed
systems in general, especially around time. If you acquire a 100s lock,
you might think you could start a timer in your program and know that
you have exclusive access to the protected resource for 100s. However,
it\'s not quite that simple.

There could\'ve been an arbitrary length network delay in the packet
containing your lock, or your thread could be put to sleep since you
started the timer. Either could cause issues. Luckily, within normal
network conditions, you can be fairly safe using conservatively long
locks with conservatively short heartbeats.

For example, acquiring a 100s lock and renewing it every 10s would be
very unlikely to cause an issue.

The longer the TTL of the lock and the shorter your heartbeat interval,
the safer the lock will be. Unfortunately, it may also be a bit more
annoying to work with! You should tailor these factors to you, based on
the severity of a breach of the lock invariants.

If a lock is thought to be acquired by 2 separate processes, how bad
will that be for you? Is it just wasted cycles of 2 separate processes
processing the same image, or could it cause something far worse?
Ultimately, you\'ll need to strike the right balance of safety and ease
of use.

## Lock Attributes​

### Lock Name​

Each lock has a name with which it can be acquired. This name can be any
arbitrary bytes, though we suggest you stick to readable characters and
semantically meaningful names. You can namespace locks for
organizational purposes by using the / character as a separator, similar
to directories in a filesystem.

### Locks IDs​

When you successfully acquire a lock using its name, you\'ll be given a
lock ID. This lock ID is similar to a handle that allows you to renew or
release the lock. The lock ID has no meaning itself, and once your lock
expires, the ID will become useless.

As long as no other client of yours knows this ID, you know the lock
can\'t be renewed or released unless you choose to do so. (There is a
way for other clients to find out the lock ID just in case, but
generally speaking this point stands).

### Time to Live (TTL)​

All locks are eventually released, either by the user or automatically
after a certain user-given time-to-live (TTL) has passed. This is to
protect against the client that is currently holding the lock from being
unable to release it for whatever reason (the client dies, the network
fails etc.) We\'ll see later how you can keep a lock alive if you need
long-term access.

### Client ID​

Each lock has a client ID. This ID indicates who or what process
currently holds the lock. In general, only the person or process holding
the client-id (and lock-id) should renew or release a lock.

### Sequencer Token​

If the external resource a lock is protecting can also be involved in
the coordination, then we can win back some guarantees. To do this, the
Locks service provides a sequencer token, and this token is guaranteed
to increment each time the lock goes from free → held.

If you can configure your external service to only accept a request if
it's accompanied by a token that is higher than any other token it
received for such a request, you can guarantee that requests to the
external service don't happen out of order.

## Restrictions​

The Locks service supports the use of provider and location
restrictions. For more information, see coordination restrictions.

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

Last updated on **Dec 22, 2022** by **fokkedekker**

</div>

</div>

</div>

</div>

</div>

</div>
