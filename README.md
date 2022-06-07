![https://i.imgur.com/B42NavR.jpg](https://i.imgur.com/B42NavR.jpg)

# Contents

# Intro

While developing an application that requires a MongoDB, you can only connect to your local MongoDB engine for so long. This is because the application, once deployed, will have to connect to a MongoDB engine accessible via the Internet.

Heroku, the application hosting service we deploy our projects to, is capable of supplying a MongoDB. However, delaying connecting to a hosted database until the time of deployment is not ideal.

It makes sense to set up and connect to a hosted MongoDB sooner, rather than later. Doing this will ensure that any data, users, etc., created will be there upon deployment.

Additionally, it’s advantageous to use a service to host MongoDB databases other than Heroku to databases anytime you want.

The most popular service for hosting MongoDB databases, not surprisingly, is MongoDB’s own [Atlas](https://www.mongodb.com/cloud/atlas/).

# Create an Atlas Account

First, you will need to signup for a free account [**here**](https://www.mongodb.com/cloud/atlas/register).

![The MongoDB Atlas signup page](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3d0ea3f7-ff22-4c9d-9e2e-e566bc32484e/Untitled.png)

The MongoDB Atlas signup page

You'll be asked to verify your email address:

![The MongoDB Atlas email verification page](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f3c57804-f1ee-4d76-9d27-168ed5ce329f/Untitled.png)

The MongoDB Atlas email verification page

![The MongoDB Atlas email successfully verified page.](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2cdc3bc5-8c41-4fd8-9558-29a91727cc8d/Untitled.png)

The MongoDB Atlas email successfully verified page.

After continuing, you'll be asked to fill out some account setup questions.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/87818977-f2fc-44d0-8cf8-97d6c6dbda85/Untitled.png)

# Create a New Cluster

Once logged in, Atlas will request that you create a *cluster* - join a shared cluster, which is free.

![Use a shared cluster, which is free](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/afabd2f6-bd94-4a58-858f-f50f15156992/mdb1.png)

Use a shared cluster, which is free

Atlas allows one free cluster per account.

A cluster can contain multiple MongoDB databases - which Atlas refers to as **namespaces**.

Be sure to select the **Cloud Provider & Region** nearest to your job market.

Next, in the **Cluster Tier** section, select the `M0 Sandbox` tier.

Finally, you can optionally change the name of the cluster, then click the `Create Cluster` button.

![All of the options you should select for your starter cluster.](https://i.imgur.com/BsqXcrt.png)

All of the options you should select for your starter cluster.

It may take several minutes for Atlas to build your cluster, but we can keep working while it happens!

## Add a User for the Cluster

Each cluster must have a user created whose credentials will be provided in the database connection string when connecting to a database.

First click `Database Access` under the Security section:

![Navigate to Database Access](https://i.imgur.com/ziY6s1X.png)

Navigate to Database Access

Click the `Add New Database User` button.

![Select the Add New Database User button](https://i.imgur.com/c5wt3MJ.png)

Select the Add New Database User button

Then enter a username, password (with no special characters), select the `Read and write to any database` option, then click the `Add User` button.

![The Add New Database User pane](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8fb9a228-a226-4f46-9d0f-0fe3ce22eb1f/image_(34).png)

The Add New Database User pane

## Update the Whitelisted IPs

With the database user added, we can move onto Network Access. Click that in the sidebar navigation.

![Select Network Access](https://i.imgur.com/hm4wl8L.png)

Select Network Access

Atlas has a security feature that allows the databases to be accessed by *whitelisted* (approved) IP addresses only.

However, you must whitelist **all IPs** to ease the development and deployment of your application. Production environments with greater security concerns would not use this option.

Click the `Add IP Address` button.

![Select the Add IP Address button](https://i.imgur.com/pG2PGw9.png)

Select the Add IP Address button

In the dialog, first click `ALLOW ACCESS FROM ANYWHERE`. This will add 0.0.0.0/0 as a whitelist entry. Then click the `Confirm` button.

![The result of clicking the ALLOW ACCESS FROM ANYWHERE button](https://i.imgur.com/JSAcWEL.png)

The result of clicking the ALLOW ACCESS FROM ANYWHERE button

<aside>
🚨 This Whitelist Entry setting is inherently insecure, but necessary to ensure our deployed apps are able to connect to our database since we don't know the IP Address that our deployed app will be hosted on when we are using the Heroku free tier (and even if we did know, it would change on every spool up of the Heroku Dyno). ***Do not place private information in a database that allows access from anywhere.***

</aside>

Return to the Databases page.

![The Network Access page after allowing access from anywhere. Select Databases.](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c0e564ce-cda0-4c7a-ba86-b243c98ad61d/mdb2.png)

The Network Access page after allowing access from anywhere. Select Databases.

# Obtain the Connection String

To obtain the connection string for your `.env` file, first click the `CONNECT` button.

![Select the CONNECT button on the Databases page](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/59a1554a-2e4d-4097-944e-a603879f8752/mdb3.png)

Select the CONNECT button on the Databases page

Select the `Connect Your Application` option:

![Showing the connection options, select Connect your application.](https://i.imgur.com/idUnQ5J.png)

Showing the connection options, select Connect your application.

Next, ensure that the **Node.js** driver and latest version is selected. Then click the `Copy` button to add the connection string to your clipboard:

![Showing the connection string](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/369e7559-afe3-4a58-9ae9-89321c7e1f89/mdb4.png)

Showing the connection string

## Connecting with Mongoose

Here's all you need!

```jsx
mongoose.connect(process.env.DATABASE_URL)
```