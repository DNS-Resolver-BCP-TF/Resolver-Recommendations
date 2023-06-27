
## DNS configuration knobs

* Knobs to tweak in the DNS
  - TTL limits (max & min)
  - Local root (and maybe local TLD?)
    - [RFC8806](https://www.rfc-editor.org/rfc/rfc8806.html)
  - Verify root zone with ZONEMD
    - [RFC8976](https://www.rfc-editor.org/rfc/rfc8976.html)
  - EDNS0 sizes to minimize fragmentation, especially for IPv6
      - [draft-ietf-dnsop-avoid-fragmentation](https://datatracker.ietf.org/doc/draft-ietf-dnsop-avoid-fragmentation/)
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

