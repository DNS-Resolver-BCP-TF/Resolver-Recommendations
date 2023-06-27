
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
