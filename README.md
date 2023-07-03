# DNS Resolver Recommendations

[About the DNS Resolver Best Common Practice Task Force](https://www.ripe.net/participate/ripe/tf/dns-resolver-best-common-practice-task-force)

Document should consist of a set of considerations & recommendations.
Question: Is it important to consider how to measure adherence to these recommendations?

## Introduction

[EDITOR'S NOTE: Here comes the intro in which we include some motivations about the document, who it is for, explain how it is organized, and offer a money-back guarantee.Below is a start, but it needs some additional work]

This document presents best current practices and recommendations for operating DNS resolvers, both public and non-public ones. It covers technical aspects of operations and provides best practice recommendations for data management, with a particular focus on user privacy, security and resilience.

The document serves as guidance for the wider Internet community, offering input to those running public DNS resolver services and those who want to make informed choices between such services. Its purpose is to provide clear guidance and promote effective practices in DNS resolver operation.

## Random, unsorted list of things to consider

[Systems Network Capacity](systems-networks.md)

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

[Knobs to tweak in the DNS](DNS-options.md)

* Knobs to tweak in the DNS
  - TTL limits (max & min)
  - Local root (and maybe local TLD?)
    - [RFC8806](https://www.rfc-editor.org/rfc/rfc8806.html)
  - Verify root zone with ZONEMD
    - [RFC8976](https://www.rfc-editor.org/rfc/rfc8976.html)
  - EDNS0 sizes to minimize fragmentation, especially for IPv6
  - Aggressive NSEC caching
    - [RFC8189](https://www.rfc-editor.org/rfc/rfc8189.html)
  - QNAME minimization
    - [RFC7816](https://www.rfc-editor.org/rfc/rfc7816.html)
  - Negative trust anchors
    - [RFC7646](https://www.rfc-editor.org/rfc/rfc7646.html)
  - TTL record pre-fetch
  - EDNS client subnet
    - [RFC7871](https://www.rfc-editor.org/rfc/rfc7871.html)
  - DNS cookies shared secret
    - [RFC7873](https://www.rfc-editor.org/rfc/rfc7871.html)
  - DoT
    - [RFC7858](https://www.rfc-editor.org/rfc/rfc7858.html)
  - DoH
    - [RFC8484](https://www.rfc-editor.org/rfc/rfc8484.html)
  - DoQ
    - [RFC9250](https://www.rfc-editor.org/rfc/rfc9250.html)
  - Trust anchor reporting
  - DNS error reporting
    - [draft-ietf-dnsop-dns-error-reporting](https://datatracker.ietf.org/doc/draft-ietf-dnsop-dns-error-reporting)

[Privacy, Filtering, Transparency](privacy-filtering.md)

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

[Other Documents of Interest](documents.md)
