# BYOIP requirements: is your prefix a /24 or a /23, which ASN do you authorize, and how long will each provider take?

BYOIP (bring your own IP) is an authorization arrangement. Your prefix stays registered to your organization, and the provider announces it from its own ASN so the rest of the internet routes traffic into that provider's network. Whether you can do it comes down to three facts about the prefix you already hold: how long it is, what state the RIR record is in, and whether you can publish a ROA naming the provider's ASN.

The published timelines differ by an order of magnitude. Oracle allows up to 10 business days for import validation after you click Finish Import. Infobip needs at least two weeks from the day it receives your documents. Alibaba Cloud estimates about a month, and says withdrawing a range takes about as long. Cloudflare's launch post described prefixes going live worldwide in minutes once they were ready to announce.

> **Check these three things before you talk to any provider.** In your RIR portal, confirm the prefix length, the exact allocation status string on the record, and that you can edit the route object and create a ROA for someone else's ASN. If any of those fails, no amount of provider shopping will fix it.

Some people searching this term hold their own ASN already and want a machine that will peer with them, so the prefix is announced from their own hardware. That is a BGP session rather than BYOIP, and it skips the RIR validation and the ROA for a third-party ASN entirely. SixtyNet's VPS hosting page lists free BGP session setup and limited BGP technical support among its product features; 👉 [look at the Los Angeles plans and the BGP session line item](https://sixtynet.net/aff.php?aff=39) before you assume this needs a hyperscaler.

## What Oracle, Cloudflare, Infobip and Alibaba Cloud each require

| Provider | Protocol and prefix size | ASN you authorize | Published lead time | Cost published? |
| --- | --- | --- | --- | --- |
| Oracle Cloud Infrastructure | IPv4 CIDR and IPv6 prefix | 31898 (14544 in Serbia Central, Jovanovac) | Token update up to 1 day; validation up to 10 business days | No |
| Cloudflare | Prefixes for Layer 7, Spectrum and Magic Transit | No ASN published; LOA plus matching IRR records, RPKI supported | Minutes once ready (2020 launch post) | No |
| Infobip | IPv4 only, /24 minimum | 43009 | At least 2 weeks after documents | No |
| Alibaba Cloud | IPv4 only, /23 or shorter | 45102, own ASN not allowed | About 1 month; withdrawal similar | Yes, feature free, bandwidth billed |

The details behind that table matter more than the table.

**Oracle** accepts only certain allocation statuses. At ARIN it wants Direct Allocation or Direct Assignment. At RIPE NCC the record has to read ALLOCATED PA, LEGACY, ASSIGNED PI or ALLOCATED-BY-RIR. At APNIC it wants ALLOCATED PORTABLE or ASSIGNED PORTABLE. The range also needs a clean history, and Oracle reserves the right to refuse space tied to abuse. IPv6 prefixes are supported but do not go through IP pool management; you assign the whole prefix or a subdivision straight to a VCN.

**Cloudflare** works from documents rather than an ASN you publish. You provide a Letter of Authorization describing the prefix and how it should be announced, and your IRR records have to agree with it. The LOA is shared with Tier 1 transit providers ahead of provisioning so they can see Cloudflare has permission to announce. RPKI is supported if you ask. Regional announcement restrictions are available if legal or data-sovereignty rules limit where your prefix can appear. One thing worth knowing before you shop: Magic Transit requires BYOIP, because the service works by attracting traffic to Cloudflare's edge through your own addresses.

**Infobip** wants a minimum /24, valid RIR records including the organization record used to generate the LOA, a route object and a ROA authorizing AS43009, and a signed LOA on company letterhead. It handles IPv4 only.

**Alibaba Cloud** takes a different route: you log into your RIR portal and set the `origin` attribute of the route object to AS45102, then open a ticket with the range, the region where you'll use it and confirmation that the ASN change is done. A RADB lookup lets you verify the change before you submit. The current help documentation, updated 2026-09-22, lists support in US Silicon Valley, US Virginia, China Hong Kong, Singapore, Malaysia Kuala Lumpur, Japan Tokyo, Indonesia Jakarta, Germany Frankfurt and UK London. Mainland China is not on that list, and the document states plainly that BYOIP is not supported there.

## The /24 wall, and who it stops

Prefix length is the fastest disqualifier in the whole process. A /24 is 256 addresses. That clears Infobip's minimum and fails Alibaba Cloud's, which requires /23 or shorter and states that longer prefixes such as /24 are not supported. Teams with a single /24 and a mail reputation worth protecting can move to Infobip. The same team cannot use Alibaba Cloud's BYOIP at all, no matter the budget.

## If you'd rather announce the prefix yourself

SixtyNet does not publish a BYOIP program. There is no provider ASN to authorize, no minimum prefix, no stated lead time, and no RIR validation described on its site. What the VPS hosting page does list is free BGP session setup and limited BGP technical support, which is the other way to get your own addresses onto the internet: you keep your ASN, you peer, and the provider is not the origin.

The network is built for China-facing traffic, with China Unicom Premium AS9929, a 100M basic network and 3x burst billed at the 95th percentile. Quarterly and annual payment carry discounts. If the prefix is under attack, the LAX DDoS Protected line is the one to look at; the residential line is a different product for a different job.

| Plan | vCPU / RAM | NVMe | Network and traffic | Annual price | Order |
| --- | --- | --- | --- | --- | --- |
| LAX-STARTER | 2 core / 2 GB | 40 GB | 2G premium, 5 TB | $200 (list $240, 17% off) | [Order](https://sixtynet.net/aff.php?aff=39) |
| LAX-MICRO | 4 core / 4 GB | 80 GB | 4G premium, 10 TB | $400 (list $480, 17% off) | [Order](https://sixtynet.net/aff.php?aff=39) |
| LAX-MEDIUM | 8 core / 8 GB | 160 GB | 10G premium, 20 TB | $800 (list $960, 17% off) | [Order](https://sixtynet.net/aff.php?aff=39) |
| LAX-LARGE | 8 core / 16 GB | 320 GB | 10G premium, 40 TB | $1,600 (list $1,920, 17% off) | [Order](https://sixtynet.net/aff.php?aff=39) |

The residential line runs on the same hardware but ships residential IPs, and it has a restriction that matters here: ports 80 and 443 are closed on those plans for IP risk-control reasons, so they are not a place to serve public web traffic from.

| Residential size | vCPU / RAM | NVMe | Network and traffic |
| --- | --- | --- | --- |
| STARTER | 2 core / 2 GB | 40 GB | 2G premium, 5 TB |
| 4-core | 4 core / 4 GB | 80 GB | 4G premium, 10 TB |
| 8-core | 8 core / 8 GB | 160 GB | 10G premium, 20 TB |
| 16-core | 16 core / 16 GB | 320 GB | 10G premium, 40 TB |

The only residential price published on the page is STARTER at $260 per year against a $300 list price; the larger sizes are quoted at checkout. Two further residential variants add AS9929 China optimization and reject all external probes and scans by default, with access limited to whitelisted or port-forwarded devices. Refunds on those two are issued as site balance rather than to the original payment method. 👉 [Take the LAX-STARTER at the $200 annual rate](https://sixtynet.net/aff.php?aff=39) if a DDoS-protected box with a BGP session is all you actually need.

## Five things that stall an import

**1. The verification token goes on the wrong record.** Oracle's documentation warns about this directly: the token must be attached to the address range information, not to the information for the organization that owns the range. At ARIN it goes in Public Comments for the range, at RIPE NCC it becomes a new `descr` field on the range, and at APNIC it goes into the `remarks` field by email to helpdesk@apnic.net, sent from the APNIC authorized contact account for that range. A colleague's mailbox will not do.

**2. No ROA means no announcement.** Oracle states that without a ROA it cannot advertise the BYOIP CIDR block or IPv6 prefix, which makes the whole import pointless. The expiry date should be at least six months out. This is not a one-time checkbox: Alibaba Cloud puts responsibility for keeping the ROA valid on the customer and notes that carriers performing RPKI Route Origin Validation will drop the route once it becomes invalid, which takes your services offline. Alibaba also reserves the right to stop advertising.

**3. Clicking Finish Import too early.** Oracle adds up to a day for the token registration to take effect, and warns that moving to the next step before that adds a day to the total. Wait until both the ROA and the token registration have propagated.

**4. Forgetting reverse DNS, and leaving the old route object in place.** Every sending IP in an email setup needs a PTR record pointing at a hostname in your own domain; Infobip notes that receiving mail servers may reject mail without it, and verifies the setup before your first send. After the cutover, Infobip asks you to delete any route object for the prefix that still authorizes a different provider, so the two authorizations don't overlap.

**5. Scheduling the cutover as if it were seamless.** Infobip says moving traffic from your current provider without interruption is not supported and tells you to plan a maintenance window. If a maintenance window is off the table, that constraint changes your whole project plan.

Running this yourself is mostly RIR paperwork plus waiting. 👉 [Start from SixtyNet's VPS hosting page](https://sixtynet.net/aff.php?aff=39) if you'd rather keep the announcement on your own ASN and skip the third-party validation step.

## Who shouldn't use BYOIP

Only a /24 in hand rules out Alibaba Cloud. An IPv6-only estate rules out both Infobip and Alibaba Cloud, which are IPv4 only; Oracle is the one in this group that documents IPv6 prefix imports. Anyone who cannot accept downtime should avoid Infobip's email BYOIP, given the explicit maintenance window. And if the requirement is to keep announcing under your own ASN, BYOIP is the wrong product at Alibaba Cloud, which states you cannot use your own ASN, and at Oracle, which ties announcement to 31898 or, in Serbia Central, 14544.

## What it actually costs

Alibaba Cloud is the only one of the four with a published billing model. The BYOIP feature itself is free. The EIP configuration fee, which is the public IP holding fee, is waived because you still own the address range. Bandwidth is charged at the peak you specify and for the billing duration, and EIPs from your own range can only use pay-as-you-go billing by fixed bandwidth. Those EIPs can only be added to an Internet Shared Bandwidth instance billed pay-as-you-go by bandwidth or by enhanced 95th percentile. "Free" here means no fee for the feature, not a free service.

Oracle, Cloudflare and Infobip do not publish BYOIP pricing in the documentation reviewed for this article, so treat any number you hear as a quote rather than a list price. The real cost is bandwidth plus engineering time: up to 10 business days at Oracle, around a month at Alibaba Cloud with a similar withdrawal period, at least two weeks plus a maintenance window at Infobip, and ongoing ROA and IRR upkeep wherever you go.

## FAQ

**Can I keep my own ASN with BYOIP?** Not at Alibaba Cloud, which says you cannot use your own ASN. BYOIP generally means the provider announces under its ASN, which is why Oracle publishes 31898 and Infobip publishes AS43009. If keeping your ASN is the requirement, you want a BGP session with a hosting provider instead.

**Does BYOIP support IPv6?** Oracle does, and documents how prefixes are assigned without IP pools. Alibaba Cloud and Infobip state IPv4 only. Cloudflare's launch post covers prefixes without limiting the version.

**What is a ROA and what happens if it lapses?** A Route Origin Authorization is an RPKI object in your RIR that authorizes a specific ASN to originate your prefix. Oracle cannot advertise without one, and networks doing RPKI Route Origin Validation will drop the route once it becomes invalid, so an expired ROA turns into an outage.

**Why does email need PTR records?** Receiving mail servers use reverse DNS to check that the sending IP belongs to the domain it claims. Infobip requires a PTR per sending IP pointing at a hostname in your domain, and verifies it before the first send.

**How long does the whole thing take, start to finish?** Budget 10 business days at Oracle after the import request, at least two weeks at Infobip from document receipt, and about a month at Alibaba Cloud. Add your own time for the RIR record change, the ROA and the LOA.

## How to decide

Match the constraint that binds you first. One /24 and a sender reputation to protect, with a maintenance window available: Infobip. /23 or shorter, users in Asia or the US, and you want a published cost model: Alibaba Cloud, outside mainland China. IPv6 prefix in the mix: Oracle, and plan for the 10 business days. Magic Transit on the roadmap: Cloudflare, since BYOIP is a prerequisite.

Two things to confirm before you pay anyone. The exact prefix length, because /24 versus /23 changes which providers exist for you. And whether you can edit the `origin` on your route object and publish a ROA for another ASN, because without that the paperwork stops at step one. 👉 [Check SixtyNet's plans and current annual pricing](https://sixtynet.net/aff.php?aff=39) if your answer to both is yes and you'd rather announce the prefix from your own box.
