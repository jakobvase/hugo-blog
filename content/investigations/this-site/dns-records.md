---
title: DNS records
date: 2022-06-24
---

I've booked [vase.dev], [vase.art], and [jakobvase.dk]. I use [dns.services] as
DNS hotel. Now, I want to be able to receive emails on [vase.art], and to setup
[vase.dev] to forward to [vase.art].

[dns.services] allow for primary and secondary DNS. I read up on secondary DNS
and decide that's not something I need right now. Secondary DNS seems to be for
duplicating your DNS to other providers, where they refer to the primary DNS and
update when that updates. As [dns.services] already has three servers set up,
it's not something I'll go for right now.

Setting up [vase.dev] to forward to [vase.art] is easy. Two A-records with the
names `vase.dev` and `www.vase.dev`. Their data is the IP address of [vase.art]
(I think): `195.242.131.238`.

Setting up protonmail starts out by simply adding a TXT record with my
protonmail key - I'm just following their guide. It will take some time before
it's updated, so small progress there.

I had to type @ in the hostname, but that was simply ignored by [dns.services].
I've waited for 8 hours now, and it's not working yet. I think it might be
because writing @ in the hostname caused [dns.services] to write `.vase.art` in
the hostname field, which is nonsense. @ should be the same as just the url. But
when I create a new record with an empty field, they don't do that. Hm.

Aha! When I create a new record, writing @ in the field, it _does_ write
`.vase.art`. Silly silly. Now I have both `@.vase.art` and `vase.art` set to the
protonmail string. It really should work tomorrow.

It was my mistake all along. All this time I was trying to add `jakobvase.dk` to
protonmail. Not `vase.art`. Annoying. But now it's fixed. And verification
already went through. Brilliant.

And after adding all the records (2 MX, 3 CNAME, 2 TXT), I can now receive
emails at jakob@vase.art! Success!
