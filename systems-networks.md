
## Systems and Networks Hardening

* Capacity
  - CPU/network
  - Multi-layer caching
  - How to estimate
* Resilience
  - Diversity of software, geography, toplogy.
  - Bare metal vs. VM vs. containers, self-hosted vs. hosted vs. cloud
  - Diversity of organizations, legal frameworks
  - (D)DoS measures, such as filtering/rate-limiting traffic, both authoritative and client sides
  - Common HA designs in DNS resolver space
  - Security best practices (keep stuff updated, follow CERTs, and so on)

* Software Considerations
  - Open Source advantages (and disadvantages), licenses
  - Custom tweaks/implementations
  - Platforms (it's all Unix these days)


=======
### Network considerations

* Network Resiliency
  - RPKI, other BGP tricks
    - IPv4 AND IPv6
    - Egress Filtering
        - [BCP38](https://www.rfc-editor.org/rfc/rfc2827.html)

* Anycasting
  - Why and how (especially problems with listing multiple resolvers in user configurations).
  - Other options to anycasting?


Care should be taken to allocate the IP Networks used for public resolvers.
Both IPv4 and IPv6 MUST be deployed.
Egress Filtering should be done to follow BCP38
Additionally, sign route advertisements using RPKI

To be robust, using anycast to announce network routes should be used.
Not deploying some solution to announce networks in multiple locations will incur system performance.
To add further resiliency, multiple network allocations in different RIRs should be considered.

discuss BGP fallback issues - announcing a /23 via BGP and as a fallback statically pin up a /24 as fallbacks

Should have 2 (or maybe more) IP addresses per address family.

Ideally addresses from different RIRs. The threat is dealing with failures at an RIR, either governance, security, or technical.


Publishing a list of back-end addresses used for resolving can be useful for other network & DNS operators (for example, geo-IP location, making sure data is getting to correct places, and so on).

Sign RPKI routes, validating is more optional
discuss cost / benefit of validating (less spoofed traffic but cost of router cycles)


=======
### Software considerations

#### Open Source

**Recommendation**: Choose any well-maintained DNS software you are comfortable using. Regardless of which software you choose, ensure you have somewhere to go for support. In the case of open source software, consider providing financial support to ensure continued development. Some open source maintainers take donations, while others offer support contracts.

There are both open source and proprietary implementations of DNS resolver software. Mixing these is also possible, for example, by using proprietary extensions with open source software or deploying open source software modified in-house.

General observations:
  - Software licensing is orthogonal to software security. Neither is proprietary software less secure on principle nor are contributions by "unknown" developers more of a risk in open source.

Benefits of open source:
  - Open source allows for inspection, independent auditing, and troubleshooting.
  - Open source can avoid vendor lock-in.
  - Open source can aid internet standards development. Widely-deployed open source implementations allow proponents of standards drafts to contribute proof of concept implementations without permission or cooperation of vendors.

Drawbacks of open source
  - Both open source and proprietary software require skilled maintenance, which has costs. Proprietary licensed software or appliances typically come with license fees to cover these. In contrast, open source licenses decouple usage by operators from monetary compensation to developers. It is up to operators to consider the financial sustainability of continued maintenance of the open source DNS software they depend upon.

Please also consider deploying different software implementations to ensure diversity, as discussed in the diversity section TODO:REF.
