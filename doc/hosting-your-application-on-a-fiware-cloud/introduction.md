All the application components running in a centralized data center can
be provisioned and managed using the FIWARE Cloud capabilities. The
preferred way of consuming these capabilities is via one of the existing
FIWARE Lab nodes, although the reference implementation of the
underlying GEs can be also deployed in your own data center (requiring
corresponding hardware and skills).

The foundation of the FIWARE Cloud is an Infrastructure as a Service
platform based on OpenStack, comprising the Compute (Nova), Storage
(Cinder), Network (Neutron) and Image (Glance)  services.

The easiest way to start using the FIWARE Cloud is to open the [FIWARE
Cloud Portal](https://account.lab.fiware.org/), and create one or more
Virtual Machine instances in one of the FIWARE Lab nodes. The catalogue
of Virtual Machine images includes general purpose operating systems
(such as Ubuntu Server 14.04) as well as pre-packaged virtual appliances
providing capabilities of individual GEs (e.g., Orion Context Broker or
CEP). Moreover, you can allocate persistent storage volumes and attach
them to VM instances to keep the persistent data of your application.
Once you have your virtual machines up and running, you can log into the
individual VMs (typically via SSH), and configure your application.

Once you need to deploy your application in production and/or at scale,
you can leverage the advanced orchestration capabilities provided by
FIWARE Cloud, associated with provisioning and lifecycle management of
application blueprints. Blueprint is a composition of virtual
infrastructure resources, software components deployed in VMs and
inter-relations between them, provisioned and managed in an automated
manner. For example, a blueprint can represent a 3-tier application,
comprising a Web tier, an application tier and a database tier. Each
tier may potentially have a different ‘base’ Virtual Machine image, with
a particular set of software components installed on top of the ‘base’
operating system, selected from a dedicated software configuration
management catalogue (based on Chef standard). It is also possible to
accompany a blueprint tier with a set of auto-scaling rules that would
trigger adding or removing VMs to/from a tier to fit the changing
resource demand of the application (based on ongoing monitoring of
certain metrics).

FIWARE Cloud also provides Object Storage facility (based on OpenStack
Swift), that can be used to store and retrieve ‘blob’ objects and
associated metadata.

Being fully compatible with the OpenStack standard, all the above
capabilities can be also consumed via a RESTful API and via a command
line, although this usage is intended for advanced users and operators.

Also, one of our goals is to contribute OpenStack enhancements developed
in FIWARE back to the OpenStack community, so that they become broadly
available through multiple commercial vendors.

You can find more details on how to use the different FIWARE Cloud
capabilities in the following sub-sections:

-   [How to provision and manage your virtual infrastructure on FIWARE
    Cloud](/hosting-your-application-on-a-fiware-cloud/how-to-provision-and-manage-your-virtual-infrastructure-on-fiware-cloud/)
-   [How to use Object Storage capabilities of FIWARE
    Cloud](/hosting-your-application-on-a-fiware-cloud/how-to-use-object-storage-capabilities-of-fiware-cloud/)
-   [How to use Blueprints to manage complex applications hosted on
    FIWARE
    Cloud](/hosting-your-application-on-a-fiware-cloud/how-to-use-blueprints-to-manage-complex-applications-hosted-on-fiware-cloud/)
-   [How to invoke FIWARE Cloud capabilities via an
    API](/hosting-your-application-on-a-fiware-cloud/how-to-invoke-fiware-cloud-capabilities-via-an-api/)

 
