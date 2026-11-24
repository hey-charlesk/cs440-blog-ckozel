---
layout: post
title: "Presentation: Tor Hidden Services in a TEE"
date: 2026-11-19
paper_title: "Don't Get Caught, Keep Your Onions in a Vault"
paper_authors: "H. Ikram, R. Habib, M. Ali, Z. A. Uzmi (Carnegie Mellon University / Stanford University / University of Arizona / LUMS)"
paper_venue: "NINeS 2026"
paper_url: "https://drops.dagstuhl.de/entities/document/10.4230/OASIcs.NINeS.2026.17"
week: 9
tags: [tor, hidden-services, anonymity, TEE, cloud, crypto-currency]
slides: /assets/slides/2026-11-24-vaultor-onions-in-a-vault.pdf
---

My slides for presenting this paper in class are at the bottom of this post.

## Key Idea

Tor hides the client, but a normal web server still needs a public IP. Hidden Services fix that by having the client
and server each build a Tor circuit to a rendezvous point, so neither learns the others IP. The catch is the server has
to stay online to be reachable, and a machine that is always on gives an adversary all the time they need to
de-anonymize it. 2014's "Operation Onymous" took down 416 Hidden Services. You also can't just move to the cloud, 
since cloud hosting wants payment details / KYC and the host can snoop on whatever is running. Static hosts like
BitTorrent or IPFS are anonymous but can't run server side code.

VaulTor's answer is to run the Hidden Service on someone else's machine, a "vault", inside a Trusted Execution
Environment (Intel SGX). The TEE keeps the enclave's memory encrypted and isolated from the host OS, and remote
attestation lets the provider check a signed hash (a "quote") proving the vault is running exactly the code they
uploaded. The code itself is visible to the vault, so secrets are sent after: the enclave generates a key pair, puts
the public key in the quote, and the provider encrypts their data to it. The provider only connects to the vault now
and then over Tor, can rotate between vaults, and presumably pays in crypto. 

Tor + TEE = anonymous cloud compute. In their tests page load times in the TEE were about the same as normal Tor, 
the Tor circuit is the slow part not the enclave.

## Critique

The whole paper is an idea. No live network of vaults, no code or algorithms, minimal testing. Finding and paying
for a vault is out of scope, but that marketplace is the hard part. It needs enough vaults to be reliable before
anyone would use it, and someone has to take that upfront risk and cost.

Your data now lives on the vault. You need to keep in sync with it and risk losing data when it goes offline. A vault 
is also one machine where a real service could be a cluster, nothing here about scaling or failover. As discussed
in class a TEE is not reliable against hardware attacks. If your so concerned about getting your IP discovered, you
probably don't want to risk a TEE hosting your service and using a TEE exploit to access your service's data.

## Connections

[Project Proposal: Nostr Revisited](/cs440-blog-ckozel/2026/10/06/Project-Proposal/) and
[OpenFLAME](/cs440-blog-ckozel/2026/11/05/openflame-spatial-naming/) are the other decentralization posts. In
OpenFLAME I wondered if an Ethereum smart contract could do the lookup, same question here for compute. A smart contract
is anonymous code hosting too, but the memory and I/O are public, you pay per instruction and it has no Internet access.

