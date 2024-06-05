# OpenShift Virtualization for vSphere admins: Introduction to network configurations | Red Hat Developer
**Note:** This post was originally published on the [Red Hat Blog](https://www.redhat.com/en/blog/openshift-virtualization-for-vsphere-admins-an-introduction-to-network-configurations).

[Red Hat OpenShift Virtualization](https://www.redhat.com/en/technologies/cloud-computing/openshift/virtualization) is Red Hat's solution for companies trending toward modernization by adopting a [containerized](https://developers.redhat.com/topics/containers/) architecture for their applications, but find [virtualization](https://developers.redhat.com/topics/virtualization) remains a necessary part of their data center deployment strategy.

Frankly, some applications are still simply unable to be containerized, and that's okay. OpenShift Virtualization addresses this by providing a more modern paradigm that cohesively manages the need for containers and virtual machines in a unified platform.

One concern we at Red Hat must address is this: in addition to providing feature parity, how do we maintain a customer experience that is similar to that of the products and configurations that the customer is used to so that adoption of the new solution is essentially pain-free?

Once comfortable working within an information technology ecosystem, people generally attempt to adapt familiar principles to new systems. For many system administrators, a common solution for data center virtualization has been VMware vSphere. People working in this environment introduced many design solutions that have been adopted as best practices for the deployment and configuration of virtual machines or virtual data centers. These design decisions often seem like second nature to those who work in this sphere of IT, and they would likely attempt to apply similar ones to other environments they are planning.

This series will explore the differences between how enterprises and their virtualization administrators have interacted with previous hypervisor environments and how they can expect to perform similar actions or configurations in the world of OpenShift Virtualization.

[![alt text](https://developers.redhat.com/sites/default/files/styles/article_floated/public/openshift_virtualization_for_vsphere_admins_an_introduction_to_network_configurations-1.png?itok=x9BH7tYb)](https://developers.redhat.com/sites/default/files/openshift_virtualization_for_vsphere_admins_an_introduction_to_network_configurations-1.png)

Figure 1:

A familiar look at a virtual distributed switch (VDS) with multiple uplinks, VLANs, and vNICs as pictured in VMware vSphere.

Multiple network design
-----------------------

First, focus on the multiple network design, which is popular with other virtualization solutions such as VMware vSphere. Those familiar with the environment know that this is where the ESXi hosts often support a various networks, where each virtual network is dedicated to a different purpose within the virtual data center. This network segregation can be accomplished in several ways, from cabling different physical NICs on the hosts to different switches or networks upstream or by trunking additional VLANs over an uplink and verifying they are tagging appropriately on the vSwitch or Distributed vSwitch within the hypervisor itself to indicate their desired purpose. Most commonly, the networking is at least subdivided into the following networks:

*   A management network for the ESXi hosts and vCenter virtual machine, as well as other third-party management hosts that may be deployed.
*   A host migration network for the ESXi hosts to support the hot migration of guests, known as vMotion, between hosts to provide high availability.
*   A host storage network for the ESXi hosts to access storage resources that provide backing storage or guest-mapped storage for the virtual machines.
*   A virtual machine network that provides network connectivity to the virtual guests themselves.

In addition to the networks listed above, it's common to come across unique networking configurations and purposes depending on the use cases of that particular customers environment. Virtual switching, by its nature, allows for a large number of varying configurations to be easily implemented. It's no surprise that people who are used to defining their networks in a commonly understood way may be surprised when deciding to adopt a solution like OpenShift Virtualization that does things differently.

The following sections look at each of the primary use cases identified above and demonstrate how OpenShift Virtualization addresses these needs.

By default, OpenShift Virtualization is installed with a single internal pod network. Like any other application deployed in a Kubernetes environment, this internal network is used to communicate with the pod itself for the duration of its lifecycle. By using the OpenShift console, the `virtctl` CLI utility, or the `oc` command, you can perform all essential management functions for virtual guests over the internal API network. Essentially, this network in OpenShift Virtualization performs the same actions that an administrator would expect from their internal management network in VMware vSphere.

While all of the networking functions in an OpenShift environment can be handled over the default pod network, like the management functions detailed above for a simple configuration, this is often not desirable. Some tasks, such as virtual guest migrations and storage access, benefit greatly from direct access on dedicated networks. While there are ways to tune the performance of certain actions like guest migrations through Migration Policies so that they limit the amount of bandwidth used, most enterprise users would agree that having dedicated networks for additional functions like guest migrations, storage, and virtual guest access are both desirable and more familiar to those that have operated in a VMware vSphere environment before.

[![alt text](https://developers.redhat.com/sites/default/files/styles/article_floated/public/openshift_virtualization_for_vsphere_admins_an_introduction_to_network_configurations-2.png?itok=wtBW30qH)](https://developers.redhat.com/sites/default/files/openshift_virtualization_for_vsphere_admins_an_introduction_to_network_configurations-2.png)

Figure 2:

Creating a MigrationPolicy to limit the amount of bandwidth available for virtual machine live migrations.

For this purpose, OpenShift uses the Multus CNI plug-in, which allows you to configure additional network interfaces, SR-IOV, or Linux bridge devices that are available to the worker nodes, enabling them to be attached to individual pods as needed.

Before installing Multus, the client must configure the underlying network appropriately. In the case of Linux bridges, this will be performed by using the nmstate operator. For SR-IOV devices, you would use the SR-IOV operator. With the networking environment configured, the administrator can begin to configure a Network Attachment Definition, which instructs OpenShift how to use additional physical interfaces, SR-IOV devices, or Linux bridges that define them.

Next, take a deeper look at the configuration for live migrations. Many deployments of VMware vSphere have a dedicated network defined to support vMotion due to the bandwidth demands of live migration. When a live migration event occurs in an infrastructure without a network dedicated to that purpose, it can negatively affect shared network resources, such as virtual guests. It can also impact the management of the environment itself should a node become unavailable and several guests be forced to migrate simultaneously. In OpenShift Virtualization, there are several options that protect live migration events from adversely affecting the primary networking or performance of the cluster.

The first is to define a migration policy that includes rules, including some that limit the amount of bandwidth available for migrations. It's also possible to define a dedicated network for live migrations by modifying the cluster settings under the OpenShift Virtualization overview settings. In this menu, you can limit the number of live migrations that can take place at any given time, preserving bandwidth. You can also modify the network being used for those live migrations.

[![alt text](https://developers.redhat.com/sites/default/files/styles/article_floated/public/openshift_virtualization_for_vsphere_admins_an_introduction_to_network_configurations.png?itok=OHwIlb5c)](https://developers.redhat.com/sites/default/files/openshift_virtualization_for_vsphere_admins_an_introduction_to_network_configurations.png)

Figure 3:

Configuring the limits for virtual machine live migration, including setting migration limitations and defining a dedicated network for live migration.

To do this, create a network attachment definition for a dedicated network interface on your worker nodes and define a private network that allows for connectivity just between the worker nodes that are configured to support virtualization and select it. This way, when a guest is selected for migration or a host is placed into maintenance, forcing all guests to vacate, the dedicated network for live migrations will be used, preventing additional strain on the primary OpenShift networking infrastructure.

Networking for storage resources will depend on the storage provider being used for the OpenShift cluster. Many storage providers have native CSI-compliant drivers for their storage systems that automatically apply their specified best practices for storage connectivity.

From the OpenShift Virtualization perspective, there are a few suggested best practices for configuring the storage classes to support virtual guests. For example, persistent volumes provisioned for VMs should be in RWX mode so that additional nodes can access the persistent volume should the virtual guest need to be migrated to another node. Like with migration, the worker nodes can be configured to access storage over the default management network or they can have a separate network attached to provide connectivity for NFS or iSCSI-based persistent volumes. In many cases, the management or default pod network in OpenShift will be tasked with initializing API calls to the indicated storage system. The CSI driver will verify that the provisioned volume is mapped correctly over the available storage network.

Finally, consider public access to applications running in the VM or to the virtual machine directly. For users who are not quite ready to containerize their applications, it's possible to treat applications currently running on virtual machines like those that run alongside them in containers. To accomplish this, apply a tag to a virtual machine that identifies the application you wish to provide access to. Using that tag to identify it, create a [Kubernetes](https://developers.redhat.com/topics/kubernetes/) service for the necessary ports that the application on the virtual machine may need. This service can then be exposed by creating an OpenShift route and allowing access to the application directly instead of the virtual guest as a whole over the default OpenShift pod network, exactly like a containerized application would be.

Accessing a virtual guest this way is a transitional step between virtualization and containerization of applications. For those who would like a more traditional approach by having direct access to the guest operating system of a virtual machine, create a Network Attachment Definition and binding to a Linux bridge or SR-IOV device that is available on each worker node that is enabled for hosting virtual guests. This confirms the virtual guest gets an external IP assigned to it and that users can directly connect to the guest using SSH or their remote access protocol of choice.

One caveat to keep in mind when attaching additional networks to virtual guests: the Linux bridge, the available physical interfaces, and the VLAN tags need to be defined identically and available on each worker node in the cluster to support high availability of the guest virtual machines should they need to migrate to a different node in the cluster.

Identifying ways that the network architecture of a deployed OpenShift Virtualization solution can be configured to provide similar performance and behavior to other popular hypervisor environments like VMware vSphere can help customers have an engaging experience working with the product. At Red Hat, we believe that positive experiences increase customer satisfaction and product  adoption, which in turn facilitates application modernization.

To learn more about application modernization and how OpenShift Virtualization can help, consider attending one of our upcoming live events, such as [Red Hat Summit Connect](https://www.redhat.com/en/summit/connect) or the [OpenShift Virtualization Road Show](https://www.redhat.com/en/events/openshift-roadshows/virtualization), where we feature a product overview and hands-on lab dedicated to OpenShift Virtualization.

Read Part 2 in this series: [OpenShift Virtualization for vSphere admins: A change in the traditional storage paradigm](https://developers.redhat.com/articles/2024/05/20/openshift-virtualization-vsphere-admins-change-traditional-storage-paradigm)

_Last updated: May 20, 2024_
