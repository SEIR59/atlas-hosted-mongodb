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

![createaccount01](https://user-images.githubusercontent.com/48702365/172476116-396f9445-4b89-4013-8f13-68ac262244e9.png)


The MongoDB Atlas signup page

You'll be asked to verify your email address:

![createaccount03](https://user-images.githubusercontent.com/48702365/172476169-e456c042-c712-441b-b199-c641d1215000.png)


The MongoDB Atlas email verification page

![createaccount02](https://user-images.githubusercontent.com/48702365/172476247-a3d09948-53be-4948-9c91-04cdd49338bf.png)

The MongoDB Atlas email successfully verified page.

After continuing, you'll be asked to fill out some account setup questions.

![createaccount04](https://user-images.githubusercontent.com/48702365/172476314-e5575731-6a53-4a13-ac56-f16868d79032.png)


# Create a New Cluster

Once logged in, Atlas will request that you create a *cluster* - join a shared cluster, which is free.

![createcluster01](https://user-images.githubusercontent.com/48702365/172476425-40239a05-3dc8-491b-919a-28852290f842.png)


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

![Readandwritetoanydatabase](https://user-images.githubusercontent.com/48702365/172476579-65af5973-12e5-4afd-ab47-68e707fb0df4.png)


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

![returntodbpages](https://user-images.githubusercontent.com/48702365/172476690-329d3743-91f4-4452-a3ba-9fd87c15bacb.png)


The Network Access page after allowing access from anywhere. Select Databases.

# Obtain the Connection String

To obtain the connection string for your `.env` file, first click the `CONNECT` button.

![obtainconnectionstring](https://user-images.githubusercontent.com/48702365/172476745-75d6dbbf-8c4f-4c25-8a41-7fe2267d344e.png)


Select the CONNECT button on the Databases page

Select the `Connect Your Application` option:

![Showing the connection options, select Connect your application.](https://i.imgur.com/idUnQ5J.png)

Showing the connection options, select Connect your application.

Next, ensure that the **Node.js** driver and latest version is selected. Then click the `Copy` button to add the connection string to your clipboard:

![showingtocopyconnectionstring](https://user-images.githubusercontent.com/48702365/172476854-bcd075b6-9880-4a29-8eec-295eaf79b5f3.png)


Showing the connection string

## Connecting with Mongoose

Here's all you need!

```jsx
mongoose.connect(process.env.DATABASE_URL)
```