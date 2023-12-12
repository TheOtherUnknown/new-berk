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
Like with the Fandoms experiment, we can deploy our own instance using Ansible. For this, we would primarily use a VPS.

Most providers use 1 CPU core and 2GB of memory for their base instances of Synapse, so I've assumed those values for cost.

### Providers

|         | OVH         | DigitalOcean | Hetzner |
|---------|-------------|--------------|---------|
| Arch    | x64         | x64          | ARM64   |
| CPU     | 1           | 1            | 2       |
| RAM     | 2G          | 2G           | 4G      |
| Data    | 250Mpbs ALL | 2kGB trans   | 20TB    |
| Storage | 80G         | 50G          | 40G     |
| Price   | $6.44       | $12          | €3.79   |
| Term    | 1yr         | None         | None    |

ETKE supports installing on ARM, while the playbook alone may or may not. 
## Storage
Media must be stored outside of the VPS no matter if we get our own, or have ETKE manage it. Per our storage estimate, it would be unreasonable to store all media on the VPS. So, we'll likely need to use Backblaze B2. 

Thankfully, the cost appears to be less than a dollar per month for about 100GB. Dang, good storage sure is cheap. Download costs and API costs are unknown at this time, but probably low.

Other storage costs:

|                    | OVH   | BackBlaze |
|--------------------|-------|-----------|
| Cost per GB/mon    | 0.008 | 0.005     |
| API costs?         | No    | Yes       |
| Download cost/GB   | 0.011 | 0.01      |
| Own download free? | Yes   | NA        |
| Storage type       | S3    | S3/B2     |

# Possible Scenarios 

## Hard Cut
> *The petals must fall*

This is the worst-case scenario, in which Valhalla is banned, destroyed, or Discord takes significant adverse action against us in particular. If this were to occur, we would need to do a few things to migrate.

1. Grab what we can - Screenshots, copy paste, whatever. Grab all the significant moments that we can, while we can.
2. Inform the users - If Valhalla is dead, we will need a way to tell the survivors where to go. There isn't really a great way to inform everyone at the moment. That's a problem.
3. Build anew - Building a new platform will take time. There will need to be a temporary platform, at least for the techies, to communicate and build the new Berk. This would probably be something simple, like IRC.

## 3 Day Migration
> *So what are you going to do about it?*

In a time limited situation, it will likely be necessary for us to download content and store it with metadata in an intermediary location before standing up the replacement. A central SQL database hosted somewhere would likely be our best bet. 

Here's an example schema for an intermediary PostgreSQL database:
```
CREATE TABLE valhalla (
    messageid varchar PRIMARY KEY, # The discord message ID, which should be unique
    msgdate timestamp,
    content text,
    channel varchar,
    sender varchar, # The string name of the sender
    senderid varchar # The UID of the sender
);
```

In this scenario, it is unlikely that media content or all message content will be preserved. It also may be a good idea to limit archival initially to a certain duration, such as the last 4 years, or omit some channels entirely. In an attempt to access channel priorities, I've created this [[burndown.md|channel burndown table]] with notes. 

## 1 Week Migration
> *Not my snappiest comeback*

This is a more likely, and simplier scenario that assumes about a week timeline to perform the migration. This scenario attempts to migrate while using the current Valhalla for coordination and communication.

### Moving Text Content
Scraping content from Discord is [explicitly against the ToS](https://discord.com/developers/docs/policies-and-agreements/developer-policy#handle-data-with-care). While Discord has taken action to make unprivileged scraping more difficult, it should still be possible for a bot to be added to a server by a moderator with the message content intent for scraping. 

It is very likely that Discord takes automated action against API useage that appears to be scraping, so we must be careful in case it ever becomes necessary. 

The API endpoint for getting messages `/channels/{channel.id}/messages` can pull a maximum of 100 messages per channel at a time. This endpoint has dynamic ratelimits applied.
I attempted testing to see about what the ratelimits are, but didn't have much luck. There don't appear to be many Discord API libraries that let you see under the hood easily to determine the limits sent back by the API. 

However, projects like [Discord Chat Exporter](https://github.com/Tyrrrz/DiscordChatExporter) do exist, and there don't seem to be any complaints about bans or slowness due to ratelimits. ArchiveTeam also [maintains a wiki page](https://wiki.archiveteam.org/index.php/Discord) with tools and tips for archiving Discord.

### Moving Media Content
This is going to be the fun one. There is not published documentation on downloading content from the Discord CDN automatically, because you really aren't supposed to. Information on how it works is limited, but blog posts and other information points to the CDN being hosted by CloudFlare. ArchiveTeam has no notes on this subject, and most archivers available focus on text content. 

CloudFlare itself is fairly well known for not being ok with scraping or automated requests, so this will likely be difficult, and take an extended amount of time. 