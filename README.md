# DNS Resolver Recommendations

[About the DNS Resolver Best Common Practice Task Force](https://www.ripe.net/participate/ripe/tf/dns-resolver-best-common-practice-task-force)

## Terminology

* Open Resolver: A DNS resolver that accepts queries from any client. Often the result of misconfiguration.

* Public Resolver: A resolver intentionally configured to be an open resolver.

## Introduction

### What Is This Document? Who Is It For?

This document presents recommendations and best current practices for operating DNS resolvers, both public and non-public ones. It covers technical aspects of operations and provides best practice recommendations for data management, with a particular focus on user privacy, security, and resilience.

The document serves as guidance for the wider Internet community, offering input to:

* Those running public DNS resolver services, and
* Those who want to make informed choices between such services.

Its purpose is to provide clear guidance and promote effective practices in DNS
resolver operation.

The intended audience is not the entire DNS community. Advice here is probably not useful for operators of authoritative servers, domain registrars, and so on. It is also not meant to be an introductory or educational document. There are many documents which cover the basics of DNS and the roles of organizations in it; a good overview is [Addressing the challenges of modern DNS - a comprehensive tutorial](https://ris.utwente.nl/ws/files/282427879/1_s2.0_S1574013722000132_main.pdf) by van der Toorn et al.

The document does not consider how to measure adherence to these recommendations. So it is not intended to be used for certification, although certification created based on the principles here is possible.

### How Is This Document Organized?

This document has a number of sections, and specific recommendations in each section. The intent is for each recommendations to have clear guidance at the top, and then background and discussion related to the recommendation afterwards. Each recommendation indicates whether it is mostly for operators of public resolvers or for operators of any resolver.

## Random, unsorted list of things to consider

[Systems Network Capacity](systems-networks.md)

{{systems-networks.md}}


[Knobs to tweak in the DNS](DNS-options.md)

{{DNS-options.md}}


[Privacy, Filtering, Transparency](privacy-filtering.md)

{{privacy-filtering.md}}


[Other Documents of Interest](documents.md)

{{documents.md}}

## Appendix A: Why Did RIPE Write This Document?

There is increasing concern that large open DNS resolvers will become centralised points of DNS operations on the Internet. In order to address this, the European Commission issued the [DNS4EU](https://hadea.ec.europa.eu/calls-proposals/equipping-backbone-networks-high-performance-and-secure-dns-resolution-infrastructures-works_en) proposal. However, such an initiative could lead to centralised guidance or regulation which might interfere with the decentralised way the Internet infrastructure works - including the DNS. See for reference the [RIPE NCC Open House discussion](https://labs.ripe.net/author/chrisb/dns4eu-ripe-ncc-open-house-discussion/) on this topic.

Rather than attempting to respond to the EC proposal or organize specific DNS resolver deployments, the RIPE community has decided that it is best able to provide advice and guidance. The RIPE Community is well positioned to provide a set of Best Current Practices that operators of Open DNS Resolvers will be encouraged to subscribe to.
