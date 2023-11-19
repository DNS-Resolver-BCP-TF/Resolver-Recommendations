## System and Network Hardening



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

* Network Resiliency
    - IPv4 AND IPv6
    - Egress Filtering
        - [BCP38](https://www.rfc-editor.org/rfc/rfc2827.html)

=======
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

  - Scalability: Clouds excel at scaling applications. You can scale up and down rapidly based on load, which is important for a DNS resolver that needs to handle variable query loads. In case of regional or geographically distributed resolvers, in every region where the resolver would be deployed, daily periodicity is likely to be observed, e.g. peak hour is likely to occur around 19:00 local time, and off-peak hours may begin at around 01:00-03:00. In a situation like that, using cluster autoscaling features and tools, you can run less instances in the night and more instances throughout the day, which may help to optimize your cloud hosting costs.

  - Fault Tolerance and High Availability: Most clouds have built-in strategies, features, and products for handling node failures, which can increase your service's availability.

  - Deployment and Management: Cloud providers offer built-in methods to deploy and manage applications, which can simplify operations and reduce the likelihood of human errors if your infrastructure management department is already familiar with these tools.

  - Cost: While this largely depends on your specific usage, cloud services can sometimes be more cost-effective than managing your own physical servers, especially when you consider the total cost of ownership, including power, cooling, and maintenance.

Cons:

  - Performance: The virtualization layer of public clouds can impact performance. While this certainly could be mitigated through scaling the number of virtual hosts, the cost would also increase accordingly.

  - Complexity: Advanced cloud technologies are complex systems which come with a steep learning curve. Without prior experience, properly configuring and managing a cloud-based compute cluster can be challenging.

  - Cost Variability: While the cloud can be cheaper, it can also be more expensive if not properly managed. Costs can rise unexpectedly based on traffic. Make sure to always set some limits on how much may be spent on hosting in the cloud control panel, and to set up notifications to be sent to you when these thresholds are about to be triggered.

  - Multi-tenancy Risks: In a public cloud environment, the "noisy neighbor" problem could potentially affect your service's performance. Additionally, even though cloud providers take steps to isolate tenant environments, vulnerabilities could potentially expose sensitive data (see the previous section for a detailed explanation).

**Additional considerations**

  - In today's environments, Kubernetes and Terraform are sometimes used as a substitute for cloud APIs when it comes to production services' management. When running a DNS resolver in a Kubernetes cluster on top of a public cloud environment, all the pros and cons of the public cloud apply; basically, Kubernetes becomes your public cloud provider. If you have significant prior experience running services in Kubernetes in production, you may successfully replicate your experience with the DNS resolver software. Otherwise, we would advise against Kubernetes in this case.

  - The only reason we may find to run a DNS resolver in a Kubernetes cluster on top of self-hosted dedicated servers is when you have significant hands-on experience with Kubernetes and it is natural for you to manage applications this way. Otherwise, running DNS resolver daemons in containers brings little, if any, benefit. Autoscaling features are not available to you in this case, and neither horizontal nor vertical pod autoscaling is of any use, because DNS resolver software typically scales in-host by itself just fine.

  - When designing a cluster of resolvers for autoscaling, keep in mind that newly spawned resolver machines would need to populate resolver cache first before they are fully useful. Your DNS resolver software may provide cache replication mechanisms. Otherwise, it is safe to overprovision clusters somewhat under heavy load, and discarding excessive instances once all the caches are populated and the average load of a compute instance decreases.

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

