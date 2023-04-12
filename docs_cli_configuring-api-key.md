<div>

<div>

<div>

<div>

<div>

On this page

</div>

<div>

<div>

# Configuring Your API Key

</div>

To use the Seaplane CLI, you need to make sure `seaplane` knows about
your API key. We can do this in a few different ways:

-   Set `api_key = "..."` in our configuration file
-   Set the `SEAPLANE_API_KEY` environment variable
-   Use the `--api-key` CLI flag

The option you choose depends on your specific preferences and needs.
Each option has different security and override-ability considerations.

Each of these options overrides any option above. This means if you set
an API key in your configuration file, it can be overridden by setting
the environment variable or using the command line flag. This is helpful
if you need to change your API key for just a few invocations.

We generally recommend using the configuration file when possible.

### Storing the API key in the Configuration File​

We can use `seaplane account login` to store our API key in the
configuration file. We could also write the API key to the configuration
file manually, but then you have to *find* the configuration file, make
sure it\'s properly formatted, etc. It\'s easier to just let Seaplane
handle it for you!

You will be prompted to paste your API key, which is stored in the
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

### Security of `SEAPLANE_API_KEY` Environment Variable​

When the `seaplane` process executes, it\'s possible for some other
processes to see the environment that was given to `seaplane`. Generally
this requires elevated privileges, but that may not always be the case.

### Security of `--api-key` CLI Flag​

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

</div>

<div>

<div>

**Tags:**

-   CLI
-   getting started
-   API key

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
