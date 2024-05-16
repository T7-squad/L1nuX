- Web Applications are hosted in the cloud in 3 ways :
  - Within virtual machines --> This is called *IaaS (Infrastructure as a Service)* .
  - Within containers --> This is called *PaaS (Platform as a Service)* .
  - Directly on the OS maintained by the cloud provider --> This is called *SaaS (Software as a Service)*.

- The Cloud provider charges you if you use his *block storage*, it means if you uses a large amount of storage, the costs will be higher.
- *Object storage :* That allows web applications to store there files using a HTTP request to the cloud provider.
- Block storage is often referred to as a *persistent volume*, and object storage is often called *Binary Large Object (BLOB)* storage.

# Working with Containers

[[Docker installation]],[[Docker Exec]],[[Docker Restart]].

# Running Containers within the Cloud

- *Kubernetes* is a cluster that provides both orchestration and build automation for containers.
- This cluster can be used to test, deploy, manage the containers.
- A *Kubernetes* Cluster includes a list of virtual machines called *nodes*.
- Each Web-App that you run on each node is called a *pod* and consist of one or more related containers and persistent volumes :
  ![[Pasted image 20240313142355.png]]
![[Pasted image 20240313142530.png]]

