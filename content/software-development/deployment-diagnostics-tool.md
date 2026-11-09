---
title: Deployment diagnostics tool
date: Not yet
draft: true
---

Today I had the need for a diagnostic tool for deployments. That goes through a
list of assumptions that I have about the problem I was having.

I had updated the certificates for the service a few days ago. But the actual
problem was that manually restarting the container caused it to use a too-old
`:latest`-tagged image instead of the most recent one.

So something that checks "is there a connection between this container and
that?" and "are the images the versions you think they should be?" and "are the
certificates the ones you expect, and are they trusted by the truststores they
should be?".

An idea for an open source project. Or maybe just try to find one :)
