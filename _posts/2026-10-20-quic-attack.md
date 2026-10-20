---
layout: post
title: "QUIC Attack Missed! and Hit the Telescope"
date: 2026-10-20
paper_title: "Waiting for QUIC: Passive Measurements to Understand QUIC Deployments"
paper_authors: "J. Mücke, M. Nawrocki, R. Hiesgen et al."
paper_venue: "CoNEXT 2025"
paper_url: "https://dl.acm.org/doi/10.1145/3768988"
week: 4
tags: [internet-telescope, quic, hypergiant, measurement]
---

## Key Idea

Researches in Germany have utilized UCSD internet telescope data to analyze QUIC rollout and changes across
hyperscalers. When QUIC traffic from a malicious actor utilizes a false IP source address on the UDP packet servers
return responses to IPs which occasionally belong to the UCSD telescope. Since UCSD is always collecting this 
'backscatter' researches examine years of past data examining QUIC packs that were received. Due to full IPv4 scans
and this 'backscatter' the data includes details about both clients and servers using QUIC.

QUIC is implemented ontop of UDP but runs on the application layer (L7). This poses a challenge for L4 load balancers
since TLS needs to be terminated after the LB. Moreover QUIC has the feature of swapping connections to a different
client IP making L4LBs route to a different end server. QUIC connection IDs present in unencrypted handshake packets
are used by hyperscalers to route to specific servers. This LB workaround allowed the researchers to deduce information
about the number of L7LB in use. By mapping server IPs to AS/Hyperscaler and using cleartext fields in the QUIC 
handshake such as version and features, we get a glimpse into their server setup.


The authors took one month of QUIC data from 2021 to 2025 to look at how QUIC deployment has changed over the years. 
Adoption of QUIC v1 adopted quickly after its 2021 release, with Akami and Fastly being the last to adopt support in 2023.
However the newer QUIC v2 was not seen at all in the backscatter data, which could just indicate attackers/scanners 
aren't using it yet. QUIC's packet coalescence feature is seen by everyone except Meta. The size of each QUIC
packet is consistent per connection, but specific sizes vary by software implementations indicating what software 
versions servers use and when they upgrade. Also observed are retry/backoff strategies by the server when the client
does not respond. QUIC's DoS mitigation feature forcing the client to prove its address is rarely used, likely due to
the added round trip it requires. QUIC connection IDs are used to identify off-net/CDN in others networks, since 
L7 identifying server connection IDs are the same inside/outside of the network. These passive measurements were
validated with a smaller set of active measurements. 

## Critique
I'm not certain the conclusion about QUIC DoS mitigation not being widely used is correct. Akami's 3% seen in 2025
sounds significant, and might only be used if an attack is detected not unconditionally for each connection. 
Despite QUIC being an always encrypted protocol, there is still a surprising amount of information and fingerprinting
data available, even from passive measurements.

## Connections
[Lessons learned from operating a large network telescope, SIGCOMM 2025](https://dl.acm.org/doi/10.1145/3718958.3754347)
Covers fundamentals of Internet Telescopes.

[P-ECMP](/2026/10/08/ecmp-traffic-engineering/) notes the same ECMP 5-tuple hashing got-cha making specific downstream
destinations difficult to specify.