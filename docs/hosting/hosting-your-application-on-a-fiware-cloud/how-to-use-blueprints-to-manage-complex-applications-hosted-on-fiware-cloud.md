When progressing from small experimental environments to a more extensive use of
the cloud infrastructure, it is often required to automate the provisioning and
life cycle of complex applications comprising multiple resources -- such as
multi-tier applications with several inter-dependent Virtual Machines, each
running a particular software stack and configured to interact with the other
Virtual Machines. FIWARE Cloud provides advanced capabilities to manage
application **blueprints**, comprising one or more application tiers. Each tier
may consist of an elastic group of Virtual Machine instances, with auto-scaling
rules (enforced by the **Policy Manager GE**) that determine when Virtual
Machines will be added or removed to/from the group, typically based on the load
on the corresponding tier (monitored via a dedicated instrumentation provided by
the **Monitoring GE**). Moreover, each tier can define the **software packages**
that should be configured within the guest Operating System as part of the
Virtual Machine instance provisioning. The software packages and their
configuration are managed by the **Software Deployment and Configuration GE**,
which is based on (and compatible with) Opscode Chef -- a widely used open
source software configuration management tool. The combination of these GEs,
surfaced in a holistic manner via the**PaaS Management GE** (and available in
the FIWARE Cloud Portal), provides unique value proposition currently not
available in other OpenStack-based clouds. It is worth noticing that the FIWARE
team is working with the OpenStack community to contribute the necessary
enhancements to make our technology broadly available.

More details on working with blueprints (via the FIWARE Cloud Portal) can be
found at  http://edu.fi-ware.org/course/view.php?id=47 and
https://www.youtube.com/watch?v=MdZS\_0jt6Vo
