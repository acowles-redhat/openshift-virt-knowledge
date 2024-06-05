# OpenShift Virtualization for VMware vSphere admins: Disaster and site recovery | Red Hat Developer
Nobody likes to think about disasters. Surely that won’t happen to you. Often the mantra is to hope for the best and plan for the worst. In today’s IT world, downtime, no matter how small, can have a disastrous effect on a business’ ability to compete in the marketplace. 

What is disaster recovery?
--------------------------

There are many options to consider when it comes to the concepts of business continuity and disaster recovery. Primarily we find ourselves concerned with two major metrics: the recovery point objective (RPO) and the recovery time objective (RTO). 

The RPO is what amount of data is considered an acceptable loss in the event of a disaster. If you lose data due to an unforeseen event, and your last successful backup and restore point is three days old, is that an acceptable loss of data? 

The RTO is the time it takes to get back on your feet after a disaster. Long ago, when everyday backups of data were performed to tape and had to be restored to the primary storage array before being accessed, there could be a large amount of downtime associated with a recovery effort. If your company’s business had to grind to a halt for 24 hours in order to restore and resume working, is that an acceptable break in activity? 

For most customers, the answer to both of these questions is a resounding "No," and so they seek out solutions that provide RPO targets of zero, and RTO targets of near-zero.

Site Recovery Manager (SRM) and disaster recovery planning
----------------------------------------------------------

In order to achieve these goals, it’s quite crucial to have a disaster recovery plan in place that can meet these objectives. In the world of virtual data centers, this is usually achieved by having a secondary site on standby with a similar configuration of hypervisors, with access to either a shared or synced storage array, and a regular backup schedule that takes virtual machine snapshots from the primary site. In this scenario there is also a mediator system of some type that is able to tell when a virtual data center has become unavailable so that an automated failover can occur. 

If things go to plan, the most recent snapshot is used, and achieves the RPO of zero, and the time to failover and start serving data from the secondary site is minuscule, thus achieving the RTO of near-zero. In the world of VMware vSphere, the solution that achieves this is called Site Recovery Manager (SRM). 

SRM functions by having a set of appliances deployed at both the primary and secondary site which maintain heartbeat connections with each other (see Figure 1). The solution also requires establishing replication relationships between vSphere deployments at each site, and the underlying storage systems at each site, which allows for that quick failover of both VMs and the storage volumes associated with the VMs when an outage occurs.

![Sample architecture of vSphere Site Recovery Manager (SRM).](https://developers.redhat.com/sites/default/files/image1_52.png)

Figure 1: Sample architecture of vSphere Site Recovery Manager (SRM).

Disaster recovery planning with Red Hat OpenShift Virtualization
----------------------------------------------------------------

Surprisingly or not, being prepared for a disaster scenario is one thing that Red Hat has also kept at the front of our minds with [Red Hat OpenShift](https://developers.redhat.com/products/openshift/overview) and our array of storage and data protection partner integrations, as well as tools that we have developed internally for this purpose. 

One such example that is now generally available with [the most recent (4.15) release of Red Hat OpenShift](https://developers.redhat.com/articles/2024/03/19/whats-new-developers-red-hat-openshift-415) is our Metro DR solution, which can now support disaster recovery for both container workloads and virtual machines (VMs) deployed using [Red Hat OpenShift Virtualization](https://www.redhat.com/en/technologies/cloud-computing/openshift/virtualization). The solution consists of two components included in [Red Hat OpenShift Platform Plus](https://www.redhat.com/en/technologies/cloud-computing/openshift/platform-plus): [Red Hat Advanced Cluster Management for Kubernetes](https://www.redhat.com/en/technologies/management/advanced-cluster-management) (RHACM), and [Red Hat OpenShift Data Foundation](https://www.redhat.com/en/technologies/cloud-computing/openshift-data-foundation). You will also need a deployment of [Red Hat Ceph Storage](https://www.redhat.com/en/technologies/storage/ceph). See Figure 2 for a sample architecture for this solution.

![Architecture diagram of Red Hat Advanced Cluster Management and OpenShift Data Foundation Metro DR](https://developers.redhat.com/sites/default/files/image3_31.png)

Figure 2: Sample architecture of Red Hat Advanced Cluster Management and OpenShift Data Foundation Metro DR.

The setup of a disaster recovery and business continuity solution for VMs in OpenShift runs in a similar manner to what a vSphere administrator might already be used to. It still uses a mediator, and there is a replicated or stretched storage system between the primary and secondary sites. In this specific solution, a stretched Red Hat Ceph External Mode cluster is deployed between two sites to allow the data storage providing the persistent volumes that back the VMs to always be available. 

One thing that is necessary to keep in mind about this solution is that for the stretched Ceph cluster to function correctly, there are distance limitations in effect which require 10 milliseconds round trip for communication between the primary and secondary sites. 

Currently there is also a RegionalDR solution for OpenShift container workloads that makes use of two separate Red Hat Ceph clusters with a replicated relationship, with a larger distance allowed between both the OpenShift and storage clusters at both sites. However, it does not currently support virtual machine workloads.

Where Red Hat’s solution for disaster recovery differs from what an SRM administrator is used to is that it does not require the installation of the SRM appliance or configuration of the Storage Replication Adapter at each site, but instead leverages a centralized deployment of RHACM, which is installed on an adjacent OpenShift cluster, referred to as the Hub cluster, to monitor and manage workloads deployed at both the primary and secondary sites. 

The use of RHACM in this solution presents the customer with a single viewpoint from which to manage the deployment of VMs at their primary site and to ensure that those guests are now operational on the secondary site if that becomes necessary. The stretch Red Hat Ceph cluster in the configuration ensures that the data is always available, and OpenShift Data Foundation installed on each OpenShift cluster provides persistent storage for the virtual machines. Both the primary and secondary clusters in this arrangement are available, and VM and container workloads can be deployed independently by users at either site. 

In order for workloads to be protected by the solution, however, they must be deployed using the console of the hub cluster. For this to function, a DRPolicy must be created on the hub cluster (see Figure 3), identifying both the primary and secondary clusters as well as the application or VMs to be protected by that policy.

![Red Hat Advanced Cluster Management DRPolicy wizard tab in the OpenShift console.](https://developers.redhat.com/sites/default/files/image2_28.png)

Figure 3: Red Hat Advanced Cluster Management DRPolicy wizard.

This allows the RHACM to be aware of the virtual machine or container that it is monitoring, so that it can be configured as a protected entity that it knows has been deployed to the primary site. It should be set to restart automatically on the secondary site, if the first cluster were to become unavailable. Once the OpenShift cluster at the primary site becomes available again, a failback operation can be issued from the hub cluster to resume running the VMs and containers from that site.

Conclusion
----------

In this article, we discussed how OpenShift Virtualization can protect mission critical workloads in a Metro DR configuration by automatically restarting virtual machines at a secondary location if the primary OpenShift cluster becomes unavailable. This was done in a similar manner to the feature set provided by VMware Site Recovery Manager. This solution makes use of several of the components included in OpenShift Platform Plus, as well as Red Hat Ceph Storage to protect virtual machines and assist with a return to normal operations very quickly in the event of a disaster scenario. 

To learn more about how to protect your virtual machine workloads in OpenShift Virtualization, consider attending one of our live events, such as [Red Hat Summit Connect](https://www.redhat.com/en/summit/connect), or a session of the [OpenShift Virtualization Roadshow](https://www.redhat.com/en/events/openshift-roadshows/virtualization), where we feature a product overview and hands-on lab dedicated to OpenShift Virtualization that can help familiarize you with this modern container and virtualization platform. 

You can also check out the [Getting started with OpenShift Virtualization](https://cloud.redhat.com/learn/getting-started-red-hat-openshift-virtualization) learning path to get some hands-on experience with OpenShift Virtualization at your own pace.
