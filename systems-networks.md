## System and Network Hardening

### Infrastructure considerations

Running any Internet service requires attention to the infrastructure used to operate it. This section discusses various approaches that can be used to run a DNS resolver. Everything applies to both public and non-public DNS resolvers.

#### Bare metal or public cloud

All DNS resolver software can run either on dedicated servers (rented or colocated), or in virtualized clouds, or in a combination of those. Every approach has pros and cons. Most of these are not specific to running DNS resolvers, however, some of them are.

**Running DNS resolver instances as OS level daemons on bare metal hosts:**

Pros:

  - Performance: Bare metal servers have direct access to the underlying hardware, and can offer superior performance/cost balance by avoiding the overhead associated with virtualization. Moreover, you have full control over the server's configurations, down to the hardware level, which can be beneficial for performance and cost optimization once you get the understanding of your typical work load during peak hours.

  - Data Security: Since you are in control of the physical servers, there is no risk of data leakage that can occur due to vulnerabilities in multi-tenant virtualization platforms, including CPU cache-based side-channel vulnerabilities. It could be argued that attacks targeting such issues are rare, and their impact on a DNS resolver service is low, but potential breaches may have significant privacy impact. It is advised to evaluate this against your organisation's risk model, or to discuss this with your information security compliance experts.

  - Predictability: Because there is no virtualization layer and no "noisy neighbours" on the host, the performance of your servers is more predictable.

Cons:

  - Cost of failure: If you pick hardware configuration that is not optimal for the workload of your DNS resolver, you may need to upgrade and replace hardware components afterwards. Ways to reduce this risk include renting servers instead of buying them, carrying load testing with data similar to production workloads, and providing limited beta access to the service before it fully enters the production phase.

  - Scalability: Scaling up with physical servers means acquiring or renting, installing, and configuring new hardware, which will take more time than provisioning new virtual servers in a cloud environment. Moreover, most cloud environments will provide you with cluster autoscaling features, which could barely be achieved in bare metal.

  - Maintenance: You will be responsible for all server maintenance tasks, including hardware issues, which can require significant effort and specific expertise.

  - Redundancy: Setting up high availability and disaster recovery strategies can be more complex and time consuming compared to the cloud, where these features are often provided as value added products. See the Redundancy section for more details.

**Running DNS resolver instances in containers in a public cloud:**

Pros:

  - Scalability: Clouds excel at scaling applications. You can scale up and down rapidly based on load, which is important for a DNS resolver that needs to handle variable query loads. In case of regional or geographically distributed resolvers, in every region where the resolver would be deployed, daily periodicity is likely to be observed, for example peak hour is likely to occur around 19:00 local time, and off-peak hours may begin at around 01:00-03:00. In a situation like that, using cluster autoscaling features and tools, you can run less instances in the night and more instances throughout the day, which may help to optimize your cloud hosting costs.

  - Fault Tolerance and High Availability: Most clouds have built-in strategies, features, and products for handling node failures, which can increase your service's availability.

  - Deployment and Management: Cloud providers offer built-in methods to deploy and manage applications, which can simplify operations and reduce the likelihood of human errors if your infrastructure management department is already familiar with these tools.

  - Cost: While this largely depends on your specific usage, cloud services can sometimes be more cost-effective than managing your own physical servers, especially when you consider the total cost of ownership, including power, cooling, and maintenance.

Cons:

  - Performance: The virtualization layer of public clouds can impact performance. While this certainly could be mitigated through scaling the number of virtual hosts, the cost would also increase accordingly.

  - Complexity: Advanced cloud technologies are complex systems which come with a steep learning curve. Without prior experience, properly configuring and managing a cloud-based compute cluster can be challenging.

  - Cost Variability: While the cloud can be cheaper, it can also be more expensive if not properly managed. Costs can rise unexpectedly based on traffic. Make sure to always set some limits on how much may be spent on hosting in the cloud control panel, and to set up notifications to be sent to you when these thresholds are about to be triggered.

  - Multi-tenancy Risks: In a public cloud environment, the "noisy neighbour" problem could potentially affect your service's performance. Additionally, even though cloud providers take steps to isolate tenant environments, vulnerabilities could potentially expose sensitive data (see the previous section for a detailed explanation).

**Additional considerations**

  - In today's environments, Kubernetes and Terraform are sometimes used as a substitute for cloud APIs when it comes to production services' management. When running a DNS resolver in a Kubernetes cluster on top of a public cloud environment, all the pros and cons of the public cloud apply; basically, Kubernetes becomes your public cloud provider. If you have significant prior experience running services in Kubernetes in production, you may successfully replicate your experience with the DNS resolver software. Otherwise, we would advise against Kubernetes in this case.

  - The only reason we may find to run a DNS resolver in a Kubernetes cluster on top of self-hosted dedicated servers is when you have significant hands-on experience with Kubernetes and it is natural for you to manage applications this way. Otherwise, running DNS resolver daemons in containers brings little, if any, benefit. Autoscaling features are not available to you in this case, and neither horizontal nor vertical pod autoscaling is of any use, because DNS resolver software typically scales in-host by itself just fine.

  - When designing a cluster of resolvers for autoscaling, keep in mind that newly spawned resolver machines would need to populate resolver cache first before they are fully useful. Your DNS resolver software may provide cache replication mechanisms. Otherwise, it is safe to overprovision clusters somewhat under heavy load, and discarding excessive instances once all the caches are populated and the average load of a compute instance decreases. In addition, it may be worthwhile to consider sharing cache data between instances.

  - It is always advised to prefer environments your infrastructure management team is familiar with.

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

### Networking considerations

#### IPv4 and IPv6

**If available, both IPv4 and IPv6 must be deployed.**

Large parts of the authoritative DNS are only accessible via IPv4, so the resolver must be able to originate IPv4 queries. Authoritative DNS that is only accessible via IPv6 is very rare.

Depending on the connectivity of clients, a resolver may be IPv4-only, IPv6-only, or support IPv4 and IPv6.

#### Addressing

**Using multiple IP addresses for the service address should be considered.**

Using 2 or more IPv4 addresses and 2 or more IPv6 addresses from different RIR will allow resilience in failure at an RIR, either governance, security, or technical. Note that support for multiple addresses for recursive resolvers varies and some clients perform poorly if any address does not respond normally.

There is no need to pick an IPv4 address with all octets the same, like 2.2.2.2 or 11.11.11.11.

**Publishing a list of back-end addresses used for resolving should be considered.**

Publishing a list of back-end addresses used for resolving can be useful for other network & DNS operators (for example, geo-IP location, making sure data is getting to correct places, and so on).

#### Anycasting

**Anycasting may be considered**

Anycasting means routing the same IP prefix to more than one location. As mentioned above for addressing, client support for multiple addresses is not always good; with anycasting you can use a single IP address and have redundancy from different sites. This will often allow you to place sites close to the user - although it is tricky to get optimal routing with BGP.

For a resolver service with a single site there is no benefit. For a resolver service with multiple sites, it may be better to configure clients with different IP addresses rather than use anycasting.

[RFC7094](https://www.rfc-editor.org/rfc/rfc7094.html) discusses anycast in detail, including references to various other RFC which discuss anycasting in general and to DNS in particular.

If a separate prefix is to be used for anycasting, usually this means a /24 in IPv4 and a /48 in IPv6, as those are the smallest sizes that will be widely propagated in BGP. A common practice is to use a covering prefix (/23 in IPv4 or /47 in IPv6) for fallback, and a more-specific prefix (/24 or /48) for the traffic. The more-specific prefix can then be withdrawn to send traffic to a backup site; this will happen automatically if the site is disconnected from routing.

#### Ingress Filtering

**Ingress Filtering to follow BCP 38 should be deployed.**

DNS normally uses UDP traffic, which makes it a common vector of both [reflection](https://en.wikipedia.org/wiki/Reflection_attack) and [amplification](https://www.cisa.gov/news-events/alerts/2014/01/17/udp-based-amplification-attacks) attacks. To minimize the amount of spoofed traffic that a resolver responds to, the network should be configured as recommended in [BCP 38](https://www.rfc-editor.org/rfc/rfc2827.html).

#### RPKI Sign Advertised Routes

**Route Advertisements should be signed using RPKI**

Using RPKI to sign any route advertisements - either toward authoritative servers or toward DNS clients - is straightforward to do and will reduce the impact of BGP misconfigurations and some BGP hijacking attempts.

RPKI validation is also possible, although the effort is greater. It is possible that the hosting provider or the transit provider for your service validates BGP; asking and making this part of your selection criteria is reasonable.

#### (D)DoS measures

Denial-of-Service (DoS) attacks, both distributed (DDoS) and not are a threat to any Internet service. Network operators for a service providing any DNS service must be prepared for large amounts of attack traffic.

In addition to attacks on the service itself, a resolver may be used both as an attack reflector and as an attack amplifier.

Active monitoring of network and service usage, careful logging, and a security team that is able to respond to problem reports is necessary. Mitigation techniques will include filtering or rate-limiting traffic, both on the authoritative and client side of the resolver.

### Capacity planning

#### Server capacity

If using a model that is easy to scale (cloud based, or Kubernetes based, or similar), then getting server capacity correct is largely a question of budgeting. If using a less-flexible model (bare metal for example), then under-estimating will mean problems delivering service.

Hardware performance varies widely, as does operating system and resolver performance. Some lab testing will be necessary to estimate the number of systems needed.

#### Network capacity

Since DNS is mostly UDP-based, it is often easy to generate large amounts of spoofed traffic to and from DNS servers. DNS traffic is small compared to application traffic (videos and other content), but still significant. Authoritative server operators often build their networks and servers to handle 10 times their normal load. Recursive server operators may need to do the same, although the service only accepts traffic from IP addresses that cannot be spoofed (for example users within a network that operated by the same company) then this can be reduced, for example to 3 times normal load. To estimate expected load, the best approach is to examine historical usage for the actual expected users of the system.

### Resilience

#### System Diversity

In addition to the software considerations above, operators should consider whether to use different server implementations to provide service. This allows continued operation if a critical vulnerability is found in one implementation, by shifting traffic to other implementations.

Placing resolvers and control systems in different physical locations will
allow continued operation in the event of a disaster or other problem that impacts a single location. In addition, ensuring diverse connectivity to other networks will prevent single points of failure on the network side. Ensuring network diversity may take some care, as it is not always obvious what fate is shared between any given path; this may be physical, virtual, or organizational, and my sometimes be hidden.

#### Security

In addition to the DNS-specific security considerations, normal security best
practices for any Internet service should be followed, including updating
software updated regularly, patching software as soon as possible for any
known security vulnerabilities, following CERT announcements and so on.

#### Certification

It may be useful or required for an organization to follow specific certifications, such as ISO or ITIL. These can be government-defined or industry-defined. For end users there is typically not much direct value, but business customers will often look for services that are operated by organizations meeting such standards.
