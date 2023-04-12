<div>

<div>

<div>

<div>

<div>

On this page

</div>

<div>

<div>

# Initializing Your Environment

</div>

This step is entirely optional as the Seaplane tool will auto-initialize
for you when possible. However, since we\'re still learning, let\'s go
ahead and do it manually to illustrate a few key configurable areas.

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

For example, on a Mac, the output looks like this:

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

However, we know that everyone is different and we do our best to be
respectful of each individual\'s preferences. There are multiple ways to
disable output coloring. We\'ll cover three of them here:

#### Setting `NO_COLOR`​

If you have the `NO_COLOR` environment variable set, your output will
not contain color. In fact, if you already had that variable set, the
previous message may not have been colored at all!

This cannot be overridden by `--color` flags.

#### Using `--no-color` or setting `--color=never`​

These can be set as a shell alias, or if you only want to remove color
from one particular invocation.

Setting an alias also allows you to override this choice on certain
invocations. For example, if you normally don\'t want any color, but
then later decide for a particular invocation you *do*, you can simply
pass `--color=always` which will override your alias.

#### Setting `color = "never"` in the configuration file​

Another method turning off color in output for all invocations is by
setting`color = "never"` in your configuration file. The configuration
file is called `seaplane.toml` and uses the \[TOML\] language. Its
location is platform dependent, which you can find by looking at the
output of the `seaplane init --verbose`. In our above example, it\'s
located here:
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

</div>

<div>

<div>

**Tags:**

-   CLI
-   getting started

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
