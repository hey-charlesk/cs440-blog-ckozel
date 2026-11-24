---
layout: post
title: "You Can't Hijack the Speed of Light"
date: 2026-11-24
paper_title: "Passive Data-Plane Telemetry to Mitigate Long-Distance BGP Hijacks"
paper_authors: "S. Sengupta, H. Kim, D. Jubas, M. Apostolaki, J. Rexford (Princeton / University of Virginia / Five Rings)"
paper_venue: "NINeS 2026"
paper_url: "https://drops.dagstuhl.de/entities/document/10.4230/OASIcs.NINeS.2026.14"
week: 9
tags: [bgp, hijack, security, rtt, programmable-switch, measurement]
---

## Key Idea

"BGP Hijacking" is an attack that involves announcing a false route to a destination inorder to pull traffic into
your network for monitoring purposes. Since today BGP is an unsecured protocol, with BGPsec (or its alternative SCION) 
do not have sufficient adoption to prevent such attacks. As such responses to false BGP announcements are heuristic and
operator-dependent. Traffic in an attack is forwarded to the real destination so its largely undetectable client side.

This paper suggests using RTT variations to detect such attacks, because pulling traffic into the attackers
network using false BGP route announcements typically adds RTT. Most defenses watch the control plane (BGP feeds, 
route monitors) for suspicious announcements, but attackers can use BGP communities to keep the bad announcement 
isolated. However since its impossible to hide a distance change because most internet traffic travels near
the speed of light, making significant rerouting detectable. 

Doing analysis on all countries and potential attacker countries rerouting traffic in to their land, 97% of
these worst case attacks are detectable. This dropped to 91% (campus trace) and 86% (M-Lab) when using real historical
RTT data collected. Isolated countries do great (New Zealand 100%), big countries with lots of neighbors do
worst (Russia 85%).

Note that this RTT data can be collected passively by monitoring traffic and not introducing any additional packets. 
So the authors also created a detection mechanism that can run on programmable switches. An attack is flagged when the 
min RTT crosses the speed of light lower bound for that hop. On detection the switch blocks or rate limits the prefix, 
while sending ICMP probes to detect when it recovers. On campus trace data the false positive rate was at most 0.012% 
with a median 0.75 s of downtime per false alarm.

## Critique
I suppose there are bad things that could happen to various unsecure protocols and there being a general ability to
gain information by monitoring internet traffic even if its encrypted. However from a user data security point-of-view
this is not threatening to data security. I see the value this adds as another vector to detect such attacks. However
these attacks are less dangerous than a compromised ISP because they could monitor traffic without a speed of light
increase. We already have minimal trust for network operators with TLS solving the data protection problem.

## Connections

Code: [github.com/Princeton-Cabernet/HiDe](https://github.com/Princeton-Cabernet/HiDe)

[QUIC Attack Missed! and Hit the Telescope](/cs440-blog-ckozel/2026/10/20/quic-attack/) also used passive measurements 
to detect attacks.

[What is 'partial reachability' on the Internet?](/cs440-blog-ckozel/2026/10/01/Partial-Reachability/) was about BGP
policy quietly breaking reachability. Here BGP policy quietly keeps reachability working while changing the path, 
which is worse since nothing fails.

[OpenFLAME](/cs440-blog-ckozel/2026/11/05/openflame-spatial-naming/) ties network identifiers to physical places
on Earth. Here HiDe has to guess them from geolocation databases, maybe a location based lookup in OpenFLAME could
strengthen this detection capabilities. 
