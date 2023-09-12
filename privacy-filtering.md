
## Privacy, Filtering, Transparency

### Privacy & anonymity

Operators are advised to apply RFC 8932 "Recommendations for DNS Privacy Service Operators" as follows:

  1. its operational and policy guidance related to DNS encrypted transports and data handling, by applying all "Threat mitigations" (thereby by meeting its level of "minimally compliant") and additionally by applying the "Optimizations" on EDNS Client Subnet listed in section 5.3.1.

  2. its framework on a Recursive operator Privacy Statement, by publishing a privacy statement on their website that is compliant with Section 6.

**Logging considerations**:

**Third party access to personal data:** it seems that the only critical personal data that DNS resolvers collect are IP addresses and the queries that are resolved. The other meta data collected can be used to have an understanding of for example which user accessed which website which can reveal information about a person’s health, lifestyle and other personal preferences (we call this profiling). For example, resolving the website for alcoholics anonymous may tell you something about the health of a person behind an IP address. IP addresses are personally identifiable information. Follow the applicable privacy laws or privacy principles when receiving third party requests to access. Resolvers should only comply with such requests when balancing legitimate third party interest with other fundamental rights. 

**Access to data for researchers:** how it’s done, who has access and who can request access, how the resolver makes a decision to give access (validated and credible researchers, what they can access and other issues) 

**Data minimization:** do not collect personal information not needed for critical operations.  Only retain or use what is being asked (the query). If collecting data to make the service more private and secure, explain the rationale for each piece of data (data collection purpose) 

**Ad policy and encryption:** explain the ad policy and how it can potentially affect the users privacy. If data is encrypted, explain how it’s been encrypted (DoH, DoT etc)

**Data security and retention:** when to delete the data and how it’s stored

**How to handle user accounts:**

### Filtering
Filtering and blocking should be done under exceptional circumstances, when there is no other proportional way to block access to certain web-services.  

#### Legal blocking: 
**Legal requests and blocking and filtering laws:** DNS resolvers should not filter content and block access to web-services. When the local law requires blocking, and the law applies to the resolver, the resolver should transparently disclose a list of blocked websites and services. 

**Community governance of blocklists:** blocklists, if mandatory, have to be audited and assessed by third parties and there should be a right to appeal for those blocked. The Internet community can vet the blocklists from time to time to avoid blocking access to websites that are mistakenly blocked. During crisis when mistakes can have drastic effects on accessing a critical service, preferably filtering and blocking should not be used. 

  - Legally required blocking (how to figure out which applies to any given query?)
  - RPZ-based filtering
      - [RFC8976](https://www.rfc-editor.org/rfc/rfc8976.html)
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
