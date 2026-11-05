---
layout: post
title: "OpenFLAME: Public Location Querying on DNS"
date: 2026-11-05
paper_title: "OpenFLAME: A Federated Spatial Naming Infrastructure"
paper_authors: "S. Bharadwaj, Z. Ma, I. Liang, M. Farb, A. Rowe, S. Seshan (Carnegie Mellon University)"
paper_venue: "NINeS 2026"
paper_url: "https://drops.dagstuhl.de/entities/document/10.4230/OASIcs.NINeS.2026.20"
week: 7
tags: [dns, federation, decentralization, spatial, query, maps, location]
---

## Key Idea

OpenFLAME is a good solution to a problem I did not even think existed. Decentralized location claiming, here 
in the context of maps. Today we use Google/Apple maps for basically all location and mapping related queries.
Don't need to convince me that centralized gate keepers on data as important as location and map data are bad.

The central idea is querying for map based on locations of the Earth chunked into different levels of squares, 
so it can be numerically index, with a hierarchy of different resolution levels. You want to match these queries to 
servers who claim to have maps relevant to those index chunks. If this search is facilitated by an unbiased 
decentralized service then you can have multiple independent map providers hosting content that is publicly indexable.

OpenFLAME's decentralized host is DNS. Using different levels of domain name, 'L5.L4.L3.L2.L1.Face.loc' allow looking
up servers at different places in the world via DNS recursive resolution. While the authors propose 3 new DNS record-types ,
they can be implemented today as TXT records; the new records would have some changes to the resolution needing to 
account for all locations returned due to multiple valid authorities for the same location on earth being valid.  

Instead of offloading all your data to someone like Google to implement a walled-garden solution, this allows places
like governments, corporations (office buildings), university campus/buildings to provide maps on a DNS level protocol.
Since the DNS ultimately returns servers for that location, each server could implement its own authentication 
if the mapping data was private. This allows for applications to pull from multiple data sources in a cohesive mapping
application. 

They callout that searching of these servers and filtering by reputation is not core to this project, and can be
improved upon later.


## Critique

Ultimately the network / hosts needs to be seeded with sufficiently useful data for another to bother integrating
into an application. If it could piggyback of Google/Apple maps data somehow that would be nice.

Could various proof-of-location protocols be used here to prove data or somehow validate map authors? Seems
like an interesting potential intersection here. 

I wonder if a smart contract (Ethereum) could help at all to handle de-'centralized' lookup problem mentioned. 
You could ensure a central repo to query from while maintaining unbiased decentralization. 4.1

Yay discussing hex and square indexes, but since they don't sub divide well, square indexes are used.

Get rid of altitude, seems like it would rarely if ever be used. 
Just make it a metadata feature on returned results and filtered out client side.
Feels like extra complication to the idea because 'maps are 3d', but the main indexing is 2D and it allows for
multiple authorities so I don't see much harm in returning all servers for a given index, altitude independent. 
I especially hate that its the top most value in the DNS lookup ('Altitude.L5.L4.L3.L2.L1.Face.loc') with "U" for unknown.
4.2

Interesting hierarchical signing scheme, unclear how roots or their children are determined. How can an independent
entity not supported by the local governments provide mapping data? Do I need to go to a root? 3.3.2

4.2 Geo-domains are great besides Altitude, great way to leverage hierarchy of DNS with global location lookup.

4.3 I struggled to understand how overlaps were enabled here.









## Connections

Project home: [open-flame.com](https://www.open-flame.com/) · code: [github.com/openflam](https://github.com/openflam)

[Project Proposal: Nostr Revisited](/cs440-blog-ckozel/2026/10/06/Project-Proposal/) Hey a connection to decentralization!
This lays out the landscape for federated location details. OpenFLAME is built on DNS where Nostr is on its own servers.

[What is 'partial reachability' on the Internet?](/cs440-blog-ckozel/2026/10/01/Partial-Reachability/) saw peering
disputes fracture the Internet core along organizational lines. OpenFLAME allows for multiple claims providing potentially
redundant map data is not all servers can be accessed. 
