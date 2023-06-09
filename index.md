---
layout: default
---
With the large number of VC backed companies being acquired or making huge changes in attempt to reach profitability for an IPO, it is not unlikely that Discord could eventually have its day.
This site is intended to serve as a first look into what a migration from the current server might require, including:
* Costs
* Process
* Limitations
* Possible issues

Good luck out there.

# By The Numbers
Valhalla is a big server. Created in 2017, it contains 22 text channels regularly used for chatting (not including announcement and rule channels) as of 06/23. 
The largest channel, `#berk`, contains ~520,000 messages (06/23). Most other channels are considerably smaller, containing about ~50,000 each (06/23). Note that these numbers include all messages, including those containing only media.

A large number of the messages in Valhalla contain media uploaded to the Discord CDN. According to search, there are ~10,000 messages in `#random_gifs_photos` and ~12,000 messages in `#berk` that contain at least one image (06/23). 

Assuming that each media message is 2MB in size, which is 1/2 the upload limit for free users, storing these media messages would require 44GB.

# Costs
This doc assumes that we migrate to Matrix Synapse, like with the Fandoms experiment. We could go elsewhere. Alternatives include RocketChat, and other XMPP based platforms. 

## Saas/PaaS
Alpabet soup aside, maybe we don't want to host our own instace after all. 
ETKE would likely be our first choice for a managed instance, and they offer a fully managed solution on their hardware, or installation and maintenance on our hardware. 

Installation on their hardware with Hetzner costs $19USD a month, and includes backups, 1 CPU core, 2GB RAM, 20GB SSD, and 20TB traffic recommended for 100 users. This isn't enough storage, and would likely require the addition of something like Backblaze B2 for additional cost (06/23).

Installation on our hardware or VPS costs a one-time fee of $50USD, plus $10USD a month if we want maintenance and support (06/23).

## Doing it Ourself
Like with the Fandoms experiment, we can deploy our own instance using Ansible. 

# Possible Scenarios 

## Hard Cut
> *The petals must fall*

## Normal Migration
> *Not my snappiest comeback*