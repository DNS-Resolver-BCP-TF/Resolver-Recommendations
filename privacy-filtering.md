## Privacy, Filtering, Transparency

### Privacy & anonymity

Operators are advised to apply [RFC8932](https://www.rfc-editor.org/rfc/rfc8932.html) "Recommendations for DNS Privacy Service Operators" as follows:

  1. its operational and policy guidance related to DNS encrypted transports and data handling, by applying all "Threat mitigations" (thereby by meeting its level of "minimally compliant") and additionally by applying the "Optimizations" on EDNS Client Subnet listed in section 5.3.1.

  2. its framework on a Recursive operator Privacy Statement, by publishing a privacy statement on their website that is compliant with Section 6.

#### Logging considerations

1. Public privacy policy: DNS resolvers are recommended to publish their privacy policies transparently on their website. It can be a brief privacy commitment as well or be more elaborate on how the privacy policy was made. (See for example [Cloudflare's statement](https://developers.cloudflare.com/1.1.1.1/privacy/public-dns-resolver) or [Quad9's privacy page](https://www.quad9.net/service/privacy/).)

2. Third party access to personal data: it seems that the only critical personal data that DNS resolvers collect are IP addresses and the queries that are resolved. The other meta data collected can be used to have an understanding of for example which user accessed which website which can reveal information about a person’s health, lifestyle and other personal preferences (we call this profiling). For example, resolving the website for alcoholics anonymous may tell you something about the health of a person behind an IP address. IP addresses are personally identifiable information. Follow the applicable privacy laws or privacy principles when receiving third party requests to access. Resolvers should only comply with such requests when balancing legitimate third party interest with other fundamental rights.

3. Access to data for researchers: how it is done, who has access and who can request access, how the resolver makes a decision to give access (validated and credible researchers, what they can access and other issues)

4. Data minimization: do not collect personal information not needed for critical operations.  Only retain or use what is being asked (the query). If collecting data to make the service more private and secure, explain the rationale for each piece of data (data collection purpose)

5. Ad policy and encryption: explain the ad policy and how it can potentially affect the users privacy. If data is encrypted, explain how it has been encrypted (DoH, DoT, or so on).

6. Data security and retention: when to delete the data and how it is stored

### Filtering and blocking

#### Legal blocking:
**Legal requests and blocking and filtering laws:** DNS resolvers should not filter content and block access to web-services. When the local law requires blocking, and the law applies to the resolver, the resolver should transparently disclose a list of blocked websites and services.

**Community governance of blocklists:** blocklists, if mandatory, have to be audited and assessed by third parties and there should be a right to appeal for those blocked. The Internet community can vet the blocklists from time to time to avoid blocking access to websites that are mistakenly blocked. During crisis - when mistakes can have drastic effects on accessing a critical service - preferably filtering and blocking should not be used.

#### RPZ-based filtering

Response Policy Zone (RPZ) allows a way to both document specific modifications that resolvers will make to DNS answers, and send the rules to resolvers. Resolvers can be directed to block or modify answers in various ways. Blocklists may be provided by governments, communities, or other parties (for example security firms) using RPZ. This allows updates to occur very quickly. These source must be highly trusted, as changes to blocklists will usually immediately impact user queries.

RPZ is not standardized, but there is an IETF draft, [draft-vixie-dnsop-dns-rpz](https://datatracker.ietf.org/doc/draft-vixie-dnsop-dns-rpz/00/).

#### Opt-in/Opt-out Mechanisms

End users may choose to use a DNS resolver that filters specific kinds of traffic. For example, they may wish to avoid potential malware web sites. Or resolver operators may be required to default to filtering but allowed for to provide an unfiltered DNS resolver service.

Depending on the specific requirements, a resolver service may publish different IP addresses and what type of filtering applies to each address. It is also possible to perform client authentication and authorization, using IP-based authentication, TSIG keys, or client-side TLS certificates.

### Transparency

DNS resolvers usually provide transparency reports once a year. The reports inform the public about disclosure of user information and removal of content required by law enforcement and other government agencies.

Transparency reports should (to the extent that the law allows) indicate which government agencies and law enforcement agencies request access on what basis.

It should also be clear from the transparency reports what kind of data has been requested and if content removal and content blocking have been requested. Categories of data include: Content Data, Basic Subscriber Data, Other Non-Content Data and Content Blocking.

#### Voluntary certificates and standards

Some DNS resolvers opt for obtaining certificates in security and privacy. Some also undertake audits on their privacy practices. See for example:https://www.cloudflare.com/trust-hub/compliance-resources/

#### Human rights considerations

DNS resolvers can opt for declaring their understanding of their responsibilities regarding human rights from the Universal Declaration of Human Rights. Specifically, Quad9 mentions rights to freedoms without distinction made on the basis of country, no interference with privacy, the right to freedom of opinion and expression, the right to peaceful assembly, and the right to freely participate in the cultural life of the community.

See [Quad9's Human Rights Considerations](https://www.quad9.net/privacy/human-rights-considerations/) for the full statement.

It also invokes other human rights related solutions other than [UDHR](https://www.un.org/en/universal-declaration-human-rights/) such as Articles 8 and 9 of Resolution 42/15 of the United Nations Human Rights Council on the right to privacy in the digital age of 26 September 2019 more directly define the responsibilities of the private sector toward the furtherance of human rights in modern terms. They also follow the Guidelines for Human Rights Protocol and Architecture Consideration of the Human Rights Protocol Considerations Research Group at Internet Research Task Force.

The latest version of the IRTF [Guidelines for HRPC](https://tools.ietf.org/html/draft-irtf-hrpc-guidelines) may be considered for all network operators.
