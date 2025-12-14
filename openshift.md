# OPENSHIFT

Red Hat OpenShift Container Platform (RHOCP) 

OpenShift is a complete platform to run application in clusters of servers. It is based om existing
technologies such as containers and Kubernetes.

Containers provide a way to package applications anf their dependencies so they can be executed at runtime.

Kubernetes provides many features to build clusters of servers that run containerized applicaions.  
Kubernetes do not intend to provide a complete solution but provide extension points that system administrators
can use. Kubernetes isn't a complete, end-to-end solution but rather a powerful container orchestrator
that provides core building blocks for managing applications at scale, leaving other crucial functions
like CI/CD, advanced monitoring, logging, or service meshes to be added via external tools.

OpenShift builds on there extension points of Kubernetes to provide a complete platform.

## OVERVIEW

### Containers

A container is a process that runs an applicaion indempendednt from other containers on a host. Container
instances are created from aContainer image which includes all dependencies for the applicaion. Containers
can be executed on any host without the need to install dependencies on the host and without conflicts
between different conytainers dependencies.

A container uses the Linux features like namespace and cgroups. Cgroups are used fot resorce management
such as CPU time allocation and system memory. Name spaces isolate containers process and resources 
from other containers and the host system.

The Open Container Initiative (OCI) mantains the standards about containers, container images ann container
run-time. Most container engines are implemented to conform to the OCI specifications, applicaions that
are packaged according to the specifications can run on any conforming platform.

### Kubernetes

Kubernetes is a platform to simplify the deployment, management and scaling of containerized applications.

Kubernetes creates clusters that run applications across multiple nodes. If a node fails Kubernets can
restart the application (container) on a different node.

Kuberents uses **declarative** configuration model, hos it should be. Kuberentes administrators write a definition
of the *workload*, the amount of memory, ... Kubernets then creates the containers on different node to
meet the definition that was set out.

Kuberbetes define  *workload* in terms of pods. Apod in one or more containers that run in the same cluster
node. Pods with multiple containers are useful in certain situations, when two containers must run in
the same cluster to share some resourse. 

#### Kubernetes features

Kuberents offers the following featurs on top of container infrastructure.

- Service discovery and load balancing  
	Distributing appliactions across multiple nodes that can communicate between each other.
	
	Kubernets automatically configure networking and provide DNS service for the pods. This makes it
	possible for Pods to communicate with services for other pods across nodes by using only the hostname
	and not IP address of the pod. Multiple pods can back a service for performance and reliability.
	
- Horizontal scaling  
	Kubernets can monitor the load on a service and create or delete pods to adapt to the load. Even
	additional nodes can be provided.
	
- Self-healing  
	If an healt check is configured  then Kubernetes can monitor, restart and reschedule falling or
	unavailable applications. The self-healing protects appliacions from internal failure (applicaion
	stops) or external failure (the node that runs the container becomes unavailable.
	
- Automated rollout  
	Kuberenetes can rollout updated conaties gradually, if domething goes wrong then the rollout can
	become a rollback to the previous version of the deployment. Kubernetes routes requests to the rollout
	versions of the applicaions and deletes pods from the previos version when the rollout is complete.
	
- Secret and configuration management  
	Manage configurations and secrets for the applicaions without the need to change the container.
	Secrets could be users, passwords, service endpoints or any configuration that needs to be kept
	secret.
	
- Operators  
	Packed Kubernetes applications that manage Kuberentes workloads. Ex: an operator can define a database
	server resources. If a user creates a database resourcethen the operator creates the necessary workloads
	to deploy the database, configure replication and backup automatically.
	
#### Kubernetes Architectural Concepts

Kubernetes uses several servers (nodes) to ensure resilience and scalability of applicaions that it manages.
The nodes can be either physical or virtual machines. 

The nodes are of two types that provide different aspects ti the cluster operations

- Control plane node  
	Provide global cluster coordination for scheduling the deployed workloads. These nodes also manage
	the state of the cluster configuration in response to cluster events and requests.

- Compute plane node
	Runs the workload

A server can avt as both control plan node and compute node the two roles are often seperated for increased
stability, security and managebility. The sise of a Kubernetes cluster can range from single-node cluster
to large cluster with up to 5000 nodes.

#### The Kubernetes API and Configuration Model

Kuberentes provides a model to define the workloads to run on a cluster 

Administrator define workloads in terms of resources.

All resources types work the same way with a uniform API. *Kubectl* command tool is used to manage resources
of all types even custom resource types.

Kubernetes can import or export resources as a text file, working with resouces definitions in text 
format administrators can describe their workloads instead of performing the right sequence of operations
to create them. This is called *declarative resource management*.

Declarative resouce management can reduce the work to create workloads and improve maintainability. 
By using text files to describe resources admninstrators can use tools such as versioning to gain further
benefits (tracking).

To support declarative resouce management Kuberentes uses controllers that tracks the state of the
cluster and perform the steps to keep the cluster in the intended state. Meaning that this proccess
require time to be effective when changes to resources are done. But Kubernetes can automaticall apply
complex changes, example if increasing the RAM requirement for an applicaion and the current node can
supply it, then Kubernetes can mode the applicaion with sufficient RAM. Only when the move is complete
will Kubernetes redirect the traffic to the new instances.

Kubernetes support namespaces. Administrators can create namespaces and most resources and most resources
must be created inside of a namespace. Beside helping orgianize large amounts of resource namespaces provides
the foundation for feature such as resource access. Administrators can define permissions for namespaces,
to allow specific user to view or modify resources. 

---

## OPENSHIFT INTRODUCTION

Kubernetes provides many features to run container workloads on clusters. But Kubernetes only also provides
the building blocks for other features becase differernt environment might need different solutions.
Kuberenets administrators can select an existing solution or add their own solution to fit their specific
need.

OpenShift is a set of modular components and services that are built on top of Kubernetes container
infrastructure. It adds capabilities to the production platform as remote management, multitenancy, 
increased security, monitoring and auditing, application lifecycle management, and self-service interfaces
for developers.

OpenShift uses Kubernetes to build a complete solution by adding the following features to Kubernetes
cluster.

- Integrated developer workflow  
	When running applications on Kubernetes you need to store container images for the application.  
	OpenShift integrates a built-in container registry, Continuous Integration/Continuous Delivery 
	CI/CD) pipelines, and Source-to-Image (S2I), a tool to build artifacts from source repositories
	to container images.

- Observability  
	To get reliability, performance, and availability for applications, additional tools for cluster
	administrations is neededto prevent and solve issues. OpenShift includes monitoring and logging
	services for both the running applicatiosn and the cluster.

- Server management  
	Kubernetes require an OS to run on that needs to be installed, configured and maintained.  
	OpenShift provides the installation and update procedures for many scenarios.
	
	Also hosts in a cluster uses Red Hat Enterprise Linux CoreOS (RHEL CoreOs) as the underlying OS.
	RHEL CoreOS is an immutable OS that is optimized for running comntainerized applications.
	RHEL CoreOS uses the same kernel and packages as RHEL. OpenShift also provides features to manage
	RHEL CoreOS following the Kubernetes configuration model.
	
	OpenShift brings tools and a graphical web console to manage all the different capabilities and 
	an improved security measures.

---

## OPENSHIFT EDITIONS

Beside running OpenShift on your local computer, OpenShift Local, for testing and exploration. Red Hat
also provides an "Developer Sandbox" free for 30 ddays access to a shared OpenShift Cluster. This give
you access to a cluster and support testing and exploration.

For production workloads various editions are available. Public cloud partners like AWS,  Azure, IBM Cloud,
Google Cloud all provide an access to Red Hat OpenShift deployment. These managed deployments offers 
access to clusters on infrastructure that you can rely on.

You can also deploy Red Hat OpenShift cluster on virtual or physical infrastructure either on-premise
or in public cloud. These self-managed offerings are availble in several forms.

Different factors affect the choice of deployment. Managed services assign mor responsibilities to Red Hat
and the cloud provider. With self-managed editions you have greator control and flexability but also
take more responsibility over the service.

For example, with managed services, the Red Hat Site Reliability Engineering teams update the clusters
and remedy update issues (although you still participate in scheduling the updates). On self-managed
editions, you update the clusters and remedy update issues. On the other hand, on self-managed editions
you have complete control over aspects such as authentication, when managed editions might restrict 
some options.

Each managed edition documents the responsibilities for customers, Red Hat, and the cloud provider.

Additionally, to assist in managing clusters, the Red Hat Insights Advisor is available from the Red Hat
Hybrid Cloud Console. The Insights Advisor helps administrators to identify and remediate cluster issues
by analyzing data that the Insights Operator provides. The data from the operator is uploaded to the
Red Hat Hybrid Cloud Console, where you further inspect the recommendations and their impact on the
cluster.

- Red Hat OpenShift Virtualization Engine  
	Provides the virtualization functionality found across all OpenShift editions to deploy, manage,
	and scale virtual machines (VMs) exclusively. This solution offers a focused approach to virtual
	machine management and includes key capabilities for VM support including a dedicated virtualization
	admin console, Red Hat OpenShift GitOps, user workload monitoring, and platform logging.

- Red Hat OpenShift Kubernetes Engine  
	Includes the latest version of the Kubernetes platform with the additional security hardening and
	enterprise stability that Red Hat is famous for delivering. This deployment runs on the Red Hat 
	Enterprise Linux CoreOS immutable container operating system, by using Red Hat OpenShift Virtualization
	for virtual machine management, and provides an administrator console to aid in operational support.

- Red Hat OpenShift Container Platform  
	Builds on the features of the OpenShift Kubernetes Engine to include additional cluster manageability,
	security, stability, and ease of application development for businesses. Additional features of this
	tier include a developer console, as well as log management, cost management, and metering information.
	This offering adds Red Hat OpenShift Serverless (Knative), Red Hat OpenShift Service Mesh (Istio),
	Red Hat OpenShift Pipelines (Tekton), and Red Hat OpenShift GitOps (Argo CD) to the deployment.

- Red Hat OpenShift Platform Plus
	Expands further on the offering to deliver the most valuable and robust available features. This
	offering includes Red Hat Advanced Cluster Management for Kubernetes, Red Hat Advanced Cluster Security
	for Kubernetes, and the Red Hat Quay private registry platform. For the most complete and full-featured
	container experience, Red Hat OpenShift Platform Plus bundles all the necessary tools for a complete
	development and administrative approach to containerized application platform management.

All OpenShift editions use the same code.

---

## OPENSHIFT WEB CONSOLE






















---

OLD!!!

## LIFECYCLE

Lifecycle of applications in Red Hat OpenShift Container Platform.  
The following figure illustrates the basic lifecycle of an application that is deployed in a RHOCP cluster.

![From REDHAT Academy](openshift/pod_lifecycle.svg)

1. Starts with the definition of a pod and the containers that it is composed of, which contain the application.
1. Pods are assigned to a healthy node.
1. Pods run until their containers exit.
1. Pods and their containers are removed from the node.  
	Depending on policy and exit code, RHOCP might remove pods after exiting, or might retain them to 
	enable access to the pod container logs.




