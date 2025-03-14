---
layout: default
---
# Valhalla Survey & Stratagey

## By The Numbers
Valhalla is a big server. Created in 2017, it contains 22 text channels regularly used for chatting (not including announcement and rule channels) as of 06/23. 
The largest channel, `#berk`, contains ~520,000 messages (06/23). Most other channels are considerably smaller, containing about ~50,000 each (06/23). Note that these numbers include all messages, including those containing only media.

A large number of the messages in Valhalla contain media uploaded to the Discord CDN. According to search, there are ~10,000 messages in `#random_gifs_photos` and ~12,000 messages in `#berk` that contain at least one image (06/23). 

Assuming that each media message is 2MB in size, which is 1/2 the upload limit for free users, storing these media messages would require 44GB.

## Possible Scenarios 

### Hard Cut
> *The petals must fall*

This is the worst-case scenario, in which Valhalla is banned, destroyed, or Discord takes significant adverse action against us in particular. If this were to occur, we would need to do a few things to migrate.

1. Grab what we can - Screenshots, copy paste, whatever. Grab all the significant moments that we can, while we can.
2. Inform the users - If Valhalla is dead, we will need a way to tell the survivors where to go. There isn't really a great way to inform everyone at the moment. That's a problem.
3. Build anew - Building a new platform will take time. There will need to be a temporary platform, at least for the techies, to communicate and build the new Berk. This would probably be something simple, like IRC.

### <=3 Day Migration
> *So what are you going to do about it?*

In a time limited situation, it will likely be necessary for us to download content and store it with metadata in an intermediary location before standing up the replacement. A central SQL database hosted somewhere would likely be our best bet. 

Here's an example schema for an intermediary PostgreSQL database:
```sql
CREATE TABLE valhalla (
    messageid varchar PRIMARY KEY, # The discord message ID, which should be unique
    msgdate timestamp,
    content text,
    channel varchar,
    sender varchar, # The string name of the sender
    senderid varchar # The UID of the sender
);
```

In this scenario, it is unlikely that media content or all message content will be preserved. It also may be a good idea to limit archival initially to a certain duration, such as the last 4 years, or omit some channels entirely. In an attempt to access channel priorities, I've created channel burndown list below with notes.

Step by step:
1. Notify users and collect emails
2. Build intermediary database
3. Freeze all channels, minus one or so
4. Dump content according to burndown list until server closure
5. Build New Berk
6. Add content and users

### 1 Week+ Migration
> *Not my snappiest comeback*

This is a more likely, and simplier scenario that assumes about a week timeline to perform the migration. This scenario attempts to migrate while using the current Valhalla for coordination and communication.

In this situation, we could likely stand up the new solution, and migrate messages directly to New Berk as they are archived. This would eliminate the need for an intermediary database like in the 3 day migration.

Step by step:
1. Notify users and collect emails
2. Stand up New Berk
3. Dump content according to burndown into New Berk
4. Freeze archived channels 
5. Move users

## Channel Burndown

In the case of a restricted-time migration, it might be necessary to omit some channels from archival. This page contains a table that attempt to prioritize channels in order of importance with some notes. 

This table is only my oppinion, and may change. 

1. Berk - The MAIN channel. If we had to take only one, it would be this one. It is the largest by far.
2. httyd-general - Our main fandom channel. Lots of important lore in here
3. fandom-meetup-plans - Small channel, but lots of important meetup messages and photos.
4. dragons_edge - Secondary chat channel. Important, but less so. Smaller.
5. media_spoilers - Contains httyd lore for movie releases and things
6. nerds - Lots of good techie discussions. May be useful for reference
7. art-and-stuff - Very nice to have **if media can be saved**. Not much use without media. Lots of original art. 
8. non-httyd-general - Other fandoms lore chat 
9. book_spoilers - New and small channel, has some fandom discussion 
10. ebay - Good discussion and links to merch, **more useful with media**.
11. nsfw - Lots of funny discussion in here, but not super important
12. games - Some game discussion, meh
13. language-talks - Small discussion channel
14. tumblrs-and-social-media - Small channel, mostly links
15. music - Some old music discussion, mostly links nowadays
16. random-gifs-photos - Mostly **media**, lots of memes and such
17. random_videos - Mostly YouTube links
18. voice-chat-live - Messages, but mostly nonsense from VCs that we don't understand or remember anymore
19. fandom-dates - Birthday messages
20. emotional_support - This is complicated. A lot of users may not want this moved, and I don't blame them. As a result, not high priority 
21. tales-of-xadia - Dead, small channel
22. nsfw-spoilers - Dead channel
23. Hidden and mod channels - No idea what is in these
24. Archived channels - Do we even look at these now? Some lore importance

