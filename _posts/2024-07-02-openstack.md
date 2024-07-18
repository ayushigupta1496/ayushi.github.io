---
layout: post
title: 02-07-2024
---

# OPENSTACK

- Openstack - OpenStack is a cloud operating system that controls large pools of compute, storage, and networking resources throughout a datacenter, all managed through a dashboard that gives administrators control while empowering their users to provision resources through a web interface.

- openstack  services consist of several independent part:
- All services authenticate through a common identity service called as Keystone.
- Individual services interact with each through public APIs ,except where privliged administrator commands are necessary
- All services have atleast one API process,which listens for api requests and preprocesses them and passes them to the other part of the services.


**The architecture of OpenStack is modular, consisting of several core components that work together to provide cloud services.**

1.  Compute (Nova):
- Manages and automates the provisioning of virtual machines (VMs).

**Nova components**
- nova-api: Handles API requests.
- nova-scheduler: Selects the compute host where instances will run.
- nova-conductor: Mediates between nova-compute and the database.
- nova-compute: Interacts with hypervisors to manage VM lifecycle.
- nova-consoleauth: Authorizes tokens for console access.

2. Networking (Neutron):
- Provides networking-as-a-service between interface devices managed by other OpenStack services.

**components of neutron**
- neutron-server: Receives API requests.
- neutron-plugin: Performs actual networking and IP management.
- neutron-l3-agent: Manages layer-3 networking (routing).
- neutron-dhcp-agent: Provides DHCP services to tenant networks.
- neutron-metadata-agent: Provides metadata to instances.
- neutron-ovs-agent: Manages Open vSwitch configurations.

3. Storage: There are two type of storage one is block storage(cinder) and other is object storage(swift).

 **Block storage(cinder)** -  Provides persistent block storage to running instances.
-  components of cinder.
- cinder-api: Handles API requests.
- cinder-scheduler: Selects the appropriate storage provider node.
- cinder-volume: Manages block storage devices.


**Object Storage(swift)** - Provides scalable and redundant storage for objects and files.
- OpenStack Object Storage (Swift) is a scalable storage system. Objects and files are stored on multiple disks located on servers in the data center.
- The OpenStack Object Store project, known as Swift, offers cloud storage software so that you can store and retrieve lots of data with a simple API. It's built for scale and optimized for durability, availability, and concurrency across the entire data set. Swift is ideal for storing unstructured data.
-  The OpenStack Swift architecture includes a proxy server and storage nodes. The proxy server implements the Swift API to enable the transmission of read and write requests between clients and the storage servers via the HTTP protocol.Swift places copies of every object in locations that are unique as possible -- first by region, then by zone, server and drive. If a server or hard drive fails, OpenStack Object Storage replicates its content from active nodes to new locations in the cluster.

- components of swift
- swift-proxy-server: Handles incoming API requests.
- swift-account-server: Manages user accounts.
- swift-container-server: Manages containers.
- swift-object-server: Manages storage of objects.


4. Identity (Keystone):
- Provides authentication and authorization for all OpenStack services.

- components of keystone
- keystone api - Handles API requests for token validation and  user management.

5. Image (Glance):
- Manages disk images used by VMs.

- components of glance-
- glance-api: Handles image service API requests.
- glance-registry: Stores and retrieves metadata about images.

6. Dashboard (Horizon):
- Provides a web-based user interface to OpenStack services.

7. Orchestration (Heat):
- A Heat template describes the infrastructure for a cloud application in text files which are readable and writable by humans, and can be managed by version control tools.
- Templates specify the relationships between resources (e.g. this volume is connected to this server). This enables Heat to call out to the OpenStack APIs to create all of your infrastructure in the correct order to completely launch your application.
- The software integrates other components of OpenStack. The templates allow creation of most OpenStack resource types (such as instances, floating ips, volumes, security groups, users, etc), as well as some more advanced functionality such as instance high availability, instance autoscaling, and nested stacks.
- Heat primarily manages infrastructure, but the templates integrate well with software configuration management tools such as Puppet and Ansible.
- components of heat
- heat-engine: It is the core element of Heat architecture and is dedicated to orchestrating the services by launching the templates.
- heat-api: Handles API requests.

  **steps for managing heat service**
- Step 1 First of all, the template is designed which consists of resource description. This is written in a human-readable format. 
- Step 2 Now the stack is created by the user. This is considered to be successfully created when the Heat CLI tool points to the parameters   and the template. 
- Step 3 Now the Heat API and the Heat cli tool communicate with each other. 
- Step 4 After, the communication is done, the Heat API starts to send requests to the Heat engine. 
- Step 5 The requests are finally processed by the Heat engine and the output is sent to the user. 


8. Telemetry (Ceilometer):
- Ceilometer is the Telemetry service component in OpenStack, designed to collect and manage metering and usage data across various OpenStack services. 

- components of  telemetry
- Collector - Collectors are deployed on each node where monitored services are running (e.g., compute nodes for Nova, storage nodes for Cinder/Swift). They retrieve data such as CPU usage, memory usage, disk usage, network usage, and more from these services.

- Agents - Agents gather detailed resource usage metrics specific to hypervisors (e.g., CPU usage of VMs, disk I/O of VMs).


- Data store - Ceilometer supports multiple data stores such as SQL databases (default), NoSQL databases (like MongoDB), and other storage backends. These stores hold aggregated and raw data for further analysis, reporting, and billing purposes.

- API-Server - Provides a  API for querying and retrieving metering data.

- Alarm-Evalution - Ceilometer can evaluate collected metrics against configured alarm rules to detect events of interest. 

- Notification Listener - Listens to OpenStack message queues for notifications.

- Publishers - Publishers in Ceilometer allow integration with external monitoring tools.

- Database - Stores configuration data and state information.



