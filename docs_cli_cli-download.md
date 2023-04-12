<div>

<div>

<div>

<div>

-   ![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)
-   Command Line Interface
-   Downloading and Installing the Seaplane CLI

<div>

<div>

# Downloading and Installing the Seaplane CLI

</div>

The Seaplane CLI tool is the go-to way to run your workloads on the
Seaplane Global Network and interact with our public APIs. The first
step is to download the tool, which is a single static binary, from our
Github release page.

The Seaplane CLI tool is supported on both x86_64, and arm64 for Linux
and macOS, as well as x86_64 Windows. Make sure you download the
appropriate archive for your system. This guide uses a macOS variant.

To extract and install the CLI from the `Downloads` directory, replace
`$ARCH` and `$VERSION` with whichever architecture and version you
downloaded from the release page (or simply copy over the filename) then
execute the command that matches your operating system:

-   For macOS use
    `sudo unzip ./seaplane-$VERSION-$ARCH.zip -d /usr/local/bin/`.
-   For Linux, use
    `sudo tar xzf ./seaplane-$VERSION-$ARCH.tar.gz -C /usr/local/`.

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

We can double check that it worked by typing `seaplane`, which should
display a help message similar to the one below.

It\'s okay if the help message looks a little bit different on your
machine, we\'re constantly iterating and working to improve our
products!

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
        shell-completion    Generate shell completion script files for Seaplane

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
