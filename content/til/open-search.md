---
title: Connect to local opensearch instance
date: 2023-10-16
---

When connecting to a local opensearch instance

- get the username and password (in this case `admin`/`admin`)
- write `docker ps` to get at the port (in this case `0.0.0.0:64199`, same as
  `localhost:64199`)
- if ssl is enabled (usually is), go to `https://localhost:64199`, click
  advanced and continue, and enter the username and password.
- then open `Elasticvue`, and add all the same info there.
- profit
