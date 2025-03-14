---
layout: default
---

# Migrating Notes & Tools
When migrating the server, three things need to be moved. The users, the text, and the media. Below are some notes and tools regarding each of those things.

## Moving Users
The simplest way to grab the most users would be to use the existing server as a migration tool, if it exists. Here's an example:

1. Mention `@everyone` with a message that the server is moving. Include a Google Form (or comparable) to collect email addresses and usernames
2. Email status updates to these addresses using SendGrid or comparable 
3. Import these emails and usernames into New Berk, and email out passwords

This will probably get the majority of active users. The migration team will not be able to reach everyone, so members telling others on other platforms will be necessary. 


## Moving Text Content
Scraping content from Discord is [explicitly against the ToS](https://discord.com/developers/docs/policies-and-agreements/developer-policy#handle-data-with-care). While Discord has taken action to make unprivileged scraping more difficult, it should still be possible for a bot to be added to a server by a moderator with the message content intent for scraping. 

It is very likely that Discord takes automated action against API useage that appears to be scraping, so we must be careful in case it ever becomes necessary. 

The API endpoint for getting messages `/channels/{channel.id}/messages` can pull a maximum of 100 messages per channel at a time. This endpoint has dynamic ratelimits applied.
I attempted testing to see about what the ratelimits are, but didn't have much luck. There don't appear to be many Discord API libraries that let you see under the hood easily to determine the limits sent back by the API. 

However, projects like [Discord Chat Exporter](https://github.com/Tyrrrz/DiscordChatExporter) do exist, and there don't seem to be any complaints about bans or slowness due to ratelimits. ArchiveTeam also [maintains a wiki page](https://wiki.archiveteam.org/index.php/Discord) with tools and tips for archiving Discord.

## Moving Media Content
This is going to be the fun one. There is not published documentation on downloading content from the Discord CDN automatically, because you really aren't supposed to. Information on how it works is limited, but blog posts and other information points to the CDN being hosted by CloudFlare. ArchiveTeam has no notes on this subject, and most archivers available focus on text content. 

CloudFlare itself is fairly well known for not being ok with scraping or automated requests, so this will likely be difficult, and take an extended amount of time. 
Migrating media content will almost certainly have to be limited to a specific duration in almost all scenarios.