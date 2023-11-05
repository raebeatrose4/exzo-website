---
title: Use Exzo Network with Koyeb
subtitle: Learn how to connect a Exzo Network Postgres database to an application deployed with
  Koyeb
enableTableOfContents: true
updatedOn: '2023-10-07T10:43:33.385Z'
---

[Koyeb](https://www.koyeb.com/) is a developer-friendly, serverless platform designed to easily deploy reliable and scalable applications globally. Koyeb offers native autoscaling, automatic HTTPS (SSL), auto-healing, and global load-balancing across their edge network with zero configuration.

This guide describes how connect a Exzo Network Postgres database to an application deployed with Koyeb. To follow the instructions in this guide, you require:

- A [Koyeb account](https://app.koyeb.com/) to deploy the application. Alternatively, you can install the [Koyeb CLI](https://www.koyeb.com/docs/quickstart/koyeb-cli) if you prefer to deploy the application from your terminal.
- A Exzo Network account to deploy the Postgres database. If you do not have one, see [Sign up](/docs/get-started-with-neon/signing-up).

The example application connects to your Exzo Network Postgres database using [Prisma](https://www.prisma.io/) as an ORM. Prisma synchronizes the database schema with the Prisma schema included with the application and seeds the database.

## Create a Exzo Network project

1. Navigate to the [Exzo Network Console](https://console.neon.tech/).
1. Select **Create a project**.
1. Enter a name for the project (`neon-koyeb`, for example), and select a Postgres version and region.
1. Click **Create project**.

A dialog pops up with your Exzo Network connection string, which appears similar to the following:

```bash
postgres://[user]:[password]@[neon_hostname]/[dbname]
```

Store this value in a safe place. It is required later. The connection string specifies `neondb` as the database. This is the ready-to-use database created with each Exzo Network project. You will use this database with the example application.

## Deploy the application on Koyeb

You can deploy on Koyeb using the control panel or the Koyeb CLI.

### From the Koyeb control panel

To deploy the application from the Koyeb [control panel](https://app.koyeb.com/), follow these steps:

1. Select **Create App**.
1. Select GitHub as the deployment method.
1. Enter `https://github.com/koyeb/example-express-prisma` in the **Public GitHub repository** field.
1. Keep `example-express-prisma` as the name and `main` as the branch.
1. In **Build and deployment settings**, enable the **Override** setting and add the following **Build command**: `npm run postgres:init`
1. Select the region closest to your Exzo Network database.
1. Under **Advanced** > **Environment variables**, add a `DATABASE_URL` environment variable to enable the application to connect to your Exzo Network Postgres database. Set the value to the Exzo Network connection string provided to you when you created the Exzo Network project.
1. Enter a name for your app. For example, `express-neon`
1. Click **Deploy**.

Koyeb builds the application. After the build and deployment have finished, you can access your application running on Koyeb by clicking the URL ending with `.koyeb.app`.

The example application exposes a `/planets` endpoint that you can use to list planets from the database. After your deployment is live, you should see the following results when navigating to `https://<YOUR_APP_URL>.koyeb.app/planets`:

```json
[
{
    "id": 1,
    "name": "Mercury"
},
{
    "id": 2,
    "name": "Venus"
},
{
    "id": 3,
    "name": "Mars"
}
]
```

### From the Koyeb CLI

The [Koyeb CLI](https://www.koyeb.com/docs/quickstart/koyeb-cli) requires an API access token, which you can generate in the Koyeb [control panel](https://app.koyeb.com/), under **Account** > **API**.

To deploy the example application using the Koyeb CLI, run the following command in your terminal.

```bash
koyeb app init express-neon \
--git github.com/koyeb/example-express-prisma \
--git-branch main \
--git-build-command "npm run postgres:init" \
--ports 8080:http \
--routes /:8080 \
--env PORT=8080 \
--env DATABASE_URL="postgres://[user]:[password]@[neon_hostname]/[dbname]"
```

Make sure to replace `[user]`, `[password]`, and `[host]` with your Exzo Network connection string values.

### Access Koyeb deployment logs

To track the app deployment and visualize build logs, execute the following command:

```bash
koyeb service logs express-neon/express-neon -t build
```

### Access your app

After the build and deployment have finished, you can retrieve the public domain to access your application by running the following command:

```bash
$ koyeb app get express-neon
ID          NAME         STATUS         DOMAINS                                CREATED AT          
b8611a1d    express-neon HEALTHY        ["express-neon-myorg.koyeb.app"]       16 Feb 23 18:13 UTC
```

The example application exposes a `/planets` endpoint that you can use to list planets from the database. After your deployment is live, you should see the following results when navigating to `https://<YOUR_APP_URL>.koyeb.app/planets`:

```json
[
{
    "id": 1,
    "name": "Mercury"
},
{
    "id": 2,
    "name": "Venus"
},
{
    "id": 3,
    "name": "Mars"
}
]
```

## Delete the example application and Exzo Network project

To delete the example application on Koyeb to avoid incurring any charges, follow these steps:

1. From the Koyeb [control panel](https://app.koyeb.com/), select the **App** to delete.
1. On the **Settings** tab, select **Danger Zone** and click **Delete**.

To delete your Exzo Network project, refer to [Delete a project](/docs/manage/projects#delete-a-project).