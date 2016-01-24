---
title: Pushing a Node.js/MongoDB app to Pivotal's Cloud Foundry
layout: post
date: 2013-10-06
---

I spent an exciting two days at PlatformCF earlier this week and
immediately wanted to play with it.  I had a prototype of a
Node.js/MongoDB app deployed on AWS, so I thought deploying the existing
code would be a better indicator of adoption difficulties than pushing
up a simple hello world app.  Cloud Foundry is open source and you can
use it to deploy your own private PaaS but I'm going to use Pivotal CF,
Pivotal's paid hosting service, so I don't have to deal with my own
infrastructure. See
[http://www.cloudfoundry.com/hosted-pricing](http://www.cloudfoundry.com/hosted-pricing)
for more details.

The whole process can take just a few minutes.  In fact, if you're not
trying to put your services(MongoDB in this case) in the cloud, it'll
take you less.  Unfortunately, some of the documentation and tools
regarding services haven't been updated for CF v2 so I had to do a lot
of digging in order to figure out how to get them to work.  Hopefully
this document will spare you that digging and allow you to deploy your
own Node.js/MongoDB app to Cloud Foundry in a matter of minutes!

## Prerequisites

Go to
[http://docs.cloudfoundry.com/docs/dotcom/getting-started.html](http://docs.cloudfoundry.com/docs/dotcom/getting-started.html)
and deal with the first two steps: getting an account and setting up the
Cloud Foundry CLI.

## Adjusting your config (package.json and manifest.yml)

When it comes to setting up your app, you can follow the link and follow
the steps at
[http://docs.cloudfoundry.com/docs/using/deploying-apps/javascript/](http://docs.cloudfoundry.com/docs/using/deploying-apps/javascript/).
This boils down to adding:

```javascript
"engines": {   "node": "your node version" }
```

to your `package.json` and adding a `manifest.yml` file in your root
directory (although even this is optional).

## Create, bind, and configure a service

So far so good.  Now comes configuring MongoDB as a service, which is
where I ran into some roadblocks.  The docs are fine when it comes to
creating and binding a service, but I've duplicated here just for your
sake:

1. Run `cf create-service` on the command line and follow the prompts to create a mongolab service.  Remember the name of the service you create.

2. Bind the service to your app:

`cf bind-service --app <your app name>  --service <your service name>`
