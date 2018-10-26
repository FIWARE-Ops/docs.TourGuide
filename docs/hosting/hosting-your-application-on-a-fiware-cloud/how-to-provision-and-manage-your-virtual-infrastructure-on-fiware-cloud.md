FIWARE Cloud provides advanced IaaS capabilities based on OpenStack, including
Compute (Nova), Storage (Cinder), Network (Neutron) and Image (Glance) services.
The services are available both via OpenStack APIs and via the FIWARE Cloud
Portal. **OpenStack Compute (Nova)** service can be used to provision Virtual
Machine instances by specifying the virtual machine configuration ‘flavor’ (from
a catalogue of pre-defined flavors configured by the administrator) and virtual
machine image, which comprise a pre-packaged Operating System and a software
stack managed by **OpenStack Image (Glance)** service. FIWARE Lab holds a rich
catalogue of Virtual Machine images, including base Operating Systems (such as
Ubuntu or Red Hat) as well as images comprising pre-packaged installations of
the various Generic Enabler implementations. It is possible to automatically
customize the guest Operating System configuration on the first boot of the
Virtual Machine, as well as to control the power state of the Virtual Machine
(power on/off), suspend/resume, take snapshots, etc. The availability of some of
the functions depend on the capabilities of the underlying virtualization layer.
**OpenStack Storage (Cinder)** service can be used to provision persistent
storage volumes, allocated from one of the storage pools configured by the
administrator. These volumes can be then attached to a Virtual Machine instance,
to hold persistent data of an application. **OpenStack Network (Neutron)**
service is used to provision and manage virtual networks, which can be used to
define connectivity domains and attach Virtual Machine instances to them, as
well as virtual routers, used to control connectivity between virtual networks
and with the Internet.

More details on how to work with the FIWARE Cloud infrastructure via the FIWARE
Cloud Portal can be found at  http://edu.fi-ware.org/mod/scorm/view.php?id=90   
 and https://www.youtube.com/watch?v=mb7CdMnZh7Q
