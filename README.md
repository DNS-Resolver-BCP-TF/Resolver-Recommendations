# DNS Resolver Recommendations


Document should consist of a set of considerations & recommendations.
Question: Is it important to consider how to measure adherence to these recommendations?

## Introduction

[In which we include some motivations about the document, who it is for, explain how it is organized, and offer a money-back guarantee.]

## Random, unsorted list of things to consider

* Capacity
  - CPU/network
  - Multi-layer caching
  - How to estimate
* Resilience
  - Diversity of software, geography, toplogy.
  - Bare metal vs. VM vs. containers, self-hosted vs. hosted vs. cloud
  - Diversity of organizations, legal frameworks
  - (D)DoS measures, such as filtering/rate-limiting traffic, both authoritative and client sides
  - RPKI, other BGP tricks
  - Common HA designs in DNS resolver space
  - Security best practices (keep stuff updated, follow CERTs, and so on)
* Anycasting
  - Why and how (especially problems with listing multiple resolvers in user configurations).
  - Other options to anycasting?
* Software Considerations
  - Open Source advantages (and disadvantages), licenses
  - Custom tweaks/implementations
  - Platforms (it's all Unix these days)
* Knobs to tweak in the DNS
  - TTL limits (max & min)
  - Local root (and maybe local TLD?); verify root zone with ZONEMD if it ever arrives
  - EDNS0 sizes to minimize fragmentation, especially for IPv6
  - Aggressive NSEC caching
  - QNAME minimization
  - Negative trust anchors
  - TTL record pre-fetch
  - EDNS client subnet
  - DNS cookies shared secret
  - DoT, DoH, DoQ
  - Trust anchor reporting
  - DNS error reporting
* Privacy & anonymity  
  - Logging considerations
  - How to handle user accounts
* Filtering
  - Legally required blocking (how to figure out which applies to any given query?)
  - RPZ-based filtering
  - Opt-in/opt-out mechanisms
* Transparency
  - Policies
  - Finances, ownership, and so on
  - Outages
  - Statistics
* Finances
  - How to pay for all of this?
* Communication channels
  - Web page
  - E-mail (DANE protected)
  - Security reporting channels
  - Regular reports
  - Snarky Mastodon intern completely optional
