---
layout: post
title: "Using Google Container Registry with Marathon"
date: 2015-11-23
external: true
link: http://spantree.net/blog/2015/11/23/marathon-with-gcr.html
---

We recently were tasked with setting up a container management solution
on Google Cloud Engine (GCE). After standing up Mesos and Marathon on
CoreOS, our initial tests worked fine and we deployed several docker
apps just fine to Marathon. We ran into a snag, however, when switching
from a public registry to a private Google Container Registry.
