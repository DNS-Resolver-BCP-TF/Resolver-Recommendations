## DNS configuration knobs

The DNS is an old protocol that has a lot of settings that can be tweaked. This section reviews these and provides recommendations on which should be used for a resolver.

### DNSSEC validation

**DNSSEC validation should be enabled.**

For: All DNS resolver operators.

DNSSEC validation is the best way to ensure that the answers from the owner of domain name being queried are returned.

The root KSK must be updated when it changes. While [RFC5011](https://www.rfc-editor.org/rfc/rfc5011.html) defines an automated way to do this, a resolver operator will probably either manage this trust anchor directly or have it updated via OS updates.

[RFC9364](https://www.rfc-editor.org/rfc/rfc9364.html) provides a lot of useful information, and links to further documents about DNSSEC. However, operators usually do not need to know the details, and can simply ensure that DNSSEC validation is enabled in their software; this is usually enabled by default.

Resolver software that does not support DNSSEC validation should be avoided.

### DNS Transport Protocols

**UDP and TCP must be supported.**

For: ALL DNS resolver operators.

UDP is what most clients use, and TCP is necessary for DNS answers that are too
large for a single UDP packet.

[RFC7766](https://www.rfc-editor.org/rfc/rfc7766.html) explains why TCP is necessary in more detail.

### Packet Fragmentation Avoidance

**Servers should be configured to avoid fragmentation.**

For: ALL DNS resolver operators.

Packet fragmentation can cause issues with DNS over UDP, especially over IPv6. These issues can be minimized by choosing implementations that set IP options to avoid this, and by taking care with EDNS0 message sizes.

Recommendations are available in [draft-ietf-dnsop-avoid-fragmentation](https://datatracker.ietf.org/doc/draft-ietf-dnsop-avoid-fragmentation/).

### Encrypted DNS

**DNS-over-TLS (DoT), DNS-over-HTTPS (DoH), and DNS-over-QUIC (DoQ) should be supported.**

For: All DNS resolver operators.

DoT, DoH, and DoQ are different technologies that all provide an encrypted channel between the resolver and the authoritative server. DoT is the oldest, and provides encrypted DNS using TLS. DoH uses HTTP over TLS as a way to transmit queries and answers, and is widely supported by web browsers. DoQ is the newest, and provides advanced features such as separate streams for each query, avoiding the "head of line" blocking problem common with all protocols layered on top of TCP (such as DoT and DoH).

- DoT
  - [RFC7858](https://www.rfc-editor.org/rfc/rfc7858.html)
- DoH
  - [RFC8484](https://www.rfc-editor.org/rfc/rfc8484.html)
- DoQ
  - [RFC9250](https://www.rfc-editor.org/rfc/rfc9250.html)

QUESTION: How to publish certificates? Need something about Designated Resolvers.

**Discovery of DNS Designated Resolvers**

There are new mechanisms that allow DNS clients to use DNS records to discover
encrypted DNS configurations.  Resolvers should publish DNS records to assist
clients finding encrypted resolvers.

- Discovery of Designated Resolvers
  - [RFC9462](https://www.rfc-editor.org/rfc/rfc9462.html)


### QNAME Minimization

**QNAME minimization should be enabled.**

For: All DNS resolver operators.

Using QNAME minimization, a resolver does not send the full name that it is trying to resolver to authoritative servers higher in the DNS hierarchy. So, for example, when querying "atlas.ripe.net" the servers for ".net" would only be asked for "ripe.net". This improves privacy for the end user querying the name.

[RFC7816](https://www.rfc-editor.org/rfc/rfc7816.html) covers QNAME minimization

### Aggressive NSEC caching

**Aggressive NSEC caching should be enabled.**

For: Public resolver operators.

"Aggressive NSEC caching", meaning negative caching based on NSEC and NSEC3 values, can reduce traffic greatly. It is important to protect against random subdomain attacks.

This style of caching takes advantage of the way that NSEC and NSEC3 records cover a range of names in a zone. A resolver can know that a query falls within such a range without sending any further queries, by remembering the NSEC or NSEC3 redords that is has seen as answers to earlier queries.

Aggressive NSEC caching is almost always a good idea. However enabling this is less important for DNS resolver operators who have a close relationship with users, since they can stop attacks by blocking users or otherwise directly dealing with the source of abusive queries.

[RFC8189](https://www.rfc-editor.org/rfc/rfc8189.html) describes negative caching in detail.

### Local Root

**Local root should be used.**

For: Public resolver operators.

Since the root zone is DNSSEC signed,

Running a local root has several benefits, but it is an additional component to maintain. For public resolver operators this is definitely worth the cost, but other resolver operators may choose to simply send all queries to the well-distributed root name servers.

[RFC8806](https://www.rfc-editor.org/rfc/rfc8806.html) describes local root, including several example configurations.

In the future it will be possible to use ZONEMD to validate the copy of the root zone obtained before using it. This is currently being deployed for the root zone, but not yet available.

[RFC8976](https://www.rfc-editor.org/rfc/rfc8976.html) describes ZONEMD.

### DNS Cookies

**Interoperable DNS Cookies should be supported.**

For: Public resolver operators.

DNS cookies provide some improved security over plain UDP, and are designed to be more lightweight than TCP. If more than one server is responding for a given IP address, then the Server Secret must be shared by all servers, and the answer must be constructed in a consistent manner by all server implementations.

Since client-side support for DNS cookies is not very widespread, and since managing server-side secrets involves some work, the costs may outweigh the benefits for some non-public resolver operators.

[RFC7873](https://www.rfc-editor.org/rfc/rfc7873.html) describes DNS cookies, and [RFC9018](https://www.rfc-editor.org/rfc/rfc9018.html) standardizes shared DNS cookies.

### TTL Recommendations

**TTL limits may be adjusted.**

For: All DNS resolver operators.

Software typically defaults to a maximum stored TTL of 1 or 2 days. This may be lowered to reduce the cache size. A lower TTL will mean removing rarely-used records that have long TTL, and should not have much operational impact from a CPU or network point of view, but may save memory.

It is possible to set a minimum TTL in many implementations. This is a violation of the DNS protocol, although may be useful to reduce load from records with very low TTL (less than 5 seconds).

Note that software may set different maximum and minimum TTL independent of the results that the resolver returns. That may have a significant impact on queries as well, but resolver operators cannot influence that.

### TTL-based Record Pre-Fetch

**TTL record pre-fetch should be enabled when available.**

For: All DNS resolver operators.

Some resolvers have the ability to look up a record before it has expired from cache, in order to refresh the value and extend the TTL. This way there is never a time when the records are missing from the cache. This is not currently standardized, but a form of this was proposed in the IETF as [DNS Hammer](https://datatracker.ietf.org/doc/html/draft-wkumari-dnsop-hammer-03). We recommend turning this feature on if available.

### EDNS Client Subnet (ECS)

**ECS may be enabled.**

For: All DNS resolver operators.

EDNS Client Subnet (ECS) allows the resolver to include information about the IP address of the client querying it when sending messages to authoritative servers. This may allow authoritative servers to provide different answers which are more appropriate for the client. However, ECS will increase the amount of cache space required by resolvers, may reduce DNS performance, and may have privacy implications.

A resolver operator that has clients that are limited to a specific region may see no benefit. A resolver operator that has a widely distributed anycast network may not have much benefit from ECS, since the locations that initiate the query will be close to the client. But a resolver operator that answers client queries only from a few locations, and expects clients to come from a wide area, may provide better service for end-users by supporting ECS.

EDNS client subnet is described in [RFC7871](https://www.rfc-editor.org/rfc/rfc7871.html), an informational RFC.

### Extended DNS Errors

**Extended DNS errors should be enabled.**

For: All DNS resolver operators.

DNS traditionally provides very broad error reporting, SERVFAIL being the most common. This makes diagnosing and fixing problems difficult. Extended DNS errors provide extra information about failures, for example expired DNSSEC signatures. They also allow resolver operators to report administrative reasons for DNS failures, such as blocks due to legal requirements.

[RFC8914](https://www.rfc-editor.org/rfc/rfc8914.html) defines extended DNS errors.

### Negative Trust Anchors

**Negative trust anchors may be deployed.**

For: All DNS resolver operators.

Negative trust anchors (NTA) allow a resolver operator to handle a case where an authoritative server has a DNSSEC problem and becomes inaccessible. They basically disable DNSSEC checking for a domain. When this is warranted is difficult to know with certainty, and will usually requires some manual checking. Since DNSSEC validation is now widespread, DNSSEC failures on the authoritative side will impact many resolvers.

Because of these reasons this document does not recommend NTA, but also does not recommend that a deployment avoid NTA if it makes sense for that environment.

Negative trust anchors are documented in [RFC7646](https://www.rfc-editor.org/rfc/rfc7646.html).

### DNS Error Reporting

**DNS error reporting may be enabled.**

For: All DNS resolver operators.

DNS error reporting is a way for resolver operators to let authoritative operators know about problems in authoritative servers or zones. It provides little direct value for the resolver operators, but over time should improve the overall quality of the DNS ecosystem. It is neither widely deployed nor standardized, but hopefully will be both soon. Resolver operators are encouraged to enable DNS error reporting when it is available.

DNS error reporting is proposed in [draft-ietf-dnsop-dns-error-reporting](https://datatracker.ietf.org/doc/draft-ietf-dnsop-dns-error-reporting).

### Trust Anchor Reporting

**Trust anchor reporting may be enabled.**

For: All DNS resolver operators.

Trust anchor reporting is a way for resolver operators to convey their DNSSEC trust anchor configuration to the operator of the zone that it is for. For most resolvers this is only the root zone. This information is intended to be used during a root KSK rollover to ensure that it is safe to proceed. In the future ICANN is planning an algorithm roll for the root KSK, and this information could be helpful. Resolver operators are encouraged to enable trust anchor reporting.

[RFC8145](https://www.rfc-editor.org/rfc/rfc8145.html) covers trust anchor reporting, in both possibilities available.
