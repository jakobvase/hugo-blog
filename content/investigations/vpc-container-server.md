---
title: VPC rootless container server
date: 2024-10-25
---

Today I succesfully set up a self-updating, self-rebooting, self-starting,
self-certificating web server that I wrote myself, running rootless podman.

This is a quick note on the requirements for setting that up. For all the
detours go to the repository at https://github.com/jakobvase/rust-score-tracker.

## Prerequisites

This guide is probably only relevant if you're running Debian 11 and Podman 4.3,
which is the default on Debian 11.

## Setup the vpc

- Follow the
  [guides on security from OVH](https://help.ovhcloud.com/csm/en-gb-vps-security-tips?id=kb_article_view&sysparm_article=KB0047706)
  (or some other provider you trust).
- Set up a firewall like in
  [this guide](https://docs.rockylinux.org/guides/security/firewalld-beginners/).
- Install podman `sudo apt-get update`, `sudo apt-get -y install podman`.
- Set up port-forwarding because rootless podman can't access 80 and 443:
  - `sudo firewall-cmd --zone=public --add-masquerade`
  - `sudo firewall-cmd --zone=public --add-forward-port=port=80:proto=tcp:toport=8000`
  - `sudo firewall-cmd --zone=public --add-forward-port=port=443:proto=tcp:toport=8001`
  - test and then `sudo firewall-cmd --runtime-to-permanent`
- Follow [the let's encrypt guide](https://letsencrypt.org/getting-started/) to
  install certbot. I used
  [snapd](https://snapcraft.io/docs/installing-snap-on-debian).
- Setup automatic updates and reboots by editing
  `/etc/apt/apt.conf.d/50unattended-upgrades`. See also
  [https://wiki.debian.org/UnattendedUpgrades].
- Now get your web-server running.

## Run your webserver

- Save the image you want to run on the server as a .tar file on your local
  machine: `docker save -o image.tar your-image`.
- Copy to the server: `scp image.tar user@server-ip:/home/user/image.tar`.
- Add it to the images on the server: `podman load -i image.tar`
- Run it. This will look different for your project, but some of it is the same.
  For the certificates to work, you will at least need to load the certs and
  acme challenge. The ports are the ones I set up forwarding to above.

```bash
podman run -d -p 8000:80 -p 8001:443 \
--name rust_score_tracker_server \
-v rust-score-tracker-data:/app/data \
-v /home/user/config.json:/app/config.json \
-v "/home/user/score-tracker-static/.well-known/acme-challenge:/app/acme" \
-v "/etc/letsencrypt:/app/certs" \
rust-score-tracker --config /app/config.json
```

## Automatic restart on reboot

If you run into problems, I recommend looking at the documentation for your
version of Podman. For 4.3, that's here:
https://docs.podman.io/en/v4.3/markdown/podman-generate-systemd.1.html

1. Enable user lingering with `sudo loginctl enable-linger <username>`. This
   allows that user to start services after a reboot.
2. Make sure your webserver is running.
3. Generate a systemd file with
   `podman generate systemd rust_score_tracker_server --new > score-tracker-server.service`.
4. `mv score-tracker-server.service ~/.config/systemd/user/`. May need to make
   the directories along the way.
5. `systemctl --user enable score-tracker-server.service`.
6. `systemctl --user start score-tracker-server.service`.

There you go!
