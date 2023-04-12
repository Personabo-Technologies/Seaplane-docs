<div>

<div>

<div>

<div>

-   ![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)
-   Managed Global Database
-   Authentication

<div>

On this page

</div>

<div>

<div>

# Authentication

</div>

This document describes how to connect to *Seaplane Managed Global SQL*
(MGSQL). You can get access to *MGSQL* by signing up here.

### Postgress CLI authentication​

*Managed Global SQL* supports the standard login protocol of the
Postgress CLI. For instructions on how to install the Postgresql client
see postgresql.org. To connect with *MGSQL* run the following command in
your terminal on any machine that has the Postgress CLI installed:

<div>

<div>

     psql -h sql.cplane.cloud -U <USERNAME> -d <DATABASE>

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

Replace `<USERNAME>` with your username and `<DATABASE>` with your
database. Enter your password in the prompt that follows.

`sql.cplane.cloud` serves as the single entry point for *MGSQL* and
routes you to the closests available node. *MGSQL* automatically
optimizes node placement based on end-user traffic patterns and your
business rules.

### Using Other Interfaces​

*Seaplane Managed Global SQL* supports any interface that supports the
standard Postgress protocol. You can use the information below to
connect.

<div>

<table><thead><tr><th>Variable</th><th>Value</th></tr></thead><tbody><tr><td>host</td><td>sql.cplane.cloud</td></tr><tr><td>username</td><td>your username</td></tr><tr><td>password</td><td>your password</td></tr><tr><td>port</td><td>5432</td></tr><tr><td>database</td><td>your database name</td></tr></tbody></table>

</div>

</div>

<div>

<div>

**Tags:**

-   sql
-   authentication

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
