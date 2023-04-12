<div>

<div>

<div>

<div>

<div>

On this page

</div>

<div>

<div>

# Getting Started With Compute

</div>

This guide will walk through running your first workload on Seaplane.
We\'ve structured this guide with increasingly complex (read:
*real-world* ) examples.

<div>

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)tip

</div>

<div>

For the absolute fastest up-and-running version, please see Quickstart

</div>

</div>

## Prerequisites​

Before we begin, you\'ll need to:

-   Sign up for a Seaplane Account (we\'re currently in a closed beta,
    so you\'ll need an invitation link from the Seaplane team to create
    your account)
-   Copy the API key given to you after account creation (this can be
    requested here)

## Quickstart​

As a preview, and to demonstrate how simple it can be to spin up a
Seaplane workload, here is what it looks like to run
\[`nginxdemos/hello`\] on our platform.

You\'ll need:

-   Our single static binary for your particular platform and
    architecture
-   An API key you received from this form

We\'ll be combining multiple concepts we\'ll explain more fully later,
but here\'s a quick look at how simple it is to run your first
*formation* on our platform:

<div>

<div>

    $ seaplane formation plan \
      --api-key "mysuperspecialapikey" \
      --include-flight-plan "name=frontend,image=seaplane-demo/nginx:latest" \
      --public-endpoint /=frontend:80 \
      --launch 

    Successfully created Seaplane directories
    Successfully created local Flight Plan 'itchy-sweater' with ID '664954bb'
    Successfully created local Formation Plan 'few-actor' with ID 'b67437dc'
    Successfully launched remote Formation Instance 'few-actor' with remote Configuration UUIDs:
        'd87938b6-c57d-47c4-8037-6e69026008ac'

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

<div>

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)note

</div>

<div>

there are other, more secure ways to pass your API key!

</div>

</div>

To see it working:

<div>

<div>

    $ curl https://few-actor--seaplane-demo.on.cplane.cloud/ | head -n 4
    <!DOCTYPE html>
    <html>
    <head>
    <title>Hello World</title>

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

To learn more about what\'s happening in these commands, what else you
can do with them, and how to fully manage your fleet --- read on!

## Setup​

Here we\'ll:

-   Download and install the Seaplane CLI tool
-   Initialize our environment
-   Configure our API key
-   Test our setup

Let\'s begin!

### The Seaplane CLI Tool​

The Seaplane CLI tool is the go-to way to run your workloads on the
Seaplane network and interact with our public APIs. The first step is to
download the tool, which is a single static binary, from our Github
release page. You can request access here.

The CLI tool is supported on both x86_64, and arm64 for Linux and macOS,
as well as x86_64 Windows. Ensure you download the appropriate archive
for your system. This guide will be using a macOS variant as a
demonstration.

We\'ll assume the download was saved in the Downloads directory of your
home folder (`~/Downloads`). We need to extract the binary and place it
somewhere pointed to by your `$PATH` variable. On macOS and Linux
`/usr/local/bin/` is a good location.

<div>

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)note

</div>

<div>

You\'ll need to replace `$ARCH` and `$VERSION` with whichever
architecture and version you downloaded from the release page.

</div>

</div>

<div>

<div>

    $ cd ~/Downloads
    $ sudo unzip ./seaplane-$VERSION-$ARCH.zip -d /usr/local/bin/

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

We can ensure it worked by typing `seaplane`, which should display a
help message similar to the one below.

It\'s okay if the help message looks a little bit different on your
machine, we\'re constantly iterating and working to improve our product!

<div>

<div>

    $ seaplane
    seaplane v0.1.0 (f9f6dedab8)
    Seaplane IO, Inc.

    USAGE:
        seaplane [OPTIONS] <SUBCOMMAND>

    OPTIONS:
        -A, --api-key <STRING>    The API key associated with a Seaplane account used to access Seaplane API endpoints [env: SEAPLANE_API_KEY]
            --color <COLOR>       Should the output include color? [default: auto] [possible values: always, ansi, auto, never]
        -h, --help                Print help information
            --no-color            Do not color output (alias for --color=never)
        -q, --quiet               Suppress output at a specific level and below
        -v, --verbose             Display more verbose output
        -V, --version             Print version information

    SUBCOMMANDS:
        account             Operate on your Seaplane account, including access tokens [aliases: acct]
        flight              Operate on Seaplane Flights (logical containers), which are the core component of Formations
        formation           Operate on Seaplane Formations
        help                Print this message or the help of the given subcommand(s)
        image
        init                Create the Seaplane directory structure at the appropriate locations
        license
        shell-completion    Generate shell completion script files for seaplane

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

Success!

### Initialize our Environment​

This step is somewhat optional as the Seaplane tool will auto-initialize
for you when possible. However, since we\'re still learning, let\'s go
ahead and do it manually to illustrate a few key areas we can configure.

Here we will create all the necessary files and directories used by the
Seaplane CLI. These directories are platform-specific and are only
located within your home folder. We don\'t touch system files or
directories!

We can initialize everything with a single command:

<div>

<div>

    $ seaplane init
    Successfully created Seaplane directories

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

If you\'re curious which exact files and directories were created, you
can add the `--verbose` flag. If you\'ve already initialized, you can
safely re-run with the `--verbose` flag just to check. We won\'t
overwrite anything unless you tell us to.

For example, on a mac, the output looks like this:

<div>

<div>

    $ seaplane init --verbose
    Looking for configuration file at "/Users/kevin/Library/Application Support/io.Seaplane.seaplane/seaplane.toml"
    Found configuration file "/Users/kevin/Library/Application Support/io.Seaplane.seaplane/seaplane.toml"
    Looking for configuration file at "/Users/kevin/.config/seaplane/seaplane.toml"
    Looking for configuration file at "/Users/kevin/.seaplane/seaplane.toml"
    Creating directory "/Users/kevin/Library/Application Support/io.Seaplane.seaplane"
    Creating directory "/Users/kevin/Library/Application Support/io.Seaplane.seaplane"
    warn: "/Users/kevin/Library/Application Support/io.Seaplane.seaplane/seaplane.toml" already exists
    (hint: use 'seaplane init --overwrite=config to erase and overwrite it)

    warn: "/Users/kevin/Library/Application Support/io.Seaplane.seaplane/formations.json" already exists
    (hint: use 'seaplane init --overwrite=formations to erase and overwrite it)

    warn: "/Users/kevin/Library/Application Support/io.Seaplane.seaplane/flights.json" already exists
    (hint: use 'seaplane init --overwrite=flights to erase and overwrite it)

    Successfully created Seaplane directories

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

If you\'re following along live (because the above text doesn\'t do it
justice!) you probably noticed that last command added some color to the
output.

We believe that a tasteful coloring of words helps draw attention to
important parts of the output, which is especially critical for errors
and hints.

However we also know that everyone is different, so we do our best to be
respectful of each individual\'s preferences. There are multiple ways to
disable output coloring. We\'ll cover three of them here:

#### Setting `NO_COLOR`​

If you have the `NO_COLOR` environment variable set, your output will
not contain color. In fact, if you already had that variable set, the
previous message may not have been colored at all!

This cannot be overridden by `--color` flags.

#### Using `--no-color` or setting `--color=never`​

These can be set as a shell alias, or only if you want to remove color
from one particular invocation.

Setting an alias also allows you to override this choice on certain
invocations. For example, if you normally don\'t want any color, but
then later decide for a particular invocation you *do*, you can simply
pass `--color=always` which will override your alias.

#### Setting `color = "never"` in the configuration file​

Another method turning off color in output for all invocations is by
setting`color = "never"` in your configuration file. The configuration
file is called `seaplane.toml` and uses the \[TOML\] language. It\'s
location is platform dependent, which you can find by looking at the
output of the `seaplane init --verbose`. In our above example it\'s
located at
`~/Library/ApplicationSupport/io.Seaplane.seaplane/seaplane.toml`

If we add `color = "never"` under the `[seaplane]` table, our output
will no longer contain any color.

<div>

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)info

</div>

<div>

For more options see the Seaplane Configuration Specification

</div>

</div>

### Configure Your API Key​

The final setup step is to ensure `seaplane` knows about our API key. We
can do this in a few different ways:

-   Set `api_key = "..."` in our configuration file
-   Set the `SEAPLANE_API_KEY` environment variable
-   Use the `--api-key` CLI flag

Which option you choose depends on your preferences and needs. Each has
different security and override-ability considerations.

Each of these options overrides any option above, meaning if you set an
API key in your configuration file it can be overridden by setting the
environment variable or using the command line flag. This is helpful if
you need to change your API key for just a few invocations.

We generally recommend using the configuration file when possible.

#### Security of `SEAPLANE_API_KEY` Environment Variable​

When the `seaplane` process executes, it\'s possible for some other
processes to see the environment that was given to `seaplane`. Generally
this requires elevated privileges, but that may not always be the case.

#### Security of `--api-key` CLI Flag​

Like the environment variable when the `seaplane` process executes,
it\'s possible for some other processes to see command line flags given
to `seaplane`. Generally this requires elevated privileges, but that may
not always be the case.

However, unlike the environment variable, the `--api-key` flag supports
a more secure option of using the value `-` which means \"read the API
key from STDIN\" which is generally considered secure, and not viewable
by other processes on the same system.

For example, if the API key was stored in a file called `my_api_key.txt`
and using the short flag of `--api-key` of `-A`:

<div>

<div>

    $ cat my_api_key.txt | seaplane -A-

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

#### Storing the API key in the Configuration File​

We can use `seaplane account login` to store our API key in the
configuration file. We could also write the API key to the configuration
file manually, but then you have to *find* the configuration file, make
sure it\'s properly formatted, etc. It\'s easier to just let Seaplane
handle it!

You will be prompted to paste your API key which will be stored in the
appropriate location of the configuration file.

<div>

<div>

    $ seaplane account login
    Enter your API key below.
    (hint: it can be found [here](https://share.hsforms.com/1dc59Fr5vTdWIHdhplJl3sw4q18d))

    InlifethevisiblesurfaceoftheSpermWhaleisnottheleastamongthemanymarvelshepresents
    Successfully saved the API key!

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

### Test\!​

Now that we have a new API key installed, we can make sure it\'s
working.

To do that, we\'ll perform a silly test by asking our Access Token
endpoint for a new access token. In general you\'ll never need to
interact with this feature directly. However, internally and especially
in our SDK, this is used quite frequently. If this works, we\'ll know
that everything is installed correctly.

<div>

<div>

    $ seaplane account token
    eyJ0eXAiOiJKV1QiLCJraWQiOiJhdXRoLXNpZ24ta2V5LTEiLCJhbGciOiJFZERTQSJ9.eyJpc3MiOi
    Jpby5zZWFwbGFuZXQuZmxpZ2h0ZGVjayIsImF1ZCI6ImlvLnNlYXBsYW5ldCIsInN1YiI6IklubGlmZ
    XRoZXZpc2libGVzdXJmYWNlb2Z0aGVTcGVybVdoYWxlaXNub3R0aGVsZWFzdGFtb25ndGhlbWFueW1h
    cnZlbHNoZXByZXNlbnRzIiwiaWF0IjoxNjQ2NzUzODIwLCJleHAiOjE2NDY3NTM4ODAsInRlbmFudCI
    6IklubGlmZXRoZXZpc2libGVzdXJmYWNlb2Z0aGVTcGVybVdoYWxlaXNub3R0aGVsZWFzdGFtb25ndG
    hlbWFueW1hcnZlbHNoZXByZXNlbnRzIiwic2NvcGUiOiIifQ.epUyBWDiU2N6C7CBM7gnZPqoixd_ZH
    dB8Khn_1BKwnjNxJaIba9PC9YTuDwYaFVs17aCWhY-oRDPpmo87YBrDQ

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

The access token is just a JWT and allows access to our public APIs
derived from your API key. These tokens are only valid for a very short
period of time. The token above should be *long* expired by the time you
read this.

Congratulations! You now have a working Seaplane CLI ready to run some
fantastic workloads!

## Running Your Workload​

------------------------------------------------------------------------

In this chapter we will run our first workload on the Seaplane Cloud.

We will:

-   Upload a Container Image
-   Create and Launch a *Formation* With a Single *Flight*
-   See a Hello World Page

Let\'s get started!

### Upload a Container Image​

You must first upload your container image to the Seaplane Cloud
Registry. We\'ll be using the \[`nginxdemos/hello`\]

### Create and Launch a Formation with a Single Flight​

In Seaplane a *formation* can be thought of as your application or
service. *Formations* are made up of *flights* which are logical
containers.

Using the Seaplane CLI we can create a *formation* and *flight plans*.
These *plans* are merely local definitions, so we can edit them, copy
them, change them, and do pretty much anything we need to them.

Once we\'re satisfied with our local *formation plan* we can `launch`
it; sending it to the Seaplane Cloud and creating a Remote Instance from
our local *plan* definition. Once launched, Seaplane can activate it for
receiving public traffic.

We can also both `plan` and `launch` all in one go, if desired.

For our first command we\'ll do it all in one go, so you can see just
how effortlessly it can happen. Then we\'ll break down the components
into different *plans*.

#### Your First Workload​

We\'ll be running the `nginx-hello` container from earlier.

<div>

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)note

</div>

<div>

Here `seaplane-demo` is our Tenant ID. You\'ll need to replace that with
your own tenant ID. :::

<div>

<div>

    $ seaplane formation plan \
      --include-flight-plan "image=seaplane-demo/nginxdemos/hello:latest" \
      --launch
    Successfully created local Flight Plan 'mellow-order' with ID 'd4e877b7'
    Successfully created local Formation Plan 'nimble-bike' with ID '8bfe5304'
    Successflly launched remote Formation Instance 'nimble-bike' with Configuration UUIDs:
        'c534dc48-03b6-400b-bc19-f7d28cfd1897'

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

That\'s it!

Notice it created local *flight* and *formation plans* for us, and
because we didn\'t specify anything it also gave them some randomly
assigned names.

If all you want is to see that it worked, skip down to the See Hello
World Page section below.

#### Working with Local *Flight Plans*​

*Flight plans* are included in *formation plans* (which we\'ll explore
in depth later on) and tell Seaplane what kind of containers you\'d like
to use for your workload.

*Flight plans* on their own are nothing more than a local definition to
be referenced in *formations*.

We can see our *flight plans*:

<div>

<div>

    $ seaplane flight list
    LOCAL ID  NAME          IMAGE                             MIN  MAX  ARCH  API PERMS
    d4e877b7  mellow-order  seaplane-demo/nginx-hello:latest  1    INF  auto  false

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

Notice there are multiple default values because we didn\'t specify
anything other than an `image`.

Let\'s say we wanted to edit this *flight plan* to include in another
*formation*. We could create an entirely new *flight plan* via the
`seaplane flight plan` command, but instead let\'s just edit the one
we\'ve already made.

Let\'s say that we actually don\'t want to be running the `:latest` tag,
we *actually* want to pin to a specific digest. We\'ll use
`sha256:33d30466bb608f607a8d708d39bf13ec7a908dde1a8a8b228f7f3f4c6a4d1bdf`
for this example.

</div>

</div>

Even when you specify `:latest` Seaplane pins your containers to the
last digest at that point so that your *flights* don\'t change out from
under you. The example commands are somewhat contrived and purely to
demonstrate the CLI and how to use it.

:::

When using `seaplane flight edit` we must specify a `NAME` or an `ID`
(local ID) that we want to edit, along with any parameters we\'d like to
change.

<div>

<div>

    $ seaplane flight edit d4e8 --image seaplane_demo/nginxdemos/hello@sha256:33d30466bb608f607a8d708d39bf13ec7a908dde1a8a8b228f7f3f4c6a4d1bdf

    $ seaplane flight list
    LOCAL ID  NAME              IMAGE                                                                                                   MIN  MAX  ARCH  API PERMS
    d4e877b7  mellow-order      seaplane_demo/nginxdemos/hello@sha256:33d30466bb608f607a8d708d39bf13ec7a908dde1a8a8b228f7f3f4c6a4d1bdf  1    INF  auto  false

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

**IMPORTANT:** This edit *only affects our local flight plan
definition*! Nothing has changed to our running Formation Instance in
the Seaplane Cloud. Remember, once a remote instance has been created
from a local definition, the two are disconnected from one another.
It\'s like building a house from a set of blueprints --- changing the
blueprints doesn\'t change any previously built house!

Notice we used part of the `LOCAL ID` to reference our *flight plan*. We
also could have done so by unambiguous partial name, or full name if
desired.

Hmm. You know what? Actually, I think we *do* want another *flight plan*
that points to the `:latest` tag. We can copy `mellow-order` and just
make that one change.

<div>

<div>

    $ seaplane flight copy mellow-order --image seaplane_demo/nginxdemos/hello:latest
    Successfully copied Flight 'mellow-order' to new Flight 'waiting-leaf' with ID '2d331f0c'

    $ seaplane flight list
    LOCAL ID  NAME          IMAGE                                                                                                   MIN  MAX  ARCH  API PERMS
    d4e877b7  mellow-order  seaplane_demo/nginxdemos/hello@sha256:33d30466bb608f607a8d708d39bf13ec7a908dde1a8a8b228f7f3f4c6a4d1bdf  1    INF  auto  false
    2d331f0c  waiting-leaf  seaplane_demo/nginxdemos/hello:latest                                                                   1    INF  auto  false

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

There are a bunch of other things you can do with your *flights* as
well, see the `seaplane flight --help` for details.

#### Working with Local Formation Plans​

Remember that we said a *formation* is made up of *flights*? Technically
a *formation* is made up of zero or more *formation configurations*.
These configurations define what *flights* are utilized by referencing
one or more *flight plan* definitions, and how they\'re allowed to
communicate/scale. A *formation* can have zero or more configurations
because having multiple configurations will allow you to load balance
between them. This empowers things like blue/green deployments, atomic
upgrades, and all kinds of nifty tools.

<div>

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)caution

</div>

<div>

Remember, a *remote instance* is created from a *local plan*. Much like
building a house from a set of blueprints. We can change the blueprints
and create a second, slightly different house from the altered
blueprints. However, changing the blueprints used to build a house
won\'t retroactively change a house that was already built. The same
logic applies to altered local *formation plans* and remote *formation
instances* created from them.

</div>

</div>

All that said, let\'s keep it simple. We\'re just working with a single
Configuration, which only references a single *flight plan* --- no load
balancing or other complexities. That will come in future chapters!

The reason we\'re bringing up this distinction now is, if you\'re
looking for something similar to `seaplane flight list` but for
*formations*; you\'ll find it\...but it might not make sense. At least,
it won\'t make sense without knowing about *formation configurations*.

So, let\'s check that command now:

<div>

<div>

    $ seaplane formation list
    LOCAL ID  NAME         LOCAL  DEPLOYED (GROUNDED)   DEPLOYED (IN AIR)   TOTAL CONFIGURATIONS
    8bfe5304  nimble-bike  1      0                     1                   1

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

Notice how it says we have one `LOCAL`, and one `DEPLOYED (IN AIR)`?
What do those mean? The `seaplane formation list` is telling you how
many *formation configurations* it knows about, and what their status
is. In the example above, it\'s saying we have one local definition
(which we created earlier), and one `DEPLOYED (IN AIR)` which means,
\"Uploaded to the Seaplane Cloud (Deployed) and set to active (In
Air).\"

Let\'s create another *formation plan*, without any configuration so you
can better see the distinction.

<div>

<div>

    $ seaplane formation plan
    Successfully created local Formation Plan 'kind-week' with ID '86bd3a0c'

    $ seaplane formation list
    LOCAL ID  NAME         LOCAL  DEPLOYED (GROUNDED)   DEPLOYED (IN AIR)   TOTAL CONFIGURATIONS
    8bfe5304  nimble-bike  1      0                     1                   1
    86bd3a0c  kind-week    0      0                     0                   0

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

Now we have a new *formation plan*! A perfectly useless *formation* with
no included *flight plans*, but progress is progress!

Okay, let\'s delete that empty *formation plan* and create one that\'s a
little more useful. This time when we create the *formation plan* we
*will not* be automatically deploying it to the Seaplane Cloud, so you
can see how to do that manually.

<div>

<div>

    $ seaplane formation delete kind-week
    Deleted local Formation Plan 86bd3a0c72cfc6e6ea2c4c7c37766361d801ea84bab72ae42d4b0d86afd42217

    Successfully removed 1 item

    $ seaplane formation plan --include-flight-plan mellow-order
    Successfully created local Formation Plan 'festive-winter' with ID 'd3aa195d'

    $ seaplane formation list
    LOCAL ID  NAME            LOCAL  DEPLOYED (GROUNDED)   DEPLOYED (IN AIR)   TOTAL CONFIGURATIONS
    8bfe5304  nimble-bike     1      0                     1                   1
    d3aa195d  festive-winter  1      0                     0                   1

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

Now, notice that `festive-winter` has one local configuration, but none
that are currently deployed. Let\'s change that by deploying an Instance
of this *formation plan* but without setting it to active.

<div>

<div>

    $ seaplane formation launch --grounded festive-winter
    Successfully launched remote Formation Instance 'festive-winter'

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

The `--grounded` flag tells Seaplane to not set the configuration to
active, so no actual traffic can reach it.

We can see this by looking at the *formations* again:

<div>

<div>

    $ seaplane formation list
    LOCAL ID  NAME            LOCAL  DEPLOYED (GROUNDED)   DEPLOYED (IN AIR)   TOTAL CONFIGURATIONS
    8bfe5304  nimble-bike     1      0                     1                   1
    d3aa195d  festive-winter  1      1                     0                   1

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

But let\'s say we do want to start up that configuration and make it
active. We can actually just re-pass the same command but without the
`--grounded` flag. This will make all configurations for a given local
*formation plan* active (though we only have one right now).

<div>

<div>

    $ seaplane formation launch festive-winter
    Successfully launched remote Formation Instance 'festive-winter'

    $ seaplane formation list
    LOCAL ID  NAME            LOCAL  DEPLOYED (GROUNDED)   DEPLOYED (IN AIR)   TOTAL CONFIGURATIONS
    8bfe5304  nimble-bike     1      0                     1                   1
    d3aa195d  festive-winter  1      0                     1                   1

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

Notice `festive-winter` has changed its configuration status from
`GROUNDED` to `IN AIR`!

### See a Hello World Page​

If you\'ve been following along, you currently have two *formations*
running. If you jumped from the Your First Workload you\'ll only have
one.

So we\'ll test out the first one, and leave the second as an experiment
for the reader.

If all went according to plan, you should have a container image running
and addressable from:

<div>

<div>

    $ curl https://nimble-bike--seaplane-demo.on.cplane.cloud/ | head -n 4
    <!DOCTYPE html>
    <html>
    <head>
    <title>Hello World</title>

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

Yay 😁

</div>

<div>

<div>

**Tags:**

-   getting started
-   compute

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
