---
title: Where to host static websites today
date: 2025-02-11
---

I did a short investigation of where it is easiest and cheapest to host a static
website today. I also need easy email access.

## Simply.com

- Tech: httpd, php, asp.net
- Price: $10/year first, then $110.
- Email: yes

I know this from before. Have to use either apache httpd and php or asp.net.
Comes with a mysql database and usually roundcube email. Price currently at $10
for the first year and then $110 for the following. Expensive! But you do get a
full setup and they monitor your website for unusual activity etc. But for a
simple static website? I think this is too much.

## AWS

- Tech: static files only
- Price: $6-18/year
- Email: +$48/year

AWS has also made this really easy with AWS Amplify. It's _only_ static websites
and no databases, emails etc, but it has nice git integration. Price is at
$6-18/year. Then add an email, and it's +$48/year. So total $60-80/year. Better.
But AWS is american. I want to keep the data in the EU.

## OVH webhotel

- Tech: apache httpd, php
- Price: $19/year
- Email: yes

They do both webhotels (like simply.com) and cloud. Their webhotel is only
$14/year? If so, that's definitely the cheapest. I don't think their cloud can
compare in cost - cloud is for quickly scalable things, this isn't supposed to
scale. It's actually $19/year. Setting up email and ssl is a little annoying,
but easier once I realized that I needed to add the dns of my domain to it.

## Github pages

- Tech: static files only
- Price: free
- Email: no

Github pages are free, if the repository is public, which is fine here.
