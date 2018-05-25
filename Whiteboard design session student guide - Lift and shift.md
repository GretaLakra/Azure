![](images/HeaderPic.png "Microsoft Cloud Workshops")

# Lift and shift

## Whiteboard design session student guide

## March 2018

Information in this document, including URL and other Internet Web site references, is subject to change without notice. Unless otherwise noted, the example companies, organizations, products, domain names, e-mail addresses, logos, people, places, and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, e-mail address, logo, person, place or event is intended or should be inferred. Complying with all applicable copyright laws is the responsibility of the user. Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.

Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document. Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.

The names of manufacturers, products, or URLs are provided for informational purposes only and Microsoft makes no representations and warranties, either expressed, implied, or statutory, regarding these manufacturers or the use of the products with any Microsoft technologies. The inclusion of a manufacturer or product does not imply endorsement of Microsoft of the manufacturer or product. Links may be provided to third party sites. Such sites are not under the control of Microsoft and Microsoft is not responsible for the contents of any linked site or any link contained in a linked site, or any changes or updates to such sites. Microsoft is not responsible for webcasting or any other form of transmission received from any linked site. Microsoft is providing these links to you only as a convenience, and the inclusion of any link does not imply endorsement of Microsoft of the site or the products contained therein.
© 2018 Microsoft Corporation. All rights reserved.

Microsoft and the trademarks listed at https://www.microsoft.com/en-us/legal/intellectualproperty/Trademarks/Usage/General.aspx are trademarks of the Microsoft group of companies. All other trademarks are property of their respective owners.

# Contents

<!-- TOC -->

- [Lift and shift](#lift-and-shift)
    - [Whiteboard design session student guide](#whiteboard-design-session-student-guide)
    - [March 2018](#march-2018)
- [Contents](#contents)
- [Lift and shift whiteboard design session student guide](#lift-and-shift-whiteboard-design-session-student-guide)
    - [Abstract and learning objectives](#abstract-and-learning-objectives)
    - [Step 1: Review the customer case study](#step-1--review-the-customer-case-study)
        - [Facilitator/subject matter expert (SME) presentation of customer case study](#facilitator-subject-matter-expert-sme-presentation-of-customer-case-study)
        - [Customer situation](#customer-situation)
        - [Customer needs](#customer-needs)
        - [Customer objections](#customer-objections)
        - [Infographic for common scenarios](#infographic-for-common-scenarios)
    - [Step 2: Design a proof of concept solution](#step-2--design-a-proof-of-concept-solution)
    - [Step 3: Present the solution](#step-3--present-the-solution)
    - [Wrap-up](#wrap-up)
    - [Additional references](#additional-references)

<!-- /TOC -->

#  Lift and shift whiteboard design session student guide

## Abstract and learning objectives 

In this workshop, students will help a global publisher architect a
solution to migrate their on-premises procurement system into Azure.
Because of the desire to not change the existing application, this will
involve moving the application and its dependencies onto Azure IaaS VMs.
There are many questions and concerns the customer has, and they will
look to the student to answer these and provide the end-state design and
the high-level steps to get there.

Attendees will be better able to migrate and enable easy deployment for
lift and shift capabilities. In addition, attendees will learn to:

-   Build and deploy complex infrastructure solutions with Azure
    Resource Manager templates

-   Work with Azure Automation Desired State Configuration (DSC) for
    deploying server configurations

-   Scale existing templatized deployments leveraging VM Scale Sets

## Step 1: Review the customer case study 

**Outcome**

Analyze your customer's needs.

### Facilitator/subject matter expert (SME) presentation of customer case study 

Duration: 15 minutes

Directions: With all participants in the session, the facilitator/SME
presents an overview of the customer case study along with technical
tips.

1.  Meet your table participants and trainer.

2.  Read all of the directions for Steps 1--3 in the Student guide.

3.  As a table team, review the following customer case study.

### Customer situation

Lucerne Publishing is one of the largest English-language publishers in
the world. With nearly 200 years of history, Lucerne has published some
of the world's foremost authors, including Nobel Prize, Pulitzer Prize,
National Book Award, Newbery Medal and Caldecott Medal winners. Lucerne
is consistently at the forefront of innovation, using digital technology
to create unique reading and viewing experiences and expand the reach of
its authors and documentary producers.

Lucerne is headquartered in New York City and has publishing groups in
the United States, United Kingdom, Canada, Australia and New Zealand.

Lucerne is starting a three-year project to move most of its data center
footprint to the cloud. "We are convinced that cloud implementations
will give us cost savings and more importantly, deliver operational
flexibility," says Greg Vernon, head of infrastructure and enterprise
operations at Lucerne. "Like every other business, we are under constant
pressure to do more with less. We believe that cloud computing will be
substantially cheaper over time than in-house data centers." Currently,
their workloads are hosted in an Equinix collocated data center near
Lucerne's New York office.

Lucerne has already completed a migration to Office 365. One of the key
steps that is complete was to use the Microsoft Azure Active Directory
(AD) Connect service to synchronize their Windows Server Active
Directory Domain users and groups to Azure AD. This enabled Single
Sign-On (SSO) to Office 365 and full synchronization of Azure AD and
their Local Active Directory.

With the successful migration to Office 365 behind them, Lucerne now
wants to pilot a relatively high-impact workload directly in Azure and
set up the network infrastructure to ensure direct connectivity with
Microsoft.

The pilot migration workload is a third-party procurement system that is
hosted on-premises today that Lucerne would like to migrate to Azure
with a minimal amount of changes. One of Greg's primary concerns with
this workload is around security. Greg wants to validate that only
members of the team who are responsible for a service have access to
that service. For instance, only the networking team should be able to
manage the connectivity to Azure, and only the infrastructure team
members responsible for procurement should be able to manage the
procurement infrastructure. Greg is also interested in other governance
features he can apply to ensure only supported workloads are being used
in their Azure subscription.

**Procurement system**

Jesse Adams is the infrastructure lead responsible for managing the
procurement system today. Per Jesse, they use VMware for their
virtualization managed using vCenter 6.0. The application is currently
deployed on four VMware VMs running Windows Server 2008 R2 with
Microsoft Internet Information Services (IIS) and ASP.NET with the .NET
Framework 3.5. The application install only supports a wizard-based
installation that installs several .NET assemblies that are deployed to
the global assembly cache (GAC). Authentication and authorization to the
application are based on the user's Windows user account and a specific
group in AD the user must belong to. The frontend web servers are load
balanced using an F5 load balancer with cookie affinity enabled, because
the application uses an in-memory session state. The web servers are
currently configured with two vCPUs, six GB of memory, and the VMware
host runs Xeon processors (Sandy Bridge). The hardware is not running at
capacity when measured by CPU or memory.

The application itself is accessed by users on Lucerne's corporate
network via the URL https://procurement. Jesse is concerned about
security to the application and wants to restrict access to only
requests from the on-premises network.

The backend for this solution is hosted on a VMWare VMs using SQL Server
2005 in a failover cluster configuration. Lucerne has checked with the
vendor, and the application is supported with SQL Server 2008, 2012,
2014 and 2016. As part of an earlier planned upgrade, Lucerne has
already acquired licenses for SQL Server 2016 Enterprise Edition with
Software Assurance (SA). The solution makes heavy use of TempDB when
generating ad hoc reports. The database size for this workload is
relatively small at just under 200 GB. SQL Server capitalizes on System
Center Data Protection Manager (DPM) 2012 R2 for regular backups
throughout the day to disk and the backups are then offloaded to tape
and eventually to offsite storage.

The database is deployed on VMs, and both are equipped with eight vCPUs
and 16 GB of memory running on the same VMware host. The hardware is not
running at capacity when measured by CPU or memory. The hardware is due
for a refresh, and as such, this is a prime candidate for migration to
Azure. There are currently no plans for a significant change to the
underlying application code.

**Existing network architecture**

![The existing network architecture has Ethernet bridging the Lucerne New York headquarters and the Equinix New York DC location, which uses an On-premises Network.](images/Whiteboarddesignsessiontrainerguide-Liftandshiftimages/media/image2.png "Existing Network Architecture")

**Existing procurement solution**

![The Existing procurement solution includes vmware, vCenter, SQL Server 2005 cluster, Frontend IIS Servers, and Cookie Affinity.](images/Whiteboarddesignsessiontrainerguide-Liftandshiftimages/media/image3.png "Existing procurement solution")

### Customer needs 

1.  How can we assess our environment for suitability and cost analysis
    before migrating to Azure?

2.  Identify the infrastructure requirements and plan for establishing a
    connection with Azure ExpressRoute that will support Lucerne's
    migrated infrastructure.

3.  Isolate privileges for managing infrastructure in the cloud as well
    as understand what governance controls can be put in place to
    control costs and supported workloads. Ensure that workloads
    deployed to Azure cannot be deleted or changed without authorization
    or by accident.

4.  Document the options for migrating the web tier to Azure keeping in
    mind that the configuration of the application will essentially be
    the same (supporting cookie affinity, etc.).

5.  Create a migration plan for the database tier, and ensure the
    database will always be available even if a VM fails or during
    normal maintenance, such as patching monthly or when service packs
    are applied to the SQL Servers.

6.  Understand how backup for this workload will work after migration
    (for application and compute workloads).

7.  Prior to the migration, the procurement team should be able to
    perform a full test of the application running in Azure to ensure it
    is fully functional.

8.  The migration needs to be completed as quickly as possible, but it
    doesn't have to be immediate. The customer can stand for a 12-hour
    outage window in which the system can be down for this one-time move
    to the Cloud. Lucerne demands there is a way to "fail back" if
    something goes wrong, or they are outside of that 12-hour window.

### Customer objections 

1.  How can we tell how much we will really be spending once we have
    migrated to Azure?

2.  Moving procurement to the cloud seems like a security problem. It
    should only be accessible from people at Lucerne's offices.

3.  We already have licenses for SQL Server. We do not want to pay for
    them again.

4.  Our operations team is new to the cloud and currently uses existing
    technologies like System Center Operations Manager (SCOM). We are
    concerned about the time it takes to learn new technologies to
    monitor and maintain an existing workload.

5.  The sun never sets on Lucerne Publishing. Logistics and procurement
    is one of our most critical applications. Any glitch will cause
    havoc in our ecosystem. The system can be down for only 12 hours
    during this one-time move, but when it comes up, everything needs to
    be perfect.

### Infographic for common scenarios

![This image contains common scenarios for both Azure IaaS and Azure
Resource
Manager.](images/Whiteboarddesignsessiontrainerguide-Liftandshiftimages/media/image4.png "Common scenarios")

![In this second Infographic for common scenarios, Backup and Business
Continuity is on the left, and is made up of: Azure Backup, Azure Site
Recovery, SQL Server Managed Backup, Automation Backup, and Azure
Storage Blob. On the right, Migration tools is made up of: Azure
Migrate, Database Migration Service, and Database
Migration.](images/Whiteboarddesignsessiontrainerguide-Liftandshiftimages/media/image5.png "Infographic for common scenarios")

## Step 2: Design a proof of concept solution

**Outcome**

Design a solution and prepare to present a solution to the target
customer audience in a 15-minute chalk-talk format.

Duration: 60 minutes:

**Business needs**

Directions: With all participants at your table, answer the following
questions, and list the answers on a flip chart.

1.  Who should you present this solution to? Who is your target customer
    audience? Who are the decision makers?

2.  What customer business needs do you need to address with your
    solution?

**Design**

Directions: With all participants at your table, respond to the
following questions on a flip chart.

_Design for limiting access to resources_

1.  What does Lucerne need to do to allow isolated access to different
    components of Azure? Specifically, your design should allow the
    network infrastructure team to manage the virtual network and the
    procurement infrastructure team to manage the procurement
    infrastructure.

_ExpressRoute Integration_

1.  Which peering options and other ExpressRoute features would be
    required?

2.  Can you identify the workflow that Lucerne will need to follow to
    enable ExpressRoute in its environment?

3.  Network Design: The networking team has provided the following
    address space for creating the virtual network: 10.0.1.0/24. The
    on-premises network uses the following address space: 172.16.0.0/16.

4.  By drawing a diagram, what connectivity options and subnets would
    you use for the network design?

5.  What additional security measures can you take to minimize the
    attack surface of the procurement solution at the network level?

6.  How does the design address security and user authentication?

_Migration_

1.  What options are available to assess our environment for suitability
    to migrating to Azure?

2.  Which compute stack would you recommend for the web application?

3.  Which data storage option and pricing tier would you recommend for
    the database?

4.  What migration approach, including tools and steps would you use to
    move the workload to Microsoft Azure?

5.  How does the design address availability?

6.  How does the design perform user authentication?

7.  How is load balancing configured in the migrated workload?

8.  How are all the VMs backed up?

9.  How is the database backed up?

10. How do you remove the dependency on tape for offsite backup?

_Prepare_

Directions: With all participants at your table:

1.  Identify any customer needs that are not addressed with the proposed solution.

2.  Identify the benefits of your solution.

3.  Determine how you will respond to the customer's objections.

Prepare a 15-minute chalk-talk style presentation to the customer.

## Step 3: Present the solution

**Outcome**

Prepare to present a solution to the target customer audience in a
15-minute chalk-talk format.

**Presentation**

Duration: 30 minutes

**Directions**

1.  Pair with another table.

2.  One table is the Microsoft team and the other table is the customer.

3.  The Microsoft team presents their proposed solution to the customer.

4.  The customer makes one of the objections from the list of
    objections.

5.  The Microsoft team responds to the objection.

6.  The customer team gives feedback to the Microsoft team.

7.  Tables switch roles and repeat Steps 2--6.

## Wrap-up 

Duration: 15 minutes

-   Tables reconvene with the larger group to hear a SME share the preferred solution for the case study.

##  Additional references

|    |            |       
|----------|:-------------:|
| Azure Backup documentation | <https://azure.microsoft.com/en-us/documentation/services/backup/> |
| ExpressRoute Routing requirements| <https://azure.microsoft.com/en-us/documentation/articles/expressroute-routing/> |
| ExpressRoute workflows | <https://azure.microsoft.com/en-us/documentation/articles/expressroute-workflows/> |
| Virtual Network documentation | <https://azure.microsoft.com/en-us/documentation/services/virtual-network/> |
| Install AD Replica in Azure | <https://azure.microsoft.com/en-us/documentation/articles/virtual-networks-install-replica-active-directory-domain-controller/> |
| Operations Management Suite | <https://azure.microsoft.com/en-us/updates/announcing-microsoft-operations-management-suite/> |
| Site-to-Site VPN documentation | <https://azure.microsoft.com/en-us/documentation/services/vpn-gateway/> |
| ExpressRoute documentation | <https://azure.microsoft.com/en-us/documentation/services/expressroute/> |
| Application Gateway documentation | <https://azure.microsoft.com/en-us/documentation/services/application-gateway/> |
| Prepare for Azure Site Recovery deployment | <https://azure.microsoft.com/en-us/documentation/articles/site-recovery-best-practices/> |
| Replicate VMware virtual machines and physical servers to Azure with Azure Site Recovery | <https://azure.microsoft.com/en-us/documentation/articles/site-recovery-vmware-to-azure-classic/> |
| Azure Site Recovery Service URLs | <https://azure.microsoft.com/en-us/documentation/articles/site-recovery-best-practices/#service-urls/> |
| Azure Migrate Documentation | <https://docs.microsoft.com/en-us/azure/migrate/> |
| Database Migration Service | <https://azure.microsoft.com/en-us/services/database-migration/> |
| Database Migration Assistant | <<https://docs.microsoft.com/en-us/sql/dma/dma-overview/> |
| StarWind V2V Converter | https://www.starwindsoftware.com/converter/> |