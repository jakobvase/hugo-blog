---
title: Personal cloud
date: 2025-05-20
---

I think service providers by default should store the data you upload to your
own personal cloud. This is a thing that's now possible, and wasn't 15 years
ago.

## Arguments

I think everyone should own a "drive" on the web that applications integrate
with. This is where you store your photos, your save games, your notes, your
bookmarks, your search history, your instant messages, your calendar, contacts,
emails, passwords, your data. Potentially even your tweets/comments/posts.

If some company wants to provide you with a service, you give them access to
their little corner of your data so you can sync through them, have AI recognize
people, store your professional photos in original resolution and optimized etc.

There are more benefits. If everyone is responsible for their own data, someone
stealing data from some company would not get any personal data - there's no
LastPass to get every password in the world from.

And there's the potential for this to store the books, music, games and movies
you've bought too, so the money can go back to the artist again. You could
stream it directly from your personal cloud. And never have to worry about some
provider going bankrupt, because your data is just yours. But not initially.

Companies might also be able to use some version of this, but I'd like to start
with the private version first. For companies, things like contracts, customer
relations, accounting, etc would be relevant.

### Problems

There are problems too though. Need to write about them too. Encryption is hard.
How will services get access to the data? Can I design an interface that covers
all the things I wrote above? How about search across data? But the great thing
is that these problems could be solved by companies who want to make money off
of this. Communication is hard. How do I communicate to non-techs the importance
of this? How do I make it easy to set up? How do I show the authorization? What
apps have what kinds of access to what data? Should people also have a database
for themselves, where apps can put data they need to join etc? What about
performance, will it need to know stuff like what data center the person has
stored their data in in order to be performant? Can the services cache data so
they can provide a better experience?

Since access to your data is now spread out over many services, if one of them
is compromised, they could potentially corrupt your data in other services. That
sucks. Data access must be limited to exactly what that service needs and no
more. Potentially can't even share data between applications. And that's ok.

## Notes

Things I'd like to store there:

photos, save games, notes, bookmarks, search history, instant messages,
calendar, contacts, emails, passwords, playlists, likes (albums, books, artists,
songs) your notes about them, code repositories?, settings for applications,
certificates and ssh keys, voice memos, documents, CVs, play counts for songs,
achievements for games?, reviews of companies, webpages I'd like to keep,
medical data?

I also want to create this (if it doesn't already exist?) open source and freely
usable, so someone can start a company that makes money off of this, but
developers can just run their own if they want.

This also maybe ties in to the "online identity" I've been thinking of. Will
need to write about that too.

Should some of the services maybe also be scientific services that request
anonymized read-access to your data? Maybe later.

## Investigation

I've looked a little at what this would currently cost, and it's at about 60
USD/TB/month for google drive today incl. taxes. In aws/google cloud/ovh you
could get as low as 1 USD/TB/month if you never need to look at your data, but
around 10-20 USD/TB/month + taxes is probably more realistic. But here, you pay
GB by GB instead of everything at once, and you could potentially yourself
choose how to optimally store your digital life. Aws also has the elastic file
system, where they claim they can keep the "total cost of ownership" as low as
$31/TB/month + taxes.

A company/product like CloudBerry Labs/MSP360 is already doing some of this,
with "bring your own storage" backup software that's pretty neat. It would be
relevant to get them to integrate with this.

Looking further, it's actually fairly common for backup services to provide
"bring your own storage". Which makes sense, you want to have easy backup of
your stuff to a place that you own. But most of these are backup for business,
it's much less common to provide the service for people.

"MySpace"...

Backblaze has 6 USD/TB/month, Hetzner has about 7 EUR. Those are both with 3 TB
egress, which is where the other storage providers try to keep you. Backblaze
provides unlimited egress at 15 USD/TB/month, Hetzner doesn't even provide that.
And they're both S3 compliant. There's probably a library somewhere that wraps
file systems in an S3 interface.

## How is this actually going to work

Pseudo. I need to write a test of a service that sets up everything and
writes/reads/lists things it has access to.

```
// System requests access to user's data api
token = oauth { systemCertificate, more? }

// System uses the token to perform an action
storeImage { id, title, time, more, token }
getImage { id, token }
```

Is it more complex than that? I should probably start by just writing a system
that uses this, before I even start to create the authn/z server and the
storage. So I can determine what's required from the systems the users will use.
I should investigate what oauth/oidc requires, and how I can make everything
very simple for the ordinary user.

Arbitrary data storage first? That's what I want the most, and what is probably
the most useful. But also doesn't have some of the problems that image storage
has. Maybe check out the ente code base?

## Challenges

First roadblock. I want to use oauth 2.0 with an authorization code grant to set
up the access to the personal data. But that requires the services that request
the data to register with the data provider, and that makes it harder to host
the data provider yourself. All the services now need to also register with your
little provider. There are ways around this - if services allow users to enter
hostname, client key, client secret to their storage, that probably solves it.
But it's more work for every service.

## Terminology

- Data Authorizer - the web service I'm trying to build, that authorizes
  requests for the user's data and somehow exposes an api to it, either directly
  or by passing out links to the user's data setup.
- Integrated Service - a client facing application that integrates with the data
  authorizer to help the user achieve something with their data.
- Storage - currently an abstract idea, but probably something like an S3
  bucket, a relational database, a key-value store. I'm working to flesh out
  what this is currently.

## User journey

- Set up an account with the data authorizer.
- Set up storage somewhere (must be able to be done automatically by the data
  authorizer).
- Link the storage to the data authorizer (must be able to be done automatically
  by the data authorizer).
- Register with an integrated service, which uses oauth to authorize with the
  data provider.

### Self-hosting

More complex but you're in control.

- Run the data authorizer in docker somewhere.
- Set up an account with the data authorizer.
- Set up storage.
- Link the storage to the data authorizer.
- Register with an integrated service. Configure the integrated service to use
  your data authorizer for you. Go through the oauth flow as normal.
