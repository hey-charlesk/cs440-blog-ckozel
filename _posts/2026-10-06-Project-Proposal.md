---
layout: post
title: "Project Proposal: Nostr Revisited"
date: 2026-10-06
paper_title: "An Empirical Analysis of the Nostr Social Network: Decentralization, Availability, and Replication Overhead"
paper_authors: "Yiluo Wei, Gareth Tyson"
paper_venue: "CoNEXT '25"
paper_url: "https://arxiv.org/abs/2402.05709"
week: 3
tags: [ architecture, traffic, decentralization ]
---

## Project Proposal

This project reproduces Wei and Tyson's "An Empirical Analysis of the Nostr Social Network: Decentralization,
Availability, and Replication Overhead" (ACM CoNEXT '25, https://arxiv.org/abs/2402.05709).

The original paper used data from 2023. It would be interesting to see if the general findings of replication
and wasted traffic continue in late 2026. Additionally NIP-65 started to be adopted during this time allowing
users to post preferred relays to contact them read/write; ideally targeting 2-4 relays. Might this have had a
notable impact on traffic duplication?

With 2026 measurements I will reproduce: the distribution of posts and users across relays (Fig. 1), relay
concentration by country and AS (Figs. 2), relay uptime (Fig. 4a), if the 625 free / 87 paid relays split maintains,
the mean replicas of posts compared to 34.6 in 2023 (Fig. 6), post availability after removing the top-X relays
and top-X ASes (Fig. 7), and the finding that 98.2% of client downloads are redundant (Section 5.4).

This result is worth reproducing because it is a 3 year update on Nostr's network livelihood. Is this
fledgling network growing and scaling or collapsing. Originally it seemed decentralized to a fault having unorganized
relays duplicate most of the network traffic. Is a preferred relay spec (NIP-65) being adopted if so does it contribute
to replication reduction?

Since the authors released no code or data, I will build the crawler + data store as a self-hosted container (with
cloud fallback). Relay availability will be probed on 15min intervals; the content crawl is a separate one-off pass.
Verify nostr.watch data matches that of protocol level relay discovery. The network has grown well past the original
paper's 17.8M posts over 6 months (the paper itself cites 100M posts by early 2024), so I will not attempt a full
crawl. Instead I will bound collection to a fixed window and sample event IDs, then query every reachable relay for
each sampled ID to determine its replication set. The key metrics (replicas per post, redundant download share) are
per-post ratios, so a sample is sufficient to reproduce them from a single vantage point.

One constraint is measurement time. The original uptime results cover three months (Oct-Dec 2023); this project has
roughly six weeks total, so the availability series will span four weeks at most and only if the prober is running
in the first week. Uptime and dead-relay figures will therefore be compared against the paper's with that shorter
window in mind.

Known risks, all of which the original authors also hit: relays rate-limit or ban bulk REQs (the paper lost 199 of
911 relays this way), so the crawler will honor NIP-11 limits with one connection per relay and backoff; paid-read
relays cannot be crawled and will be excluded as in the paper; and many relays sit behind Cloudflare or similar CDNs,
which blurs IP-to-AS and country mapping, so CDN-fronted relays will be reported separately.

I would also like to include a section comparing Nostr to Bluesky, Mastodon, and Matrix, which represent the
other main architectures for decentralized communication today. Nostr replicates every post to many independent
relays with no index, Bluesky keeps one authoritative data server per user and a central relay/AppView that
indexes everything, Mastodon federates with one home instance per user and server-to-server fan-out, and Matrix
federates with full room state replicated to every participating homeserver. Essentially putting together a
landscape map of decentralized social networks in 2026 and basics on how they compare on a protocol and server
count/location perspective.  