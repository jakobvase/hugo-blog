---
title: Storage
date: 2023-03-01
---

## tl;dr

I chose [MEGA](https://mega.io/) for my current storage needs, but I'm not
completely happy with it. [Ente](https://ente.io/) is really strong too.

## Requirements

I've looked into storage solutions. I want our family photos and videos to be
available to us as long as anyone wants them to be there. I want my wife's
photos and texts to be stored somewhere quite safe. I want it to be easy to
share our favourite photos and videos, when we need to, and I want it to be easy
for others to upload photos and videos of us to us. I don't want to manage my
own servers, as that comes with a lot of other management (what hardware, what
operating system, security, how much raid-storage, multiple places, static dns,
etc).

I don't trust google, facebook, microsoft or amazon with our data. I don't want
to buy more into the apple ecosystem, because it only works with apple products
and I don't like to be forced to use those. I prefer something that is secure by
default.

So a list of requirements, though some may be bent:

- Not google, facebook, microsoft, amazon or apple.
- End-2-end encrypted.
- Must have a nice app for ios, android and macbook.
- Must be web-viewable.
- Must be easily sharable, without the friend having to create an account.
- For family photos, it would be nice if they could be sorted by date etc.
- For family photos, it would be good if they had a system in place to compress
  everything.
- If they can compress things, even better if they have a system in place to
  compare the quality of the compression and pick the best compression!
- For Maja's images, it would be good if they could store a single thumbnail for
  a whole folder, and then store the folder in cold-storage.
- Price is important.
- I want the data to be stored where data safety is important, though that is
  less important if the data is e2e-encrypted.
- I want the company not to be private, but to instead have an ownership
  structure where the goal of the company _is not to make money_, but instead to
  keep improving on the service of photo storage that they offer. This can be a
  cooperative, or a fund, or some other ownership structure. This is important
  as that is more long-term stable.
- It would be best if everything could be stored in the same place.
- Standardized options for file management is a plus (sftp, s3, etc).
- Open source is a plus.
- It's a plus if I can upload zipfiles that are uncompressed in the service.

## Options

### MEGA

https://mega.io/

A New Zealand based software created initially by Kim Dotcom, who is a bad
character, but he left in 2015 and has nothing to do with them now. They are
really cheap, e2e-encrypted, file storage. They offer nothing special for photos
(they don't even support webp). They offer sftp, s3, all the apps, and
web-access. Apparently they have some problems with their encryption practices
(https://blog.httpjames.space/what-the-fuck-is-mega-doing-a-commentary-on-their-messy-security-architecture/).
I'm no expert on that, so I can't verify it.

### ente

https://ente.io/

Photo only storage. e2e-encrypted. Open source. Really nice people. Have a lot
of great documentation about how they have built up their things. The web-app is
quite barebones. Data in the EU, company in the US. Easy sharing. A little
expensive. They're active on Discord.

I wrote and asked about their company structure. They are a private company and
don't want to change that. But I really appreciate their responsiveness and
openness. I will move my images here in time.

### family album

https://family-album.com/

Very focused on children and photos of children. Seems like a great service, but
the company behind is all money money money. Not encrypted. Japan based. Some
free offering. They send emails every month with little videos composed of what
you uploaded - I would hate that.

### Dropbox

https://www.dropbox.com/

Dropbox is the oldest file service, I think. They write that they don't sell my
data, which I believe. They're also money money money, but dropbox is the parent
company, not the kid. Not encrypted. Cheap as MEGA. I like the company, but I
don't trust in the long-term viability. And I remember it as _really annoying_
because it would synchronize things between people in the most annoying ways. My
dad uses it.
