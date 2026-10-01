---
layout: post
title: "What is 'partial reachability' on the Internet?"
date: 2026-10-01
paper_title: "Understanding Partial Reachability in the Internet Core"
paper_authors: "G. Baltra, T. Saluja, Y. Pradkin, J. Heidemann"
paper_venue: "NINeS 2026"
paper_url: "https://2026.nines-conference.org/papers/p004-Baltra.pdf"
week: 2
tags: [architecture, routing, decentralization, politics]
---

## Key Idea

Being composed of many different network providers and physical routes, the Internet is
not as strongly connected as we might like to believe. Baltra et al. define two classes of
connectivity issues, islands and peninsulas, and provide algorithms to identify them
from [Trinocular](https://ant.isi.edu/outage/) and [RIPE Atlas](https://atlas.ripe.net/) data.
Islands are portions of the network which are isolated
from the Internet core but still use the public IP address space. Peninsulas are network
portions that can reach some of the Internet core, but not all of it. (Long-term peninsulas
are typically due to peering disputes.)

## Critique

The paper otherwise does a great job stating terms and defining them in detail,
but it never defines "partial reachability". Since it's an essential term used throughout
the paper, I would have liked an explicit definition up front. I did really love
the table of data sources used called out upfront in one spot.

A confusing aspect was how to conceptualize 'islands'. The term and its technical definition
sound like intentional isolation from the Internet core, but in practice islands are often
temporary due to outages.

## Connections

[-empty-]
