---
layout: default
---
# Costs & Platforms

This doc assumes that we migrate to Matrix Synapse, like with the Fandoms experiment. We could go elsewhere. Alternatives include RocketChat, and other XMPP based platforms. 

## Saas/PaaS
Alpabet soup aside, maybe we don't want to host our own instace after all. 
ETKE would likely be our first choice for a managed instance, and they offer a fully managed solution on their hardware, or installation and maintenance on our hardware. 

Installation on their hardware with Hetzner costs $20USD a month, and includes backups, 2 vCPU cores, 4GB RAM, 80GB SSD, and 2TB traffic recommended for rooms below 3000 users. This isn't enough storage, and would likely require the addition of something like Backblaze B2 for additional cost (10/25). Things like Matrix RTC, configuring S3, and email setup cost extra per month! Ouch!

Installation on our hardware or VPS costs $5 a month, plus our VPS costs and whatever add-ons we pick (like RTC above) (10/25).

## Doing it Ourself
Like with the Fandoms experiment, we can deploy our own instance using Ansible. For this, we would primarily use a VPS.

Most providers use 1 CPU core and 2GB of memory for their base instances of Synapse, so I've assumed those values for cost.

### Providers

|         | OVH         | DigitalOcean | Hetzner |
|---------|-------------|--------------|---------|
| Arch    | x64         | x64          | x64   |
| CPU     | 4           | 1            | 2       |
| RAM     | 8G          | 2G           | 4G      |
| Data    | 400Mpbs ALL | 2kGB trans   | 20TB    |
| Storage | 75G         | 50G          | 40G     |
| Price   | $4.20       | $12          | €3.49   |
| Term    | 1yr         | None         | None    |
 
## Storage
Media must be stored outside of the VPS no matter if we get our own, or have ETKE manage it. Per our storage estimate, it would be unreasonable to store all media on the VPS. So, we'll likely need to use Backblaze B2. 

Thankfully, the cost appears to be less than a dollar per month for about 100GB. Dang, good storage sure is cheap. Download costs and API costs are unknown at this time, but probably low.

Other storage costs:

|                    | OVH   | BackBlaze |
|--------------------|-------|-----------|
| Cost per GB/mon    | 0.008 | 0.00075   |
| API costs?         | No    | Yes       |
| Download cost/GB   | 0.011 | 0.01      |
| Own download free? | Yes   | NA        |
| Storage type       | S3    | S3/B2     |

B2 pricing is confusing, but probably cheaper.