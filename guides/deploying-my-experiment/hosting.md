---
description: >-
  Hosting your experiment on Meteor Galaxy so that people can access it and
  participate in it.
---

# Hosting

## How do I deploy my app to Galaxy?

[Galaxy](https://www.meteor.com/cloud) (also called Meteor Cloud) is a hosting service provided by Meteor. For a full guide on deploying to Galaxy, see [here](https://galaxy-guide.meteor.com/deploy-guide.html).

### Registering and logging in

The first step is to register an account with Meteor Galaxy. **Please take note of the fees involved in hosting with Galaxy.**

Then, in your app's command line, you should log into your Meteor Galaxy account with:

```
meteor login
```

It will then prompt you for your username and password.

To see which account is currently logged into your app you can type:

```
meteor whoami
```

### Hostname

A **hostname **is the name that the public will use to access your app.&#x20;

Part of the hostname is the **domain name**. You can use a custom domain name if you own one, or use one of those offered by Galaxy.&#x20;

If you are using the offered domain, use:

* &#x20;`.meteorapp.com` for apps deployed to the US East region
* &#x20;`.eu.meteorapp.com` for apps deployed to the EU West region
* &#x20;`.au.meteorapp.com` for apps deployed to the Asia-Pacific region

For example, a complete hostname with a Galaxy offered domain could be: `my-game-study-example.meteorapp.com`

This, combined with _https://_ is the link you will send players for them to access your app.

### DEPLOY\_HOSTNAME

You will need to enter a deploy\_hostname variable depending on your location:

* To deploy to US East: `DEPLOY_HOSTNAME=galaxy.meteor.com`
* To deploy to EU West: `DEPLOY_HOSTNAME=eu-west-1.galaxy.meteor.com`
* To deploy to Asia-Pacific: `DEPLOY_HOSTNAME=ap-southeast-2.galaxy.meteor.com`

### Deploying

Once you have selected a hostname and a deploy\_hostname variable, you need to type the following commands to deploy your app to Galaxy with your [settings file](../../faq/faq.md#what-is-the-settings-json).

{% tabs %}
{% tab title="Linux, Mac, Windows with WSL" %}
```
[deploy_hostname] meteor deploy [hostname] --settings settings.json
```
{% endtab %}

{% tab title="Windows without WSL" %}
```
SET [deploy_hostname]
meteor deploy [hostname] --settings settings.json
```
{% endtab %}
{% endtabs %}

For example, you might run this:

```
DEPLOY_HOSTNAME=galaxy.meteor.com meteor deploy my-game-study-example.meteorapp.com --settings settings.json
```

Note that deploying an app **takes a few minutes!**

Once it is done, you can go to [https://galaxy.meteor.com/](https://galaxy.meteor.com) to check on your apps. You can inspect their **logs **and modify their **settings**.&#x20;

If you run the commands to deploy the app again, it will redeploy the app with any changes you have made since your last deployment.

### What should the scale of my hosting service be?

The size of the Galaxy hosting is determined by **containers** of different size. You can have multiple containers of the same size. This will determine how many players and games can co-occur on your deployed app. **Please take note of the fees involved in hosting with Galaxy.**

**You may be able to get a single Tiny container for a free plan (check your Meteor Cloud/Galaxy account).** However, the Tiny containers will be insufficient for collecting data. You will want bigger containers. The exact size depends on how many players will be using your app concurrently an how demanding your app is. The most basic way of finding out the right size for you is to test your deployed app with different containers whilst monitoring the CPU usage.&#x20;
