<div>

<div>

<div>

<div>

-   ![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)
-   Feature Flags with the Seaplane Metadata Key-Value Store.

<div>

On this page

</div>

<div>

<div>

# Feature Flags with the Seaplane Metadata Key-Value Store.

</div>

This guide outlines how to use the Seaplane Metadata Key-Value Store
(MDKVS) as a feature flag service. The MDKVS by design is a low-latency,
global, strongly consistent key-value store, making it ideal for use
cases including feature flags, service discovery, infrastructure
coordination and more.

There is a lot you can do with with the MDKVS, so if you\'re interested
in other use cases or just want learn more about the MDKVS, check out
our documentation.

While the MDKVS is great for feature flags, we\'re *also* working on a
dedicated feature flag product! You can request early access to feature
flags (and more!) here.

## What Are Feature Flags?​

Before we dive into the Seaplane MDKVS, let\'s talk about feature flags
more generally. If you\'re already familiar with feature flags and the
MDKVS, feel free to skip ahead to the project description and
requirements.

Feature flags, also known as \"feature toggles,\" are a tool that allow
you to run multiple feature branches in production at the same time
without re-deploying your application. A feature flag acts like a switch
where, if flipped on, enables a section of code in a live production
environment.

<div>

<div>

    if feature_flag:
      # excute this code if the feature flag is enabled
    else:
      # execute this code if the feature flag is disabled

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

You can have multiple feature flags enabled (or disabled) at the same
time, giving you the controlled flexibility you need to test new
features in production. Feature flags are often coupled with user
segments, allowing you to test new features for small groups of users
before making them generally available.

## What is the Seaplane Metadata KVS?​

The MDKVS is a strongly consistent, global key-value store that runs on
the Seaplane Global Network of clouds, bare metal, and edge. It has a
broad range of uses, but is primarily used to coordinate the activities
of global applications. Because it\'s a low-latency key-value service
with sub-40ms performance on all operations around the world, it\'s also
a great option for feature flags!

With such low global latencies, you might be wondering why we choose to
emphasize the MDKVS\'s strong consistency at all --- particularly for
feature flags. Consider the following scenario: you want to test a new
feature in production, so you enable a feature flag to push the new
feature live for all of your users at once. Because Murphy\'s Law is
real, something goes wrong and your service suddenly stops working. You
need to disable the feature immediately for all users in order to get
your app back up and running.

This is where strong consistency comes in handy. As soon as you disable
the feature flag on the MDKVS, it is also disabled for *all users
globally*. The day is saved! Meanwhile, if you use an eventually
consistent database, or if you cache the results, your broken feature
may be in production for hours depending on your cached result.

## Project Description and Requirements​

To keep things simple, we\'ll be using an existing example project
called Flaskex. We cloned the project from Github and made some small
changes for this guide. You can download our modified version of Flaskex
here.

## Implementation​

Before we get started, take a second to make sure you have:

-   A fully configured Seaplane account --- you can create one here
-   A basic understanding of the MDKVS --- you can read more about the
    API and the various SDKs here and here.
-   The latest version of the Seaplane Python SDK --- you can install
    the latest version by running `pip install seaplanekit` in your
    terminal.
-   The latest Seaplane CLI installed on your machine --- you can learn
    more about installing the Seaplane CLI here.

Once you have all of the above, go ahead and clone or fork our demo
project on Github.

### Creating Feature Flags in the MDKVS​

First, create two feature flag key-value pairs in your Seaplane account
using the CLI:

-   A feature flag to add the address field to the sign-up form
-   A feature flag to test a new button color

For our example, let\'s assume each feature flag was requested by a
different team. To keep things organized (especially if we plan on
making more feature flags in the future) we\'ll also make a directory
for each team. You can learn more about using directories in our
documentation.

To create our two example feature flags within their respective
directories, run the commands below. We initialize each feature flag as
`False` to avoid turning them on accidentally.

<div>

<div>

    seaplane metadata set sign-up-team/address-field False
    seaplane metadata set color-team/signup-button-blue False

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

### Importing and Using the Seaplane Python SDK​

With the feature flags set, we can start working on the app itself.
First, we\'ll make two minor changes to the app by adding our new
features, wrapping each change in an if statement that checks the value
of the feature flag key-value pair.

Then, we add the following two lines to the top of our `app.py` file to
import the Seaplane SDK (SDK).

<div>

<div>

    from seaplanekit import sea
    from seaplanekit.model import Key

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

Configure your API key by adding the following line to the top of our
`app.py` file under the Seaplane import statement. Make sure to replace
`my-super-secret-api-key` with your actual API key!

<div>

<div>

    sea.config.set_api_key("your-super-sectre-api-key")

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

Request both of the key-value pairs we just created, then store the
value in a variable. To avoid increasing average load time, only add
these lines to the `/` route where they\'re used.

<div>

<div>

    ff_add_address = sea.metadata.get(key=Key(b'sign-up-team/address-field')).value.decode()
    ff_button_blue = sea.metadata.get(key=Key(b'color-team/signup-button-blue')).value.decode()

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

The SDK returns our key-value pair as a `KeyValue` object. We use
`.value` to get the value directly and convert it from `base64` to a
`string` with `.decode`. To learn more about base64 encoding and the
MDKVS, have a look at our documentation.

Pass the variable you just created to the front end by adding them to
the return statement in our `/` route. Make sure you add them to the
right return statement, the one returning the `login.html` template. The
final return statement for the `login page` on `/` route should look
something like this:

<div>

<div>

    return render_template('login.html', form=form, ff_add_address=ff_add_address, ff_button_blue=ff_button_blue)

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

### Adding Feature Flags to Our Front-End Application​

Now that we have the feature flags available in our front-end template,
we can start using them to make changes based on their value. Like most
flask projects, we\'re using the Jinja template engine. We\'ll use Jinja
to create conditional code blocks based on feature flag values by
wrapping the code in an if/else statement.

Add the following code block to line *109* in `login.html`. This enables
an additional field for addresses when enabled.

<div>

<div>

    {% if ff_add_address == 'True' %}
    <div class="field">
      <p class="control has-icons-left has-icons-right">
        <input id="signup-address" class="input is-success" type="text" placeholder="address">
        <span class="icon is-small is-left">
          <i class="fa fa-user"></i>
        </span>
        <span class="icon is-small is-right">
          <i class="fa fa-check"></i>
        </span>
      </p>
    </div>
    {% endif %}

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

Once that\'s done, add the following code block to line *114* in
`login.html`. This changes the form button color to blue when enabled.

<div>

<div>

    {% if ff_button_blue == 'True' %}
      <a id="signup-button" class="form-button button is-primary is-inverted is-outlined button-blue">Sign Up</a>
    {% else %}
      <a id="signup-button" class="form-button button is-primary is-inverted is-outlined">Sign Up</a>
    {% endif %}

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

With the two feature flags in place, we now have granular control over
our website for these two specific features. We can turn them on by
running the following two commands with the Seaplane CLI, and turn them
off by replacing `True` with `False`:

<div>

<div>

    seaplane metadata set sign-up-team/address-field True
    seaplane metadata set color-team/signup-button-blue True

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

We wanted to keep this guide simple and approachable for makers of all
experience levels, but you can probably imagine how powerful these
feature flags can be when coupled with your existing developer platform.
Feature flags are **not** limited to front-end applications, and can be
used in a wide variety of contexts. You can use feature flags basically
anywhere you need granular control over your application in production
without the need to redeploy. The sky is the limit, and we\'ve built the
plane to get you there ;)

## Performance Testing​

We mentioned in the requirements section that we want our feature flags
to operate at super-low latency. This requirement is really important
for the end-user experience. Imagine enabling 10 feature flags on a page
where each flag takes 40ms to load. Those feature flags alone could add
an additional 0.4 seconds of load time to the page!

To make sure our latencies are as low as we need, we tested the MDKVS
from two testing machines in the EU. We tested each location
individually, then all at the same time. Each tested location measured
less than 40 ms of latency, both individually and concurrently (as you
can expect from a globally distributed key-value store)!

With performance like this, you don\'t have to worry about impacting the
end-user experience with your feature flags. We\'ll consider this
requirement met!

<div>

<table><thead><tr><th>Test type</th><th>Average latency MS</th><th>95th percentile latency MS</th><th># tests</th></tr></thead><tbody><tr><td>EU-central individual test</td><td>32</td><td>33</td><td>500</td></tr><tr><td>EU-west individual test</td><td>20</td><td>31</td><td>500</td></tr><tr><td>EU-central concurrent test</td><td>35</td><td>35</td><td>500</td></tr><tr><td>EU-west concurrent test</td><td>24</td><td>31</td><td>500</td></tr></tbody></table>

</div>

## Join Our Beta​

You can sign up for our MDKVS beta here if you want to give feature
flags a try, or if you have another use case in mind.

Until next time, thank you for flying with Seaplane!

</div>

<div>

<div>

**Tags:**

-   metadata
-   mdkvs
-   feature flags
-   metadata kvs

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
