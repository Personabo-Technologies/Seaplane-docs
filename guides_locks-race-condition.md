<div>

<div>

<div>

<div>

-   ![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)
-   Using Seaplane Locks to avoid race conditions

<div>

On this page

</div>

<div>

<div>

# Using Seaplane Locks to avoid race conditions

</div>

This guide outlines how to use the *Seaplane Locks Service* (*Locks*) to
avoid race conditions in your application or service, and the *Seaplane
Metadata Key-Value Store* (*MDKVS*) to store the state of the
application.

## The Example​

Let\'s say you\'re feeling festive and you want to build a system that
allows anyone in the world to turn the lights on the Rockefeller Center
Christmas tree in New York City on or off whenever they want. Obviously
this is a terrible (if extremely funny) idea, but real-world examples of
large distrbuted systems like in finance and tech are a bit too
complicated for a simple guide so please bear with us.

Building a globally distributed system, like the one you would need for
our Christmas lights example, creates a whole host of problems you can
solve using Seaplane. But in the interest of time, let\'s focus on one
common problem: race conditions.

<div>

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)note

</div>

<div>

**What Are Race Conditions?**

A race condition can occur when two or more processes access a shared
resource simultaneously, but the outcome of the process depends on the
order or state in which it began.

In our example, the state of the Christmas tree lights is presented as a
binary value: on or off. When pressed, the light switch inverts the
current state. The race condition occurs when two people around the
globe hit the light switch at the same time.

The outcome of the operation depends on the current state of the system.
If the lights are currently on, we expect them to turn off. If they are
off, we expect them to turn on. If we both flip the switch at the same
time\...nothing happens. Or, more precisely, the lights are turned on
and off in rapid succession leaving us in the original state and with
two very confused users.

The same problem occurs in large distributed systems when two processes
access the same resource at the same time. But instead of two confused
users, we (usually) end up with a broken system.

Fortunately, this is a problem we can solve using the *Seaplane Locks
Service*.

</div>

</div>

## Prerequisites​

To follow along you need:

-   Access to a Seaplane account and API key - you can create one here
-   A basic understanding of *Seaplane Locks* and the *MDKVS* services
    --- you can read more about them in our documentation
-   The Seaplane Javascript SDK installed on your machine --- you can
    install the SDK by running `npm install seaplane` in your terminal
    in the project directory

You can follow along with the project by checking out this GitHub repo.

## Sample Application​

We created a sample react app that mimics the system we described above.
It includes a Christmas tree and a light switch. Toggling the light
switch inverts the state of the Christmas tree lights.

A simple NodeJS backend serves the front end with two API endpoints:

-   `get_state` - Gets the current state of the Christmas lights
-   `set_state` - Inverts the state of the Christmas lights

Both endpoints use the *MDKVS* to set or get the current state of the
Christmas lights. The `set_state` endpoint uses the *Locks Service* to
avoid triggering race conditions.

### Configure MDKVS To Store State​

Before we crack into locks, we need to create a new record in the
*MDKVS* to store the current state of the Christmas tree lights. We\'re
using the *MDKVS* partially because it\'s a Seaplane product but
primarily because it\'s perfect for this use case. The *MDKVS* has
extremely low latencies all over the world, and its strong consistency
guarantees ensure that every user has the most recent and accurate state
of the system. You can learn more about the *MDKVS* in our documentation
here

To create a record storing the state of the Christmas tree lights on the
*MDKVS*, run this command in your terminal:

<div>

<div>

    seaplane metadata set christmast-tree-lights true

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

### Importing and Configuring the Seaplane JS SDK​

Import and configure the Seaplane JS SDK at the top of our `server.js`
file directly under the express app creation.

<div>

<div>

    const { Configuration, Metadata, Locks } = require("seaplane")

    // configure the Seaplane API key
    const config = new Configuration({ 
        apiKey: "my-super-secret-api-key"  
      })

    // create a metadata and locks instance
    const metadata = new Metadata(config)
    const locks = new Locks(config)

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

Let\'s pause here and take a closer look at what we just implemented. On
line one, we imported the Seaplane package. On line four, we configured
our Seaplane API key. If you\'re following along, be sure to replace
`my-super-secret-api-key` with your actual Seaplane API key. You can
copy your API key from the API Key section of Flightdeck.

Finally, on lines nine and ten, we created new instances of *Metadata*
and *Locks*.

### Creating the get_state Endpoint​

The `get_state` endpoint queries the *MDKVS* for the current state of
the Christmas lights. Because the *MDKVS* is strongly consistent, we
know the state that is returned will always be the correct state,
regardless of our physcial location.

<div>

<div>

    app.get('/get_state', async (res) => {
      
      // get the current state of the tree lights and convert to boolean
      let current_state = await metadata.get({key: "christmas-tree-lights"})

      // return the current state to the user
      res.send(current_state.value)

    });

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

Let\'s take another pause to examine what we just implemented. On line
four, we queried the *MDKVS* directly with our key:
`christmas-tree-lights`. The JS SDK automatically base64 encoded/decoded
both the key and the result which is required for the *MDKVS*.

Once we had our result, we sent that result back to the user using the
`send` request on line seven.

### Creating the set_state Endpoint​

Now, let\'s move on to the more interesting endpoint: `set_state`. This
endpoint inverts the current state of the Christmas lights i.e., on
becomes off and off becomes on. However, the state can only invert if it
acquires a lock. In other words, the switch can only be flipped if
nobody else is currently flipping the switch.

By preventing multiple people from attempting to turn the lights on or
off at the same time, we avoid the race condition problem we described
at the beginning of this guide. This way, everyone gets their turn to
flip the switch without breaking the system.

<div>

<div>

    app.get('/set_state', async (res) => {

        // try to acquire lock and set the state
        try {
            
            // construct lock name
            const name = { name: "light-switch" }

            // ask to acquire lock for 10 seconds
            const heldLock = await locks.acquire(name, "light-switcher", 10)

            // get the current state of the tree lights and convert to boolean
            let kv = await metadata.get({key: "christmas-tree-lights"}) 
            let current_state = (kv.value === 'true')

            // update the new state 
            await metadata.set({key: "christmas-tree-lights", value: String(!current_state)})

            // release the lock when we are done
            await locks.release(name, heldLock.id)

            // let the user know we successfuly flipped the switch
            res.send(!current_state)

        } catch (error) {

            // return 409 error if lock is alredy held
            if (error.status === 409) {
                res.send("Someone else is already flipping the lightswitch")

            } else {

                // log error to server if not locked already held
                console.log(error)
            }
        }
      });

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

Let\'s take one final pause to dissect our code sample.

Lines 4-36 wrap the entire lock request in a try-catch statement. This
gives us the opportunity to either acquire the lock or pass an error
code to the user if the lock request fails (which is to be expected when
the whole world is trying to press the button). The *Locks Service*
returns a code `409` if the lock acquisition was unsuccessful.

Lines 28-35 return the `409` error to the user and logs any other errors
to the server std output for debugging purposes.

We create the lock on lines seven and ten. First, we create a new lock
name: `light-switch`. Next, we request the lock with our lock name,
client ID (generally a UUID to see where this lock originated) and a
time-to-life in seconds (TTL). We set our TTL to ten seconds as this
gives us plenty of time to update the state (our tests will timeout way
before our lock expires). Once we complete our operations, we release
the lock on line 20. As we release the lock, we enable any other user or
service to complete their request.

Between acquiring (line 10) and releasing (line 20) the lock, we get the
current state of the lights and we invert its value (lines 12 -17) using
the *MDKVS*. This is similar to the operations performed in `get_state`.

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

### Connecting the Front-End and Back-End​

With both endpoints created, all we need to do is connect the front- and
back-end services which we set up ahead of time. Start your back-end
server by running `node server.js` in the `node-server` directory. Start
the front-end server by running `npm start` in the main directory.

To see the application live and in action, go to `localhost:3000`.

## Global Spread, Low-Latency Results​

And just like that, we\'ve given users all over the world the ability to
turn the the lights on the Rockefeller Center Christmas on and off at
will. In theory.

Obviously this demo is a bit silly, but it does highlight how to use the
*Seaplane Locks Service* to combat race conditions in your global
application or service. It\'s particularly useful for systems whose
global spread and performance requirements preclude the use of a more
standard locking solution that\'s hosted in a single location.

## What About PubSub?​

An astute reader may have noticed that while we use the *MDKVS* to store
the state of our Christmas tree, it does not update when the state is
changed by another user. Really, this is an excellent use case for the
*Seaplane Global PubSub* service. However, because *PubSub* is still in
development, we\'ll have to stick with the *MDKVS* for now.

If you\'d like early access to *Seaplane Global Pubsub* for your own
Christmas tree shennanigans, let us know by contacting support.

</div>

<div>

<div>

**Tags:**

-   locks
-   coordination
-   mdkvs

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
