<div>

<div>

<div>

<div>

-   ![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)
-   Building an example app with Vercel, Prisma and Seaplane SQL

<div>

On this page

</div>

<div>

<div>

# Building an example app with Vercel, Prisma and Seaplane SQL

</div>

## Introduction​

Jamstack websites are a popular way of deploying web pages with
performance in mind. Platforms such as Netlify and Vercel allow their
users to deploy pages on a CDN-based edge network. While this is great
for front-end pages one critical part is still missing from the JAMstack
ecosystem. The database!

Almost all modern web applications have a database component, but due to
the complexity of deploying global multi-region databases, this data
component is often hosted in a single cloud region, wiping out most
performance gains every time the database is called.

> Using a traditional relational database in multiple regions is
> possible, but potentially costly or difficult to maintain. You might
> consider keeping your relational database regional and only
> replication critical data to edge regions, otherwise keeping most
> workloads running in the single region with regionally located
> compute. \-- from the Vercel documenation

In this tutorial, we are solving that problem by using Seaplane Managed
Global SQL (*MGSQL*) and global tables. Global tables automatically
deploy and replicate data where your users are, constantly optimizing
the underlying infrastructure to better match end-user usage patterns.
Providing sub-25ms latency reads everywhere on the planet. Specifically,
we are implementing this Vercel + Prisma tutorial.

## Getting started​

First things first, you need a Seaplane SQL account. You can request
access here. Make sure to schedule a quick call to skip the line.

You also need a Vercel, Prisma, and Github account.

## Creating the project​

You can clone the example project here. To save you some time we
implemented all the steps that have nothing to do with the data
component.

## Creating the database and setting up Prisma​

Run `prisma init` in your project root directory to initiate Prisma.
This creates two files `prisma/schema.prisma` and `.env`.

Copy the project model definition listed below to the `schema.prisma`
file. You are using the model definition later to create the database
schema.

schema.prisma

<div>

<div>

<div>

<div>

    // This is your Prisma schema file,
    // learn more about it in the docs: https://pris.ly/d/prisma-schema

    datasource db {
    provider = "postgresql"
    url      = env("DATABASE_URL")
    shadowDatabaseUrl = env("SHADOW_DATABASE_URL")

    }

    generator client {
    provider = "prisma-client-js"
    }

    model Post {
    id        String  @id @default(cuid())
    title     String
    content   String?
    published Boolean @default(false)
    author    User?   @relation(fields: [authorId], references: [id])
    authorId  String?
    }

    model Account {
    id                 String  @id @default(cuid())
    userId             String  @map("user_id")
    type               String
    provider           String
    providerAccountId  String  @map("provider_account_id")
    refresh_token      String?
    access_token       String?
    expires_at         Int?
    token_type         String?
    scope              String?
    id_token           String?
    session_state      String?
    oauth_token_secret String?
    oauth_token        String?

    user User @relation(fields: [userId], references: [id], onDelete: Cascade)

    @@unique([provider, providerAccountId])
    @@map("accounts")
    }

    model Session {
    id           String   @id @default(cuid())
    sessionToken String   @unique @map("session_token")
    userId       String   @map("user_id")
    expires      DateTime
    user         User     @relation(fields: [userId], references: [id], onDelete: Cascade)

    @@map("sessions")
    }

    model User {
    id            String    @id @default(cuid())
    name          String?
    email         String?   @unique
    emailVerified DateTime? @map("email_verified")
    image         String?
    createdAt     DateTime  @default(now()) @map(name: "created_at")
    updatedAt     DateTime  @updatedAt @map(name: "updated_at")
    posts         Post[]
    accounts      Account[]
    sessions      Session[]

    @@map(name: "users")
    }

    model VerificationToken {
    identifier String
    token      String   @unique
    expires    DateTime

    @@unique([identifier, token])
    @@map("verificationtokens")
    }

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

</div>

</div>

Configure the database connection by adding the following two lines to
your `.env` file.

In addition to the Seaplane Database, add a local shadow database. The
shadow database is required to detect problems such as schema drift. We
recommend using a local Postgres database. You currently can not use
*MGSQL* for your shadow database.

<div>

<div>

    DATABASE_URL="postgresql://<YOUR-USERNAME>:<YOUR-PASSWORD>@sql.cplane.cloud:5432/<YOUR-DB-NAME>?schema=public"
    SHADOW_DATABASE_URL="postgresql://<YOUR-USERNAME>:<YOUR-PASSWORD>@localhost:5432/<YOUR-DB-NAME>?schema=public"

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

Run the following command to turn `schema.prisma` into a SQL schema.
Upon completion, Prisma generates a `schema.sql` file in
`prisma/migrations/timestamp_prisma_db`.

<div>

<div>

    prisma migrate dev --create-only

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

<div>

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)caution

</div>

<div>

Running `prisma migrate dev --create-only` can take a few minutes to
complete.

</div>

</div>

Add the following three lines to each table create statement to turn
them into global tables. For your convenience, we have included the
finished `schema.sql` file below. You can learn more about global tables
in our documentation.

<div>

<div>

    WITH (
        seaplane_table_type=global
        );

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

schema.sql

<div>

<div>

<div>

<div>

    -- CreateTable
    CREATE TABLE "Post" (
      "id" TEXT NOT NULL,
      "title" TEXT NOT NULL,
      "content" TEXT,
      "published" BOOLEAN NOT NULL DEFAULT false,
      "authorId" TEXT,

      CONSTRAINT "Post_pkey" PRIMARY KEY ("id")
    ) WITH (
      seaplane_table_type=global
      );

    -- CreateTable
    CREATE TABLE "accounts" (
      "id" TEXT NOT NULL,
      "user_id" TEXT NOT NULL,
      "type" TEXT NOT NULL,
      "provider" TEXT NOT NULL,
      "provider_account_id" TEXT NOT NULL,
      "refresh_token" TEXT,
      "access_token" TEXT,
      "expires_at" INTEGER,
      "token_type" TEXT,
      "scope" TEXT,
      "id_token" TEXT,
      "session_state" TEXT,
      "oauth_token_secret" TEXT,
      "oauth_token" TEXT,

      CONSTRAINT "accounts_pkey" PRIMARY KEY ("id")
    ) WITH (
      seaplane_table_type=global
      );

    -- CreateTable
    CREATE TABLE "sessions" (
      "id" TEXT NOT NULL,
      "session_token" TEXT NOT NULL,
      "user_id" TEXT NOT NULL,
      "expires" TIMESTAMP(3) NOT NULL,

      CONSTRAINT "sessions_pkey" PRIMARY KEY ("id")
    ) WITH (
      seaplane_table_type=global
      );

    -- CreateTable
    CREATE TABLE "users" (
      "id" TEXT NOT NULL,
      "name" TEXT,
      "email" TEXT,
      "email_verified" TIMESTAMP(3),
      "image" TEXT,
      "created_at" TIMESTAMP(3) NOT NULL DEFAULT CURRENT_TIMESTAMP,
      "updated_at" TIMESTAMP(3) NOT NULL,

      CONSTRAINT "users_pkey" PRIMARY KEY ("id")
    ) WITH (
      seaplane_table_type=global
      );

    -- CreateTable
    CREATE TABLE "verificationtokens" (
      "identifier" TEXT NOT NULL,
      "token" TEXT NOT NULL,
      "expires" TIMESTAMP(3) NOT NULL
    ) WITH (
      seaplane_table_type=global
      );

    -- CreateIndex
    CREATE UNIQUE INDEX "accounts_provider_provider_account_id_key" ON "accounts"("provider", "provider_account_id");

    -- CreateIndex
    CREATE UNIQUE INDEX "sessions_session_token_key" ON "sessions"("session_token");

    -- CreateIndex
    CREATE UNIQUE INDEX "users_email_key" ON "users"("email");

    -- CreateIndex
    CREATE UNIQUE INDEX "verificationtokens_token_key" ON "verificationtokens"("token");

    -- CreateIndex
    CREATE UNIQUE INDEX "verificationtokens_identifier_token_key" ON "verificationtokens"("identifier", "token");

    -- AddForeignKey
    ALTER TABLE "Post" ADD CONSTRAINT "Post_authorId_fkey" FOREIGN KEY ("authorId") REFERENCES "users"("id") ON DELETE SET NULL ON UPDATE CASCADE;

    -- AddForeignKey
    ALTER TABLE "accounts" ADD CONSTRAINT "accounts_user_id_fkey" FOREIGN KEY ("user_id") REFERENCES "users"("id") ON DELETE CASCADE ON UPDATE CASCADE;

    -- AddForeignKey
    ALTER TABLE "sessions" ADD CONSTRAINT "sessions_user_id_fkey" FOREIGN KEY ("user_id") REFERENCES "users"("id") ON DELETE CASCADE ON UPDATE CASCADE;

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

</div>

</div>

You are now ready to create the global tables on *MGSQL*. Run the
following command in your `prisma/migrations/timestamp_prisma_db`
directory to create the tables. Replace `<USERNAME>` with your username
and `<DATABASE>` with your database name.

<div>

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)note

</div>

<div>

Generating new tables on Seaplane SQL can take a few minutes as we
provision the infrastructure globally.

</div>

</div>

<div>

<div>

    psql -h sql.cplane.cloud -U <USERNAME> -d <DATABASE> < schema.sql

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

## Enabling edge functions​

*MGSQL* automatically places read replicas where needed based on
end-user traffic. In practice, this means wherever your database
requests are coming from; in other words, where your API is hosted. By
default, your functions on Vercel are hosted in a single location.

Without making any changes, end-users would not benefit from the low
latency *MGSQL* provides unless you deploy both the data and compute
components on the edge.

Luckily, Vercel provides us with an easy way to run compute functions on
the edge.

Add the following code snippet to the top of each function file listed
below to turn them into edge functions. We already added the snippet to
our example repo, so there is nothing left to do but deploy your app.

-   `pages/api/auth/[...nextauth.ts]`
-   `pages/api/post/[id].ts`
-   `pages/api/post/index.ts`
-   `pages/api/publish/[id].ts`
-   `pages/p/[id].tsx`

<div>

<div>

    export const config = {
      runtime: 'edge'
    };

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

For example, your `pages/api/auth/[...nextauth.ts]` file will look
something like this.

<div>

<div>

    import { NextApiHandler } from 'next';
    import NextAuth from "next-auth";
    import { PrismaAdapter } from '@next-auth/prisma-adapter'
    import GitHubProvider from 'next-auth/providers/github'
    import prisma from '../../../lib/prisma';

    const authHandler: NextApiHandler = (req, res) => NextAuth(req, res, options);
    export default authHandler;

    // enabling edge functions
    export const config = {
      runtime: 'edge'
    };

    const options = {
      providers: [
        GitHubProvider({
          clientId: process.env.GITHUB_ID,
          clientSecret: process.env.GITHUB_SECRET,
        }),
      ],
      adapter: PrismaAdapter(prisma),
      secret: process.env.SECRET,
    };

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

## Setting up Github auth​

The demo project uses Github authentication to authenticate users. Head
over to Github OAuth Apps and create a new App.

For local development, the authorization URL should be
`http://localhost:3000/api/auth`. You have to update this when you
deploy the app on Vercel since the deployment URL is different.

Click register application. This generates your `GITHUB_ID` and
`GITHUB_SECRET`. Make sure to copy the secret as it is only shown once.

Copy the `GITHUB_ID`, `GITHUB_SECRET` and `NEXTAUTH_URL` to your `.env`
file as follows.

<div>

<div>

    # GitHub OAuth
    GITHUB_ID=<YOUR-GITHUB-ID>
    GITHUB_SECRET=<YOUR-GITHUB-SECRET>
    NEXTAUTH_URL=http://localhost:3000/api/auth

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

## Running your app locally​

You are now ready to run your app locally. In your project root
directory run `npm run dev`. Navigate to `localhost:3000` to confirm
everything is working 🎉.

## Deploying on Vercel​

Before you can deploy on Vercel, you need to create a new OAuth app on
GitHub with the right callback URL for the live deployment. Head back to
Github and create a new app.

Vercel uses the deployment name in the application URL. This means that
your callback URL will look something like this:
`YOUR-DEPLOYMENT-NAME.vercel.app/api/auth`. Replace
`YOUR-DEPLOYMENT-NAME` with the name of your future Vercel deployment.

Create a new repository on GitHub and push your demo app code to the
repo.

With your code on GitHub, you are ready to deploy. Login to your Vercel
account and create a new deployment. Connect your newly created
repository using the Vercel interface and click continue. Make sure to
match your Vercel deployment name to `YOUR-DEPLOYMENT-NAME` used in your
GitHub app.

In the next step, add your environment variables.

Your app requires four environment variables (`DATABASE_URL`,
`GITHUB_ID`, `GITHUB_SECRET`, `NEXTAUTH_URL`). You can get the
`DATABASE_URL` from your `.env` file and the other three from the Github
app you just created. In addition, your live deployment requires a
`SECRET`. You can set this to any value you like, just make sure it is
secure!

With all environment variables set, hit deploy. Visit your live
deployment by clicking the link provided by Vercel.

<div>

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)note

</div>

<div>

If you visit your page through a Chrome browser you might run into
security warnings. You can solve these by adding a live deployment URL
and SSL certificate to your page. Make sure to update your callback URL
in your Github app if you do.

Alternatively, you can visit your page in another browser- such as
Safari- with less strict security enforcement, or ignore the Chrome
warnings.

</div>

</div>

## Measuring the performance​

The goal of this tutorial is to improve the data component of JAMstack
websites by using *MGSQL*. But how does it stack up? Let\'s compare page
loading speed for the original guide using Supabase and *MGSQL*.

For this test, we deployed the same demo app using a Supabase Postgres
database hosted on AWS in `us-west-1`.

Measuring the site speed directly would give *MGSQL* an unfair advantage
as it also benefits from using edge functions.

We ran the same Sysbench workload that selects users based on the
username. For both databases (Supabase and Seaplane), we ran the query
1000 times from three different locations in the world: Amsterdam
(Europe), Bahrain (Asia) and Sacramento (North America).

<div>

<div>

    SELECT * FROM users username @@ 'john';

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

</div>

<div>

<table><thead><tr><th>Region</th><th>Supabase</th><th>Seaplane</th></tr></thead><tbody><tr><td>North America</td><td>17.08</td><td>15.52</td></tr><tr><td>Europe</td><td>142.91</td><td>8.24</td></tr><tr><td>Asia</td><td>257.03</td><td>22.22</td></tr></tbody></table>

</div>

As expected, the results for North America are similar, but once we go
beyond the North American continent, we start to see a big difference.
*MGSQL* automatically placed read replicas where we requested our data.
Because our demo app uses edge functions, it will benefit from similar
latency results.

To be completely fair to Supabase, you can achieve similar results by
manually replicating your data to different regions. However, the neat
part about *MGSQL* is that you never have to think about it. If new
users in new regions show up, your data is automatically replicated as
needed.

In addition, you\'ll get all the other benefits of global tables which
includes high fault tolerance and automated disaster recovery.

</div>

<div>

<div>

**Tags:**

-   sql
-   data
-   jamstack
-   vercel
-   prisma

</div>

</div>

<div>

<div>

</div>

<div>

Last updated on **Mar 30, 2023** by **fokkedekker**

</div>

</div>

</div>

</div>

</div>

</div>
