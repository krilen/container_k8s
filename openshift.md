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

The Red Hat OpenShift Web Console  provides a graphical user interface to do many administative tasks
for managing a cluster. It uses the Kubernetes APIs and OpenShifts extension API. The menus, tasks and
features in the web consoles are always availble by using the CLI. The web console provides an easy access
and managemnet for the complex tasks for administarta a cluster.

Kuberenets provides a web-pased dashboard that is not deployed by default in a cluster. It provides minimal
security permission and accepts only token-based autentication. It also requires a proxy setup that limits
access to the web console for only the system terminal that created it. The OpenShift web console in not
as limited as the Kubernetes web console.

There is no relationship between the web console, the OpenShift webconsole is a seperate tool and operators
can extend the webconsole features and functions to include more menues, views and forms to aid in cluser
administration.

### Accessing OpenShift web console

It can be accessed by a browser, the URL for the web console for your cluster is general configurable
and  you can discovered it by using an 'oc' command from the CLI but first you need to login.

	'oc login -u developer -p developer https://api.ocp4.example.com:6443' -> "Login successful"
	An example of q successful login  into the CLI by using the 'oc' command
	
	'oc whoami --show-console' -> "https://console-openshift-console.apps.ocp4.example.com"
	After you have logged in you can get the URL to the web console.
	
	But you also need to login into the web console with your credentials to get cluster access

#### Perspectives

The OpenShift web console provides the Administator and Developer console perspectives, the sidebar menu
layout will differ between the two perspectives.

The different perspectives provides different menus categories aand pages for the two different roles.

- The Administator perspective focus on cluster configuration, deployemnts and operators for the cluster
	and its running workloads.
	
- The Developer perspective focus on creating an running applications.

Also if the OpenShift Virtualization operator is deployed a Virtulization perspective is available, its
focus are on creating and managing virtual machines.

#### OpenShift layout

https://docs.redhat.com/en/documentation/openshift_container_platform/4.18/html-single/web_console/index

The primary navigation medhod is the sidebar that organizes functions and adaministration innto categories.
After selecting a category it will expand to reveal various areas to provide specific cluster information,
configuration or functionality.

- Home -> Overview page (displayed by default)  
	Which provides a quick glimpse of initial cluster configurations, documentation, and general cluster
	status.

- Home -> Projects  
	List all projects in the cluster that are available to the credentials in use.

- Operators -> OperatorHub  
	Which provides access to the collection of operators that are available for your cluster.
	
	By adding operators you expand featurs and functions for the cluster to provide.
	
	By clicking the link on the Operator Hub page, you can peruse the Developer Catalog. Select any project,
	or use the search filter to find a specific project, to visit the Developer Catalog for that project,
	where shared applications, services, event sources, or source-to-image builders are available.
	
	After finding the preferred additions for a project, a cluster administrator can further customize
	the content that the catalog provides. By adding the necessary features to a project from this approach,
	developers can customize features to provide an ideal application deployment.
	
#### OpenShift key concepts

Some OpenShift, Kubernets and container terminology. To help with the navigation in the OpenShift web
console.

- Pods: The smallest unit of a Kubernetes-managed containerized application. A pod consists of one or
	more containers.
- Deployments: The operational unit that provides granular management of a running application.
- Projects: A Kubernetes namespace with additional annotations that provide multitenancy scoping for applications.
- Routes: Networking configuration to expose your applications and services to resources outside the cluster.
- Operators: Packaged Kubernetes applications that extend cluster functions.

---

## OVERVIEW of NODES, MACHINES and MACHINE CONFIGURATIONS

In Kubernetes a *node* is any single system in the cluster where pods can run. The system can be bare metal,
virtual or cloud instances that are members of the cluster. Nodes runs the serices to communicate within
the cluster and receive control plane requests. When a Pod is deployed an available node is tasked with
satisfying the reqiuest.

*Node* and *machine* are terms often said about the same thing. But In OpenShift the term machine is more
specific it is a resource that describe a cluster node.

A *MachineConfig* resource defines the inital state and any chnages to files, services, OS upgrades and
critical OpenShift services for *kubelet* and *cri-o* services. OpenShift relies on the MCO (Machine
Config Operator) to maintain the OS and configuration of the cluster machines. THe MCO operator is a
cluster-level-operator that makes sure that the correct config of each machine. It also performs 
administative tasks such as system updates. It uses the *MachineConfig* resource to validate and remediate
the state of cluster machines to the intended use, after a change in the *MachineConfig* the MCO makes
sure that the cange is implemented on all affected nodes.

The orchestration of MachineConfig changes through the MCO is prioritized alphabetically by zone, by
using the topology.kubernetes.io/zone node label.


### Identifying Errors from Nodes

Administrators routinely view the logs and connect to the nodes in the cluster by using a terminal.
This technique is necessary to manage a cluster and to remediate issues that arise. 

- Compute -> Nodes  
	To view all of the nodes in the cluster. Click on a specific node and then you can view its logs.
	
	Instead of viewing the log you can choose to get a terminal for the node. From this you can access
	the debug pod and use commands from the host binaries to view the status of the node's services.
	An OpenShift node debug pod is an interface to a container running on thr node.

	It is not recommended to make changes directly on the node from the terminal.  
	Only use the terminal for diagnostic investigation and remediation.  
	By using the terminal you use the same binaries as the cluster itself uses.
	
	Additionally the different tabs on the node shows metrics, events and the nodes YAML definition
	file.


### Accessing Pod Logs

Admins often looks at Pods logs to assess a deployed pods health or to troubleshoot a pods deployment
issues.

- Workloads -> Pods  
	Page to view the list of all pods in the cluster. You are able to filter and order the pods by project
	or other fields.
	
	By clicking on a pod you get its details like pod metric, environmental variables, logs, events
	and its YAML definition.
	
	It is even possible to get a terminal for the pods shell for investigation and remediation.  
	It is not recommended to alter a running pod.  
	To fix a pod update the pod configuration to reflect the chnage and redeploy the pod.


### Red Hat OpenShift Container Platform Metrics and Alerts

In the OpenShift cluster the HTTP service endpoints provide metric data that are collected to provide
information for monitoring cluster and application performance. The metric comes from each service and
are used by client libraries that are provide by Prometheus an open source monitoring an alert toolkit.
Metrics data are availble from the service */metrics* endpoint. The data can be used to monitor to alert 
if something goes wrong. *ServiceMonitor* resource defines how a specific service uses the metric to
define a monitor and alert values. *PodMonitor* can be used the same way but for monitor a pod from its
metrics.

How you define the monitoring alerts can be raised when the metric are polled. It will continuously compair
the gathered metric and create an alert when the success criteria is not longer met. An example could
be a web service monitor polling port 80 and if the port becomes invalid the alert is raised. 

- Observe -> Metrics  
	To visualize gathered metrics by using a Grafana-based data query utility. On this page, users can
	submit queries to build data graphs and dashboards, which administrators can view to gather valuable
	statistics for the cluster and applications.

- Observe -> Alerting  
	To view firing alerts, and filter on the alert severity to view those alerts that need remediation.
	Alerting data is a key component to help administrators to deliver cluster and application accessibility
	and functions.


### Kubernetes Events

Events provide a high-level information when logs provide a high detailed view. Events provides information
about more significant chnages. They are useful in understanding to quickly at a galnce understand the
performance and behavior of the cluster, nodes, project or pods.

Events provide details to understand general performance and to highlight meaningful issues.  
Logs provide a deeper level of detail for remediating specific issues.

- Home -> Events  
	Shows the events for all projects or for a specific project. You can further filter and search events.



## Red Hat OpenShift Container Platform API Explorer

From version 4 OpenShift include the API Explorer feature for users to view the catalog of Kuberentes
resources types that are available within the cluster.

- Home -> API Explorer  
	View and explore the details for resources. Such details include the description, schema, and other
	metadata for the resource. This feature is helpful for all users, and especially for new administrators.

---

## KUBERNETSES and OPENSHIFT CLI INTERFACE

Red Hat OpenShift Container Platform (RHOCP) - OpenShift


### The Kubernetes Command-line Interface

An OpenShift cluster can be managed by the OpenShift web console, 'kubectl' or 'oc' cli. The *kubectl'
is the native CLI toll for Kubernetes and connects to the Kubernetes API. The 'oc' commands are an upgrade
to the 'kubectl' commands and it adds specific OpenShhift features.

With 'oc' you are able to create applications and manage the OpenShift projects from the terminal.
The OpenShift CLI is ideal for

- Working directly with project source code
- Scripting OpenShift Container Platform operations
- Managing projects that are restricted by bandwidth
- When the web console is unavailable
- Working with OpenShift resources, such as routes and projects


#### Kubernetes Command-line Tool

The 'oc' installation also includes an installation of 'kubectl' in CLI.

You can also install the 'kubectl' CLI independently of the 'oc' CLI, the version used must be within
on minor verion of difference of the cluster, a version of 1.30 for the 'kubectl' can communicate with
a cluser of version 1.29, 1.30 or 1.31. It is alwasy recommended to use the latest compatable version
of 'kubectl'.

	'curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"'
	Download 'kubectl' 
	
	'curl -LO "https://dl.k8s.io/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"'
	Downlaod the checksum
	
	'echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check' -> OK / FAILED message
	Verify the checksum, if fail DO NOT continue
	
	'sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl'
	Install the downloaded verified 
	
	'kubectl version --client' -> ...
	Get the version, flag "---client" prints only the cllient version, skip it to also get server version

Instead of downloading and installing it youself you can use the OS distribution to install the 'kubectl'
CLI. Since OpenShift is a RedHat product we will use RedHat Enterprise Linux

	'cat <<EOF | sudo tee /etc/yum.repos.d/kubernetes.repo' -> ...
	Get info if the repo is installad and which version
	
	'sudo yum install -y kubectl'
	Install the 'kubectl' from the repo

To get help using 'kubectl' there is 'kubectl --help' and you can also use help with any option 'kubectl _option_ --help'
to get detailed information about a command.

	'kubectl --help' ->
		kubectl controls the Kubernetes cluster manager.
		...
		
	Get help and a list of general option that can be used.
	
	'kubectl create --help' -> 
		Create a resource from a file or from stdin.
		JSON and YAML formats are accepted.
		...
	
	Get help using 'kubectl' and the "create" option.
	
Kubernetes uses many resource components to support applications. The 'kubectl explain' command provides 
detailed information about the attributes of a given resource.

	'kubectl explain pod' ->
		KIND:     Pod
		VERSION:  v1
		
		DESCRIPTION:
		...

Additional documentation can be found at: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands/ 


### The OpenShift Command-line Interface

The 'oc' commands is the main way of interacting with an OpenShift cluster.

It can be downloaded from the OpenShift web colnsle to make sure that it is complatable with the cluster.
Simply go to "Help -> Command Line tools" (? is the help menu). There are several installations options
for the 'oc' client and different OS.

The basic usage is 'oc option' with the basic options or subcommands. Since the 'oc' CLI is an upgrade
or superset from the 'kubectl' the "--help" flag and "explain" options will work for both CLI. But the 'oc'
will also contain additional commands that are not present in 'kubectl' like 'oc login' or 'oc _project_'.

Users that are familiar with 'kubectl' can use it to manage a OpenShift cluster but some resources are
specific for the 'oc' like projects, routes and image streams.


#### Authentication Methods

Before any interaction with the OpenShift cluster can be done you must authenticate your requests. The
authentication layer identifies the user that is associated with requests to the OpenShift API. After 
authentication, theauthorization layer uses information about the requesting user to determine whether
the request is allowed.

An OpenShift user are allowed to rend request to the API. The user represents an actor that can be granted
permissions in the system by adding roles to the user or the users groups.

Different types of users can exists

- Regular users  
	Most interactive users are of this type, a RHOCP (OpenShift) User object represent a regular user.
- System users  
	Infrastructure uses system users to interact with the API. Some system users are automatically created,
	including the cluster administrator with access to everything.  
	Unauthenticated request use an anonymous system user.
- Service account  
	ServiceAccount objects are accounts that allow applications to access the API without sharing regular
	user credentials. When projects are created service accounts are created automtically. Project administrators
	are able to create additional service account to define access to the contents of each project.

Each user must authenticate to access a cluster After authentication the police sets what the user is
authorized to do. 

Using the 'oc login' command autenticate your request and provides role-based authentication and authorization
that will protect the cluster from unathorized access.

There are several ways of using 'oc login'

**Username and password authentication**

	'oc login -u username api-url'
	Providing your username and the API url you will be prompted for the password.

**Token-based Authentication**

The OpenShift control plane includes a OAuth server where you can use get a "OAuth access tokens" to
use to autenticate. This is the recommended method for production environmetsn, automation and CI/CD
pipelines to skip the password in scripts. 

	'oc login --token=your-token --server=cluster-url'

To get a token go to the web address "https://oauth-openshift.apps.cluster-url/oauth/token/request", click
on "Display token" to display the login command.

Another way is to login to the web console then click "Help -> command line tools" and click on the link
"Copy login command" and it will take you to a login page. After login you click "Display token" and
the infromation will appear.

**Web Authentication**

It is also possible to use the "--web" flag ant then login using a browser. 

	'oc login --web api-url'
	Will open a browser to the OpenShift autentication page where you can login with username and password.
	Then the command will return a token to the command line to complete the login.


#### Managing Resources at the Command Line

After you have been autenticated to the cluster you are able to do things

- 'oc new-project _project_name_'  
	Creates a new project that provides an isolation for your applications. In Kubernetes projects are
	called namespaces, but in OpenShift it also provides additional annotations that provide multitenancy
	scoping for applications.
	
		'oc new-project myapp'
		Create a new project named "myapp"

Since the 'oc' command is a superset for the 'kubectl' command there are commands that works on both
Kubernetes and OpenShift resources.

The following 'oc' commads are compatible with 'kubectl' commands.  
Some command require a user with cluster administrator access.

- 'oc cluster-info'
	Prints the address of the control plane and other services. 

		'oc cluster-info' ->
			Kubernetes control plane is running at https://api.ocp4.example.com:6443
			...

- 'oc api-versions'  
	The structure of cluster resources has a API version that teh command can display. It will print
	the supported API version in the form "group/version".

		'oc api-versions' ->
			admissionregistration.k8s.io/v1
			...

- 'oc get clusteroperator'  
	The cluster operators where some have been created automatically during install.

		'oc get clusteroperator' ->
			NAME                     VERSION    AVAILABLE PROGRESSING DEGRADED SINCE ...
			authentication           4.18.6     True      False       False    18d
			baremetal                4.18.6     True      False       False    18d
			...

- 'oc get ...'  
	Using the command will retrive infromation about resources in the selected project it only gets the
	general information about that specific resource type.
	
		'oc get pod' ->
			NAME                          READY   STATUS    RESTARTS   AGE
			quotes-api-6c9f758574-nk8kd   1/1     Running   0          39m
			quotes-ui-d7d457674-rbkl7     1/1     Running   0          67s
			...
	
	'oc get _resource_type_ _resource_name_'  
	Will get you more info about a specific resourse and it can even be exported with the flag
	"-o yaml" or "-o json"

- 'oc get all'  
	Get all resources in a project wit their summary information.

		'oc get all' ->
			...
			NAME                         READY   STATUS    RESTARTS   AGE
			pod/mysql-6bb757c595-qj5ng   1/1     Running   0          16s

			NAME            TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)              AGE
			service/mysql   ClusterIP   172.30.173.61   <none>        3306/TCP,33060/TCP   2m21s

			NAME                    READY   UP-TO-DATE   AVAILABLE   AGE
			deployment.apps/mysql   1/1     1            1           2m21s
			...

- 'oc describe _resource_type_ _resource_name_'  
	Provides additional information about a specific resource.

		'oc describe pod mysql-6bb757c595-qj5ng' ->
			Name:             mysql-6bb757c595-qj5ng
			Namespace:        mysql-openshift
			Priority:         0
			Service Account:  default
			Node:             master01/192.168.50.10
			Start Time:       Mon, 16 Jun 2025 16:07:00 -0400
			Labels:           db=mysql
			...

- 'oc explain ...'  
	To get info about API resource objects and their fields.  
	Add the --recursive flag to display all fields of a resource without descriptions. 

		'oc explain pods.spec.containers.resources'
			KIND:     Pod
			VERSION:  v1

			FIELD: resources <ResourceRequiremnts>

			DESCRIPTION:
				 Compute Resources required by this container. Cannot be updated. More info:
				 https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/

				 ResourceRequirements describes the compute resource requirements.
			...

- 'oc create ...'  
	Creates a resource in the currect project from a resource definition. The "-f" flaf isused to prvide
	a file as the definition. 'oc get _resource_type_ _resource_name_ -o yaml' is able to create a yaml
	file that can act as the resource definition.
	
		'oc create -f pod.yaml' -> "pod/quotes-pod created"
		An example of usage

- 'oc status'  
	Get a High-level overview of the current project. Shows service, deployments, build configurations, ...

- 'oc delete ...'  
	Delete an exitsing resource, type and name must be specified.
	
		'oc delete pod quotes-ui' -> "pod/quotes-ui deleted"
		An example of usage
	
	Be aware that deleteing a specific resouce might just spawn a new resouce of the same time. It is
	only when a project is deleted that all resources and applications are deleted.

It is possible to issue commands that are directed to another project and nu the current project.
You do this by adding the "--n" or "-namespace" flag

	'oc get pods -n myapp2'
	Get all pods in the project named "myapp2

---

## Inspect Kubernetes Resources Objectives

### Kubernetes and OpenShift Resources

Kubernetes uses the API resource object to represent the intended state of everything in the cluster.
All administrative tasks require creating,viewing and chaning the API resources. 

	'oc api-resources' ->
		NAME                    SHORTNAMES   APIVERSION   NAMESPACED   KIND
		bindings                             v1           true         Binding
		componentstatuses       cs           v1           false        ComponentStatus
		configmaps              cm           v1           true         ConfigMap
		endpoints               ep           v1           true         Endpoints
		...
		daemonsets              ds           apps/v1      true         DaemonSet
		deployments             deploy       apps/v1      true         Deployment
		replicasets             rs           apps/v1      true         ReplicaSet
		statefulsets            sts          apps/v1      true         StatefulSet
		...

	The SHORTNAMES for a object help to shorten the CLI command, 'oc get rs' instead of 'oc get replicasets'
	
	The APIVERSION devide the different objects into API groups, format _api-group_ / _api-version_. For
	_api_group_ object that are blank for Kubernetes core resource objects.

	Many Kuberenets resources exists within a namespace. Kubernetes namespace and OpenShift project 
	are basically thesame. A one-to-one relationship always exists between the namespace an an OpenShift
	project.

	The KIND is the formal Kubernetes resource schema type.

	The 'oc api-resources' command can be extended with different flags
	
		- "--namespaced=true": specify the namespace (or "--namespaced=false"
		- "--api-group appsapps": Limit the resouces in a spcific API group, for core resource use "--api-group"
		- "--sort-by name": If not empty sort listed resources by the field, "name" or "kind".

	'oc api-resources --namespaced=true --api-group apps --sort-by name' ->
		NAME                  SHORTNAMES   APIVERSION   NAMESPACED   KIND
		controllerrevisions                apps/v1      true         ControllerRevision
		daemonsets            ds           apps/v1      true         DaemonSet
		deployments           deploy       apps/v1      true         Deployment
		replicasets           rs           apps/v1      true         ReplicaSet
		statefulsets          sts          apps/v1      true         StatefulSet
		
	'oc api-resources --api-group "" --namespaced=false' ->
		NAME                  SHORTNAMES   APIVERSION   NAMESPACED   KIND
		componentstatues      cs           v1           false        ComponentStatus
		namespace             ns           v1           false        Namespace
		nodes                 no           v1           false        Node
		persistentvolumes     pv           v1           false        PersistentVolume
		
	Only the core Kubernetes resources (only v1) that have no namespace


Each resource contains fields that identify the resource or describe the intended configuration of the
resource. The 'oc explain' command give infotmation about valid fields for an object.

	'oc explain pod' ->
		KIND:     Pod
		VERSION:  v1

		DESCRIPTION:
		 Pod is a collection of containers that can run on a host. This resource is
		 created by clients and scheduled onto hosts.
		...
		
	You get the possible pod object fields.

Every Kubernetes resource container *kind*, *apiVersion*, *spec* and *status* fileds. But when creating
an object definition you can leave out the *statu*s field, Kuberenetes generats the *status* field and add 
runtime status and readiness. The *status* is useful for troubleshooting an error or verify the current
state of the resource.

	'op explain pod.spec' ->
		KIND:     Pod
		VERSION:  v1

		FIELD: spec <PodSpec>

		DESCRIPTION:
		Specification of the desired behavior of the pod. More info:
		...
		
	By using the dot notation you can infomration about a resurce field. In the above case spec field
	for the pod resource.  

The following Kubernets main resource types that can be created by using YAML or JSON manifest file,
or by using OpenShift manamement tools

- Pods (pod)  
	Represent a collection of containers that share resources, such as IP addresses and persistent storage
	volumes. It is the primary unit of work for Kubernetes.

- Services (svc)  
	Define a single IP/port combination that provides access to a pool of pods. By default, services
	connect clients to pods in a round-robin fashion.

- ReplicaSet (rs)  
	Ensure that a specified number of pod replicas are running at any given time.

- Persistent Volumes (pv)  
	Define storage areas for Kubernetes pods to use.

- Persistent Volume Claims (pvc)  
	Represent a request for storage by a pod. PVCs link a PV to a pod so that its containers can use
	the provisioned storage, usually by mounting the storage into the container's file system.

- ConfigMaps (cm) and Secrets  
	Contain a set of keys and values that other resources can use. Configuration maps and secrets centralize
	configuration values for several resources. Secrets differ from configuration maps in that the values
	of Secrets are always encoded (not encrypted), and their access is restricted to authorized users.

- Deployment (deploy)  
	A deployment represents a set of containers that are included in a pod, and the deployment strategies
	to use. A deployment object contains the configuration to apply to all containers of each pod replica,
	such as the base image, tags, storage definitions, and the commands to execute when the containers
	start. Although Kubernetes replicas can be created stand-alone in OpenShift, they are usually created
	by higher-level resources such as deployment controllers.

Red Hat OpenShift Container Platform (RHOCP) adds the following main resource types to Kubernetes:

- Build (build)  
	A build is the generic process of taking an input source, such as the source code of an application,
	and producing a usable resource as the output, such as a runnable image.

- BuildConfig (bc)  
	A BuildConfig object defines a build process. A BuildConfig object requires at least the source input
	of the build, the strategy that defines how to build the input, and where to store the output of
	the process.

- Routes  
	Represent a DNS hostname that the OpenShift router recognizes as an ingress point for applications
	and microservices.


#### Structure of Resources

Almost every Kubernets object includes 2 nested object fields, the *spec* and *status*. The *spec* object
describs the intended state of the resource and *status* object describes the current state.  
You specify the *spec* section of the resoure whe  the object is created. Kubernetes continuously updates
the *status* section solong as the object exists. The Kubernetes control plane continuously and actively
manages every objects actual state to get it to the desiered state.

The *status* section has a collection od condition resource object fields

| Field              | Example                    | Description |
|---                 |---                         | ---         |
| Type	             | ContainerReady             | The type of the condition |
| Status             | False                      | The sate of the condition |
| Reason             | RequirementsNotMet	      | An optional field to provide extra information |
| Message	         | 2/3 containers are running | An optional textual description for the condition |
| LastTransitionTime | 2023-03-07T18:05:28Z       | The last time that conditions were changed |

As an example, in Kubernetes, a D*Deployment* object can represent an application that is running in the
cluster. When you create in you might want 3 replicas of the application running. Kuberenetes reads the
deployment *spec* and starts 3 instances of the application and updated the *status* fields to reflect
the *spec* object. If something fails Kubernetes will responde to the difference betweeen the *spec* and
*status* object to make the correction, in this case start a replacement instance.

Other common fields that provide information in the *spec* and *status*.

| Field              | Description |
|---                 |---|
| apiVersion	     | Identifier of the object schema version. |
| kind               | Schema identifier. |
| metadata.name	     | Creates a label with a name key that other resources in Kubernetes can use to find it. |
| metadata.namespace | The namespace, or the RHOCP project where the resource is. |
| metadata.labels    | Key-value pairs that can connect identifying metadata with Kubernetes objects. |

In Kubernetes resources consists of multiple objects. Theses objects define the intended state of the
object. When creating or modifying an object you make a change in the state, Kubernetes reads the object
and modifies the current state to match.

All Kuberenets and OpenShift objects can be represented as a JSON or YAML structure.

	YAML format
	-----
	apiVersion: v1 #1
	kind: Pod  #2
	metadata: #3
	  name: wildfly  #4
	  namespace: my_app #5
	  labels:
		name: wildfly  #6
	spec: #7
	  containers:
		- resources:
			limits:
			  cpu: 0.5
		  image: quay.io/example/todojee:v1 #8
		  name: wildfly #9
		  ports:
			- containerPort: 8080 #10
			  name: wildfly
		  env:  #11
			- name: MYSQL_DATABASE
			  value: items
			- name: MYSQL_USER
			  value: user1
			- name: MYSQL_PASSWORD
			  value: mypa55
	...
	status: #12
	  conditions:
	  - lastProbeTime: null
		lastTransitionTime: "2023-08-19T12:59:22Z"
		status: "True"
		type: PodScheduled
	-----
	
	Explaination to the YAML above
	#1: Identifier of the object schema version.
	#2: Schema identifier. In this example, the object conforms to the pod schema.
	#3: Metadata for a given resource, such as annotations, labels, name, and namespace.
	#4: A unique name for a pod in Kubernetes that enables administrators to run commands on it.
	#5: The namespace, or the RHOCP project that the resource resides in.
	#6: Creates a label with a name key that other resources in Kubernetes, usually a service, can
		use to find it.
	#7: Defines the pod object configuration, or the intended state of the resource.
	#8: Defines the container image name.
	#9: Name of the container inside a pod. Container names are important for oc commands when a pod
		contains multiple containers.
	#10: A container-dependent attribute to identify the port that the container uses.
	#11: Defines a collection of environment variables.
	#12: Current state of the object. Kubernetes provides this field, which lists information such
		as runtime status, readiness, and container images.
	
Labels are key value pairs that are deined in the *.metadata.labels* object path

	kind: Pod
	apiVersion: v1
	metadata:
	  name: example-pod
	  labels:
		app: example-pod
		group: developers
	...

	The two labels that exists in the above example is "app = example-pod" and "group = developers".
	Labels are often used to target a a set of objects by using the "-l" or "--selector" flag

	'oc get pod --selector group=developers' -> ...
	Get the pods that have a specidif label


### Command Outputs

Both the 'oc' and 'kubectl' commands have many output formatting options. By default only a small subsets
of useful fields for the resource in a tabular output is given. By using the flag value "wide" more fields
are given when using the "-o" flag

| 'oc get pods' | 'oc get pods -o wide | Example value |
|---            |---                   |---|
| NAME	        | NAME	               | example-pod |
| READY	        | READY	               | 1/1 |
| STATUS	    | STATUS	           | Running |
| RESTARTS	    | RESTARTS	           | 5 |
| AGE	        | AGE	               | 11d |
|  	            | IP	               | 10.8.0.60 |
|  	            | NODE	               | master01 |
|  	            | NOMINATED NODE	   | $\lt$none$\gt$ |
|  	            | READINESS GATES	   | $\lt$none$\gt$ |

To view all fields that are associated to a resource use the "describe" option. A single object by name,
or all objects of a type, or provide a name prefix, or a label selector.

The 'describe' option provides a human readable output but this format of output might chnage between
version and therefore not recommended to be used in scripts.

For scripts the output format should be YAML or JSON that are suitable for parsing.


#### YAML output

The "-o yaml" provides a YAML formatted output that can be parsed but still human readable.

	'oc get pods -o yaml' ->
		apiVersion: v1
		items:
		- apiVersion: v1
		  kind: Pod
		  metadata:
			annotations
		...
		
Ther are tools like 'yq' thah can process a YAML output. the 'yq' tool uses dot notaion to seperate
fields. https://mikefarah.gitbook.io/yq/

	'oc get pods -o yaml | yq ".items[0].status.podIP"' -> "10.8.0.60"
	The [0] in the example above specifies the first index in an item array


#### JSON output

Internally JSON is used in Kubernetes to process resource objects. Use "-o json" to get the output 
of a resouce in JSON format

	'oc get pods -o json' ->
		{
			"apiVersion": "v1",
			"items": [
				{
					"apiVersion": "v1",
					"kind": "Pod",
					"metadata": {
						"annotations": {}
				}
			]
		...
		}

Also here there are tools to help process the JSON output such as the 'jq' tool (https://jqlang.github.io/jq/)

	'get pods -o json | jq ".items[0].status.podIP"' -> "10.8.0.60"


#### Custom output

Kubernetes provides a custom output format that combines the convenience of extracting data via 'jq'
styled queries with a tabular output format. Use the -o custom-columns option with comma-separated
<column name> : <jq query string> pairs.

	'oc get pods \
	-o custom-columns=PodName:".metadata.name",\
	ContainerName:"spec.containers[].name",\
	Phase:"status.phase",\
	IP:"status.podIP",\
	Ports:"spec.containers[].ports[].containerPort"' -> 
		PodName                  ContainerName   Phase     IP          Ports
		myapp-77fb5cd997-xplhz   myapp           Running   10.8.0.60   <none>

Kubernetes also supports JSONPath expressions. It is a query langauge for JSON. JSONPath expressions
refer to a JSON data structure, they filter and extract formatted fileds from a JSON object

	'oc get pods -o jsonpath='{range .items[]}{"Pod Name: "}{.metadata.name}
	{"IP: "}{.status.podIP}
	{"Ports: "}{.spec.containers[].ports[].containerPort}{"\n"}{end}'' ->
		Pod Name: myapp-77fb5cd997-xplhz
		IP: 10.8.0.60
		Ports:
		
	The JSONPath expression uses the range operator to iterate over the list of pods to extract the
	name of the pod, its IP address, and the assigned ports.

You can customize the format of the output with Go templates, which the Go programming language uses.
Use the -o go-template option followed by a Go template, where Go expressions are inside double braces,
{{ }}.

	'oc get pods -o go-template='{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}'' ->
		myapp-77fb5cd997-xplhz

---

## ASSESS the HEALTH of an OPENSHIFT CLUSTER

### Query Operator Conditions

Operators are important components of Red Hat OpenShift Container Platform (RHOCP). Operators automate
the required tasks to maintain a healty cluster instead of humans manually doing it. Operators is the
method used to packaging, deploying and managing services on the control plane.

Operators integrates with the Kubernetes API and CLI tools. They provide the means to monitor applications,
perform health checks, managing over-the-air (OTA) updates and ensure that appliaction remian in the
specific state.

CIO-O and kubelet run on every node, almost every cluster function can be managed on the control plane
by using Operators Components added to the control plane are done so by Operators, including network
and credential service.

Operators in RHOCP are managed by two different systems, depending on the purpose of the operator.

#### Cluster Version Operator (CVO)

Cluster Operators perform cluster functions, theay are installed by defulat and the CVO manages them.

Cluster Operators use a Kubernetes kind value of *clusteroperators* and can be queried by both 'oc' 
and 'kubectl'. A user with *cluster-admin* role use the *'oc get clusteroperators'* to list the all
the cluster Operators.

	'oc get clusteroperators' ->
		NAME                        VERSION   AVAILABLE   PROGRESSING   DEGRADED   SINCE   MESSAGE
		authentication              4.18.6    True        False         False      3d1h
		baremetal                   4.18.6    True        False         False      38d
		cloud-controller-manager    4.18.6    True        False         False      38d
		cloud-credential            4.18.6    True        False         False      38d
		...

To get the current staus of the Operatora and more infomation about the operaror use the *describe*
option. You can also output the infomation in a specific format using the "-o" flag.

#### Operator Lifecycle Manager (OLM) Operators

This is an optional add-on operator that can be accessable for users to run in their applications.

As a cluster-admin role get the a list of all add-on operators

	'oc get operators' ->
		NAME                                 AGE
		lvms-operator.openshift-storage      44d
		metallb-operator.metallb-system      44d

To get more info use *describe* and *get* command

Operators use one or more pods to provide cluster services. ou can find the namespaces for these pods
under the relatedObjects section of the detailed output for the operator. As a user with a cluster-admin
role, use the -n namespace option on the get pod command to view the pods. For example, use the following
get pods command to retrieve the list of pods in the openshift-dns-operator namespace.

	'oc get pods -n openshift-dns-operator' ->
		NAME                            READY   STATUS    RESTARTS   AGE
		dns-operator-74bb989655-vgm7t   2/2     Running   18         56d

Use the "-o yaml" or "-o json" output formats to view or analyze more details about the pods. The resource
conditions, which are found in the status for the resource, track the current state of the resource object.
The following example uses the jq processor to extract the status values from the JSON output details
for the dns pod.

	'oc get pod -n openshift-dns-operator dns-operator-74bb989655-vgm7t -o json | jq .status' ->
		{
		  "conditions": [
			{
			  "lastProbeTime": null,
			  "lastTransitionTime": "2025-07-14T16:04:38Z",
			  "status": "True",
			  "type": "PodReadyToStartContainers"
			},
		...

In addition to listing the pods of a namespace, you can also use the --show-labels option of the get
command to print the labels used by the pods. The following example retrieves the pods and their labels
in the openshift-etcd namespace.

	'oc get pods -n openshift-etcd --show-labels' ->
		NAME                   READY   STATUS      RESTARTS   AGE   LABELS
		etcd-master01          5/5     Running     50         56d   app=etcd,etcd=true,k8s-app=etcd,revision=2
		installer-1-master01   0/1     Completed   0          56d   app=installer
		installer-2-master01   0/1     Completed   0          56d   app=installer


### Examining Cluster Metrics

Another way of see the health of a cluster is to examin the compure resource usage of cluster nodes and
pods. The 'oc admin ...' command provides this infomration.

	'oc adm top pods -A --sum' ->
		NAMESPACE                 NAME                      CPU(cores)   MEMORY(bytes)
		metallb-system            controller-...-fxhh4      2m           43Mi
		metallb-system            metallb-...-t9pgn         1m           19Mi
		...output omitted...
		openshift-storage         topolvm-node-f9rpf        7m           47Mi
		openshift-storage         vg-manager-q6zhf          1m           21Mi
				                                           ------       --------
				                                            3121m       9123Mi
	
	Getting the the CPU and memory usage for all pods in the cluster.
	
	The "--sum" flag prints the summary of the resources used.
	
	The "-A" flag shows all in every namespace. To filter use the "-n" flaf for specific namespaces
	
	'oc adm top pods etcd-master01 -n openshift-etcd --containers' ->
		POD             NAME           CPU(cores)   MEMORY(bytes)
		etcd-master01   etcd           72m          334Mi
		etcd-master01   etcd-metrics   8m           31Mi
		etcd-master01   etcd-readyz    3m           46Mi
		etcd-master01   etcd-rev       1m           33Mi
		etcd-master01   etcdctl        0m           0Mi
	
	By using the flag "--container" you get the information about each container inside each pod.
	In the above output you see evevery container in each pod in the namespace "openshift-etcd".
	


#### Viewing Cluster Metrics

In the webconsole have graphs to visualize cluser and resource analytics. Cluser administrator or users
can either view or cluster-monitoring-view cluster roles. Access by "Home -> Overview" page and it provides
collection of cluser-wide metrics and provide oa high-level vire of the health of the cluster.

The Overview page has the following metrics.

- Current cluster capacity based on CPU, memory, storage, and network usage
- A time-series graph of total CPU, memory, and disk usage
- The ability to display the top consumers of CPU, memory, and storage


#### Viewing Project Metrics

The "Project Details" page displays metric that provides an overview of the resources that are used within
the scope of a specific project.

The "Utilization" section displays useful information about resources, such as CPU and memory.

All metrics are pulled from Prometheus. Clicking on a graph to to to the "Metrics" page to allow you
to inspect it further.


#### Viewing Resource Metrics

When troubleshooting it is useful to view the metric for a resource instead for the whole cluster. You can
use the "Metric" tab on for a Pod to get information such as CPU, memory, and file-system usage.
A sudden change in these critical metrics, such as a CPU spike caused by high load, is visible on this page.


#### Performing Prometheus Queries in the Web Console

The Prometheus UI is a feature-rich tool for visualizing metrics and configuring alerts. The OpenShift
web console providea an interface for executing Prometheus queries directly from the web console.

To perform a query, go to Observe → Metrics, enter a Prometheus Query Language expression in the text
field, and click Run Queries. The results of the query are displayed as a time-series graph.

### Query Cluster Events and Alerts

Instead of Openshift logs that can be hard to read you have OpenShift events that provide a high-level
logging and auditing. Kubernetes also provides event object in reponse to state changes in the cluster
object such as nodes, pods, and containers. Events signal significant actions, such as starting a container
or destroying a pod.

	'oc get events -n openshift-kube-controller-manager' ->
		LAST SEEN  TYPE    REASON             OBJECT                                MESSAGE
		75d        Normal  LeaderElection     lease/cert-recovery-controller-lock   master01_...
		...
		
	Getting the events for the namespace "openshift-kube-controller-manager".  You could also use 
	the "-A" to get all events for all namespaces but it will be hard to read.

After you get the events you could use describe to get more detailed infomation about a resource.


#### Kubernetes Alerts

OpenShift includes a monitoring stack based on Prometheus open source project. It is monitoring the core
cluster componenets by default. You can optionally configure it to monitor user projects.

The components of the monitoring stack are installed in the openshift-monitoring namespace.

	'oc get all -n openshift-monitoring' ->
		NAME                                              READY  STATUS   RESTARTS AGE
		pod/alertmanager-main-0                           6/6    Running  48       75d
		pod/cluster-monitoring-operator-55b4dbf86-mb4xd   1/1    Running  8        75d
		pod/kube-state-metrics-75455b796c-8q28d           3/3    Running  27       75d
		pod/prometheus-k8s-0                              6/6    Running  48       75d
		...

The Prometheus Operator in the openshift-monitoring namespace creates, configures, and manages the Prometheus
and Alertmanager instances. The "prometheus-k8s-0" entry is the Prometheus pod. 
 
 	'oc -n openshift-monitoring exec -c prometheus prometheus-k8s-0 \
 	-- curl -s  "http://localhost:9090/api/v1/alerts" | jq' -> ...
 	
	A cluster administrator can use the following command to get the alerts from the Prometheus API.

An Alertmanager pod in the openshift-monitoring namespace receives alerts from Prometheus. Alertmanager
uses alert receivers to send alerts to external notification systems. The "alertmanager-main-0" pod is
the Alertmanager for the cluster. 

	' oc -n openshift-monitoring exec -c alertmanager alertmanager-main-0 \
	-- curl -s  "http://localhost:9093/api/v2/alerts" | jq' -> ...
	
	Use the curl command to retrieve the fired alerts from the Alertmanager API.


### Check Node Status

An OpenShift cluseters have several components at least one control plane and at least one compute node.
The two componenets can be on a signle node.

	'oc cluster-info' -> ...
	
	Verify hight-level that the cluster nodes are running.
	
	'oc get nodes' ->
		NAME       STATUS   ROLES                         AGE   VERSION
		master01   Ready    control-plane,master,worker   75d   v1.31.6

	To get a more detailed view of the nodes. The above output shows that the single"master01" node
	is running multiple componenets. The "Status: Ready" means that the node is healty and can accept new
	pods. "NotReady" means that the node does not accepts new pods.

	As usual you can use "describe" to get more information and format the results with "-o" flag.
	
	'oc get et node master01 -o jsonpath= *'{"Allocatable:\n"}{.status.allocatable}{"\n\n"} \ 
	{"Capacity:\n"}{.status.capacity}{"\n"}'' ->
	
	Allocatable:
	{"cpu":"5500m","ephemeral-storage":"75691125429","hugepages-1Gi":"0",
	"hugepages-2Mi":"0","memory":"15225452Ki","pods":"250"}
	
	Capacity:
	{"cpu":"6","ephemeral-storage":"83295212Ki","hugepages-1Gi":"0",
	"hugepages-2Mi":"0","memory":"16376428Ki","pods":"250"}
	
	Using "-jsonpath" flag and the "jq" to are the output.

The JSONPath expression in the previous command extracts the allocatable and capacity measures  for the
master01 node. These measures help to understand the available resources on a node.

View the status object of a node to understand the current health of the node.

	'oc get node master01 -o json | jq ".status.conditions"' ->
		[
		  {
			"lastHeartbeatTime": "2025-08-05T15:32:36Z",
			"lastTransitionTime": "2025-05-22T12:12:24Z",
			"message": "kubelet has sufficient memory available",
			"reason": "KubeletHasSufficientMemory",
			"status": "False",
			"type": "MemoryPressure" #1
		  },
		  {
			"lastHeartbeatTime": "2025-08-05T15:32:36Z",
			"lastTransitionTime": "2025-05-22T12:12:24Z",
			"message": "kubelet has no disk pressure",
			"reason": "KubeletHasNoDiskPressure",
			"status": "False",
			"type": "DiskPressure" #2
		  },
		  {
			"lastHeartbeatTime": "2025-08-05T15:32:36Z",
			"lastTransitionTime": "2025-05-22T12:12:24Z",
			"message": "kubelet has sufficient PID available",
			"reason": "KubeletHasSufficientPID",
			"status": "False",
			"type": "PIDPressure" #3
		  },
		  {
			"lastHeartbeatTime": "2025-08-05T15:32:36Z",
			"lastTransitionTime": "2025-08-05T14:41:42Z",
			"message": "kubelet is posting ready status",
			"reason": "KubeletReady",
			"status": "True",
			"type": "Ready" #4
		  }
		]

	Explain the putput
	#1: If the status of the MemoryPressure condition is true, then the node is low on memory.
	#2: If the status of the DiskPressure condition is true, then the disk capacity of the node is low.
	#3: If the status of the PIDPressure condition is true, then too many processes are running on the node.
	#4: If the status of the Ready condition is false, then the node is not healthy and is not accepting pods.

More conditions indicate other potential problems with a node.

| Condition          | Description |
|---                 |---          |
| OutOfDisk	         | If true, then the node has insufficient free space on the node for adding new pods. |
| NetworkUnavailable | If true, then the network for the node is not correctly configured. |
| NotReady           | If true, then one of the underlying components, such as the container runtime or network, is experiencing issues or is not yet configured. |
| SchedulingDisabled | Pods cannot be scheduled for placement on the node. |

To get deeper inside into a node you could look att it logs. A cluster administator can use the 'oc adm node-logs'
command to view the logs. Node logs can conatin sensitive info that is why they are limited to node administartor.

For a single node you can use 'oc adm node-logs _node_name_'.

The logs can be filtered, use 'oc adm node-logs --help' for a complete list of command options.

| Option Example  | Description |
|---              |--- |
| "--role master" | Use the --role option to filter the output to nodes with a specified role. |
| "-u kubelet"    | The -u option filters the output to a specified unit. |
| "--path=cron"   | The --path option filters the output to a specific process under the /var/logs directory. |
| "--tail 1"      | Use --tail x to limit output to the last x log entries. |

	'oc adm node-logs master01 -u crio --tail 1' -> ...
	Getting the most recent log entry for the "cri-o" service on bide "master01".'

When a pod is created with the cli  the 'oc' or 'kubectl' command is sent to the API service which
validates the command. The *schedulare* service reads the definition and assigns pods to compute nodes.
Each compute node runs a *kubelet* service that converts the pod manifest to one or more containers in
the CRI-O runtime.

Each compute node must have an active kubelet service and an active crio service.

	'oc debug node/node-name' (Replace the node-name value with the name of your node.)
	Start a debug session on the node by using the debug command.
	
	chroot /host
	Within the debug session, change to the /host root directory so that you can run binaries in the
	host's executable path.
	
	'for SERVICES in kubelet crio; do echo ---- $SERVICES ---- ;
	systemctl is-active $SERVICES ;  echo ""; done' -> 
	
		---- kubelet ----
		active
		
		---- crio ----
		active
	
	Using the systemctl is-active calls to confirm that the services are active.
	
	'systemctl status kubelet' -> ...
	For more details about the status of a service, use the systemctl status command.

#### Check Pod Status

You are able to see logs in running containers and pods for troubleshooting.When the container start
its standard out and standard error are redirected to the container disk. By using the *logs* command
you can see the containers logs even if it has stopped but the pod must still exists.

	'oc logs pod-name -c container-name'
	See a containers log in a pod, replace pod-name with the name of the target pod, and replace container-name
	with the name of the target container.
	
	The -c container-name argument is optional, if the pod has only one container. You must use the
	"-c container-name" argument to connect to a specific container in a multicontainer pod. Otherwise,
	the command defaults to the only running container and returns the output.

When debugging images and setup issues it is useful to get an exact copy of the running pods configuration
and then troubleshoot it with a shell. If the pod is failing or does not include a shell the 'exec' option
might not work. To reolve this use the *debug* command to create a copy of the specific pod and start a
shell in that.

By defualt the *debug* command start the shell inside the first container of the refrenced pod. The debug
pod is a copy of the source pod with som extra modifications. Examples of modifications, all labels are
removed, the default shell is either '/bin/sh' for linux pods or 'cmd.exe'. for Windows containes. Also
the readiness and liveness probe is disabled.

A normal problem for containers in Pod is security policies that prohibits a container from running as
root user. You can use the *Debug* command to test running the pod as a nin-root user by using the 
"--as-user" flag. You could also run a non-root pod as the root user with "--as-root" flag.

The *debug* command you can invoke other types of objects besids pods, any controller resource that 
creates a pod, deployment, build or job. The 'debug' command also works with nodes, resources that creates
pods, such as image strem tags. You can also use the "--image=IMAGE" option of the debug command to 
start a shell session by using a specified image.

	'oc debug'
	If you do not include a resource type and name, then the debug command starts a shell session into
	a pod by using the OpenShift tools image.

	'oc debug job/test --as-user=1000000'
	Example to tess running a job pod as a non-root user
	
	'oc debug node/master01' -> 
		Starting pod/master01-debug-gdmkp ...
		To use host binaries, run chroot /host
		Pod IP: 192.168.50.10
		If you don't see a command prompt, try pressing enter.

		sh-5.1# 'chroot /host'
		sh-5.1# 

The debug pod is deleted when the remote command completes, or when the user interrupts the shell.

### Collect Information for Support Requests

When opening a support case, it is helpful to provide debugging information about your cluster to Red Hat
Support. It is recommended that you provide the following information:

- Data gathered by using the 'oc adm must-gather' command as a cluster administrator
- The unique cluster ID

The 'oc adm must-gather" command collects resource definitions and service logs from your cluster that
are most likely needed for debugging issues. A pod is created in temporary namespace on the cluster and
it gathers the debugging information. By default, the oc adm must-gather command uses the default plug-in
image, and writes into the ./must-gather.local. directory on your local system. To write to a specific
local directory, you can also use the "--dest-dir" option, such as in the following example:

	'oc adm must-gather --dest-dir /home/student/must-gather'
	Gather the debug informations
	
	'tar cvaf mustgather.tar must-gather/'
	Create a compressed archive file from the must-gather directory, replace "must-gather/" with the 
	actual directory path.
	
	Then, attach the compressed archive file to your support case in the Red Hat Customer Portal.

Like 'oc adm must-gather' command the 'oc adm inspect'  command gathers information on a specified resource.

	'oc adm inspect clusteroperator/openshift-apiserver clusteroperator/kube-apiserver'
	Collects debugging data for the openshift-apiserver and kube-apiserver cluster operators.
	
	The oc adm inspect command can also use the "--dest-dir" option to specify a local directory to write
	the gathered information. The command shows all logs by default.

	'oc adm inspect clusteroperator/openshift-apiserver --since 10m'
	The command shows all logs by default. Use the --since option to filter the results to logs that
	are later than a relative duration, such as 5s, 2m, or 3h.

---

## CREATE LINUX CONTAINERS and KUBERNETES PODS

### Running containers in Pods

Bothe OpenShift and Kubernetes offer many ways of creating containers in pods. Using the CLI tool 'oc' or
'kunectl' with the "run" option to create and deploy an application from an container image.

Using the "run" option to create single pods for debugging or temporary tasks if fine. But for enterprise
applicaions using declarative YAML manifests with resources such as deployments with relicasets is the 
recommended way to ensure ensuring reproducibility and manageability.

	'oc run _resource_/_name_ --image _image_ ...'
	
	'oc run web-server --image registry.access.redhat.com/ubi10/httpd-24'
	Deploy an Apache HTTPD application in a pod named "web-server" using a specific image

With the "run" option you can use many flags. The flag "--command" executes a custom command and its arguments
inside a container instead of the default comand. The "--command" must be followed by "--" by seperate
the command from the flags of the "run" option.

	'oc run _resource_/_name_ --image _image_ --command -- cmd arg1 ... argN'
	Execute a custom command and arguments, ignote the default command in the container image

The "--" can also be used to provide cutom argument to the default command. No "--command" is needed.

	'oc run _resource_/_name_ --image _image_ -- arg1 ... argN'
	Execute the default command but use other arguments and not the default tones.

To start an interactive session with a container use the flag "-it" before the pods name. "i" stand for
standard input or interactive and "t" for tty session. Using the "-it" starts a interactive remote shell
in a container but you need to add additional command to the shell itselft using "--command"

	'oc run -it my-app --image registry.access.redhat.com/ubi10/ubi --command -- /bin/bash'
	Run an interactive session inside the conatiner to the "/bin/bash" shell.

Always when you run any commands it will be inside the current project or namespace. Add "-n" or "--namespace"
to do it in a specific namespace or project.

You are abel to define a restart policy for a pod by using the "--restart" flag. A restart policy determins
how the cluster response the the container in that pod exists there are 3 accepted values or policies.

- Always  
	The cluster continuously tries to restart a container that exits, whether it exited successfully
	or with an error. This policy is the default.

- OnFailure  
	The cluster restarts a container only if it exits with an error.

- Never  
	The cluster does not try to restart exited or failed containers. Instead, the pods immediately fail
	and exit.

Example of usage

	'oc run -it my-app --image registry.access.redhat.com/ubi10/ubi --restart Never --command -- date'
	Executes the 'date' command in a container inside of pod named "my-app". The output of the command
	is rediected to the output and the Pod will "Never" restart.

Use "--rm" flag to delete a Pod after it exists.

	'oc run -it my-app --rm --image registry.access.redhat.com/ubi10/ubi --restart Never --command -- date' 
	Same as the one above but the Pods gets deleted after it exits.

Add environmetal variables with the "--env" flag

	'oc run mysql --image registry.redhat.io/rhel9/mysql-80 --env MYSQL_ROOT_PASSWORD=myP@$$123'
	Adds the environmental value MYSQL_ROOT_PASSWORD=myP@$$123 to the container.


### User and Group IDs Assignment

Adding a project OpenShift add annotaions to the project to determin the user ID (UID) range and supplemental 
group ID (GID) for the pods an their containers. The annotations can be retrived with the "describe"
option.

	'oc describe project my-app' ->
		Name:			my-app
		...
		Annotations:   openshift.io/description=
					   openshift.io/display-name=
					   openshift.io/requester=developer
					   openshift.io/sa.scc.mcs=s0:c27,c4
					   openshift.io/sa.scc.supplemental-groups=1000710000/10000
					   openshift.io/sa.scc.uid-range=1000710000/10000
		...

The values for supplemental-groups and uid-range use the base/size format. In 1000710000/10000, the 
1000710000 is the starting ID for the block that is allocated to this project. The 10000number is the
size of the block. This gives 10000 unique UIDs and GIDs to containers that are running within it starting
from the base ID of 1000710000.

With OpenShift default security polices a regular user can not choose the USER or UIDs for their container.
Any USER instruction in the container image are ignored when a regular cluster user creates a Pod. Instead
a random UID is assigned to the containers orimary process from the uid-renge that is defined in the
projects annotations.

The primary group IPD (GID) of the process is always 0 which is the root group. The process runs with a
non-root UID but belongs to the root group, any files or folders that the container writes to must be
owned by the root group and the group have write permission.

This feature is essential for security. Even if th process in the container belongs to the root group
its non-root UID makes it an unprivileged account.

When a cluster admin creates a pod the User instructions in the container is processes.  
Example if the USER instruction for the container image is set to 0 the process in the container runs 
as the root privileged account, with a UID of 0. This container becomes a security risk, a privileges
account in a container har unrestricted access to the containers host system. Which means with unrestricted
access that the container can modify or delete system files, install software or otherwise compromise 
its host. Red Hat recommends that container are run as rootless or as an unprivileged user with only
the necessary privileges for the container to run. This is way a distinct block of UID are assigned to
each project, OpenShift ensures that containers in different project do not run with the same UID.


#### Pod Security

Then a pod is created without the a defined security context the Kubernetes Pod Security Admission (PSA)
ontroller issues a warning. Red Hat OpenShift Container Platform 4.18 integrates the PSA controller which
enforces security profiles (privileged, baseline, restricted) at the namespace level. These security contexts
grant or deny OS-level privileges to Pods.

Red Hat OpenShift uses the Security Context Constraints (SCC) controller to provide safe defaults for
pod security.  SCCs gran tpods the needed premissions to meet the requirementsof the enforced PSA profile.


### Execute Commands in Running Containers

To execute a command in a running container you can use the "exec" option in the CLI

	'oc exec _RESOURCE_/_NAME_ -- _COMMAND_ arg1 ...argN ...'
	The executed command is set to the teminal
	
	'oc exec my-app -- date' -> "Tue Feb 21 20:46:50 UTC 2023"
	Executeing the date command and getting it output in the terminal.

Normally the executed command is executed in the first container in a pod. In a multicontainer pod you
are able to specify which container to execute the command with the "-c" or "--container" flag

	'oc exec my-app -c ruby-container -- date' -> "Tue Feb 21 20:46:50 UTC 2023"
	The command "date" is executed in a container named "ruby-container" in a pod named "my-app"

The "exec" option also accepts the "-i" and "-t" flags for an interactive session with a container in
a pod.

	'oc exec my-app -c ruby-container -it -- bash -il' ->
		[1000780000@ruby-container /]$
	
	Kuberneets sends the stdin to the bash shell in the "ruby-container" container from the "my-app"
	pod. The stdout and stderr from the shell are sent back to the terminal.
	
	This would be considered a shell session directly into the container, raw terminal.

	Now additional commands can be executed in the container.
	
		[1000780000@ruby-container /]$ date
		Tue Feb 21 21:16:00 UTC 2023
	
	Executing the 'date' command and get the date.
	
		[1000780000@ruby-container /]$ exit
		
	Using the 'exit' to terminate the shell terminal to the container


### Container Logs
 
Container logs are the standard out (stdout) and stadard error (stderr) output from a container. By using 
the *logs* option with the pods name you can get them.

The commands have the following flags

| Flag                  | Description |
|---                    |--- |
| -l or --selector=""   | Filter objects based on the specified key:value label constraint. |
| --tail=               | Specify the number of lines of recent log files to display; the default value is -1 with no selectors, which displays all log lines. |
| -f or --follow        | Follow, or stream, logs for a container. |
| -c or --container=    | Print the logs of a specific container in a multicontainer pod. If this option is omitted, then logs are shown from the first container that is defined in the pod or from the container that the kubectl.kubernetes.io/default-container annotation on the pod specifies. |
| -p or --previous=true | Print the logs of a previous container instance in the pod, if it exists. This option is helpful for troubleshooting a pod that failed to start, because it prints the logs of the last attempt. |

	'oc logs postgresql-1-jw89j --tail=10' ->
		done
		server stopped
		Starting server...
		2023-01-04 22:00:16.945 UTC [1] LOG:  starting PostgreSQL 12.11 on x86_64-redhat-linux-gnu, compiled by gcc (GCC) 8.5.0 20210514 (Red Hat 8.5.0-10), 64-bit
		2023-01-04 22:00:16.946 UTC [1] LOG:  listening on IPv4 address "0.0.0.0", port 5432
		2023-01-04 22:00:16.946 UTC [1] LOG:  listening on IPv6 address "::", port 5432

	The output is restricted to the last 10 lines

You are able to use the option *attach* to connect and start an interactive session on a running container
in a pod. The "-c" for specifying a container are required for a multicontainer pods, if it is not used
then Kubernetes will use the *kubectl.kubernetes.io/default-container* annotation on the pod to select
the container otherwise the first container in the pod is chosen.  You can use the interactive session
to get logs or troubleshoot application issues.

	'oc attach my-app -it' ->
		If you don't see a command prompt, try pressing enter.
		bash-4.4$

You can also use the OpenShift webconsole by clicking on the "Logs" tab in a Pod when seeing its details.
When thare are more than one container in the pod youmust chose which container to view.


### Deleting Resources

You can delete resources such as Pods by using the *delete* option. The command can delete resources
by resource type and name, resource type and label, standard in (stdin) and with JSON and YAML formatted
files. The command only accept one argument at a time.

	'oc delete pod php-app'
	Giving the resource type (pod) and the name of the pod to delete the pod.
	
	'oc delete pod -l app=my-app'
	Giving the resource type and the label with the key=value argument "-l app=my=app" to delete the pod.
	
	'oc delete pod -f ~/php-app.json'
	Providing the resource type and a JSON or YAML formatted file. The file specifies the name og the resource.
	
	'cat ~/php-app.json | oc delete -f -'
	You can also use stdin and a JSON or YAML-formatted file that includes the resource type and resource
	name with the delete command.

You are also able to use the OpenShift web console to delete a pod. In the pods detail you have the
dropdown "Actions" where you can select "Delete Pod".

Pods support a graceful termination. It means that the pods try to terminate theoir processes first
before Kubernetes forcibly terminates the Pods. The gracetime before graceful and forcible is 30 sec, to
change the time you add the flag "--grace-period" and a time in seconds in the delete command.

	'oc delete pod php-app --grace-period=10'
	Changing the grace period to 10 sec instead of the defalt 30 sec
	
	'oc delete pod php-app --now'
	To shutdown the the pod immediately you can either set the grace period to 1 sec or use "--now" flag.
	
	'oc delete pod php-app --force'
	Forceibly delete a pod with the "--force" flag. If this is done Kubernetes will not wait for confirmation
	that the pod's process ended, which can leave the pod's process running until the node detect the deletion.
	
	Forceibly deleting a pod can result in inconsistency or data loss. Only do this if you are sure 
	that the pod's processes are terminated.
	
	'oc delete pods --all'
	Delete all pods in a project use the "--all" flag.

To delete a project and all of its resources you can use the *delete* option with the resouce *project*

	'oc delete project my-app'
	Delete the project named "my-app"
	
	Make sure that nothing is still running in the project before doing this.


### The CRI-O Container Engine

A container enige is needed to run containers. Worker (Compute) and control plane nodes in a Red Hat OpenShift
Container Platform cluster use the CRI-O container engine to run containers. The CRI-O container engine
is a runtime that is desinged and optimized specifically for running in a Kubernets cluster, Docker
and Podman are NOT. Because CRI-O meets the Kubernetes CRI (Container Runtime Interface) standards the 
container engine can integrate with other Kubernetes and OpenShift tools, sucj as networking and storage
plug-ins.

For more information about the Kubernetes Container Runtime Interface (CRI) standards, refer to the 
CRI-API repository at https://github.com/kubernetes/cri-api.

CRI-O procides a CLI interface to manage containers using the *'crictl'* command. AS with the other container
CLIs it has different options (subcommands) that can be used.

| Command        | Description |
|---             |--- |
| crictl pods    | List all pods on a node. |
| crictl image   | List all images on a node. |
| crictl inspect | Retrieve the status of one or more containers. |
| crictl exec    | Run a command in a running container. |
| crictl logs    | Retrieve the logs of a container. |
| crictl ps      | List running containers on a node. |

To manage containers with the *crictl* command you need to identify the node that is running your containers.

	'oc get pods -o wide' ->
		NAME                READY STATUS    RESTARTS AGE IP        NODE
		postgresql-1-8lzf2  1/1   Running   0        20m 10.8.0.64 master01
		postgresql-1-deploy 0/1   Completed 0        21m 10.8.0.63 master01

	'oc get pod postgresql-1-8lzf2 -o jsonpath="{.spec.nodeName}{"\n"}"' -> "master01"
	
	In both above ways you can see thet the node running the pod "postgresql-1-8lzf2" is "master01"

After you are aware of which node you must connect to it as acluster administartor. Cluster administrators
can use SSH to connect to a node or create a debug pod for the node. Regular users can not connect or
create debug pods for cluster nodes.

As a cluster administrator you can create a debug pod for the node with *'oc debug node ...'* command. 
OpenShift will create a debug-pod in your current selected project and automatically connect you to the
pod. Then you must enable access host binaries (*crictl* tool) with 'chroot /host' command. This command
will mount the hostäs root file system in the "/host" directory within the debug pod shell. By changing
the root directory to the /host directory you  are able to run binaries contained in the hosts's executable
path.

	'oc debug node/master01'
		Starting pod/master01-debug
		...
		If you don't see a command prompt, try pressing enter.
		sh-4.4# 'chroot /host'

After enabling host binaries you can use the 'crictl' command to manage the containes on the node.

	'crictl ps --name postgresql' ->
		CONTAINER     IMAGE        CREATED STATE   NAME       ATTEMPT POD ID      POD
		27943ae4f3024 image...7104 5...ago Running postgresql 0       5768...f015 po...
	
	Using 'crictl ps' command to get the process PID of a running container. To find the PID of a running
	container, you must first determine the container's ID. You can use the 'crictl ps' command with the
	"--name" flag to filter the command output to a specific container.
	
	'crictl ps --name postgresql -o json | jq .containers[0].id' -> "2794...29a4"
	The default output of 'crictl' is a table. You can find the shorter container ID and use it to find
	the PID of the running container. But you can use "-o" or "--output" falgs to specify a format and
	then parse the output.

Having the Containers ID you can now find the process ID (PID) of that container by using the 'crictl inspect'

	'crictl inspect 27943ae4f3024 | grep pid' -> "43453"
	A simple grep on the output of 'crictl inspect'
	
	'crictl inspect -o json 27943ae4f3024 | jq .info.pid' -> "pid": 43453,
	Using a formatted output and parsing it
	
	'crictl inspect --output go-template --template "{{.info.pid}}" 27943ae4f3024' -> "43453"
	Alternative using the build-in methods as the go-templat option to parse out the ID without using
	any external tools

Last you can now list the namespaces of the container by specifying the PID och the container.

	'lsns -p 43453' ->
		NS 			TYPE	NPROCS	PID		USER		COMMAND
		4026531835	cgroup	530		1 		root		/usr/lib/systemd/systemd --switche...
		4026531837	user	530		1		root		/usr/lib/systemd/systemd --switche...
		4026537853	uts		8		43453	1000690000	postgres
		4026537854	ipc		8		43453	1000690000	postgres
		4026537856	net		8		43453	1000690000	postgres
	
	Using 'lsns' and a specified PID
	
	'nsenter -t 43453 -p -r ps -ef' ->
		UID          PID    PPID  C STIME TTY          TIME CMD
		1000690+       1       0  0 18:49 ?        00:00:00 postgres
		1000690+      58       1  0 18:49 ?        00:00:00 postgres: logger
		1000690+      60       1  0 18:49 ?        00:00:00 postgres: checkpointer
		1000690+      61       1  0 18:49 ?        00:00:00 postgres: background writer
		1000690+      62       1  0 18:49 ?        00:00:00 postgres: walwriter
		1000690+      63       1  0 18:49 ?        00:00:00 postgres: autovacuum lau...
		1000690+      64       1  0 18:49 ?        00:00:00 postgres: stats collector
		1000690+      65       1  0 18:49 ?        00:00:00 postgres: logical replic...
		root        7414       0  0 20:14 ?        00:00:00 ps -ef
	
	Using 'nsenter' command togheter with the PID to eneter a specific namespace of a running container.
	
	For example, you can use the nsenter command to execute a command within a specified namespace on
	a running container. The following example executes the ps -ef command within the process namespace
	of a running container.
	
	'nsenter -t 43453 -a ps -ef'
		UID          PID    PPID  C STIME TTY          TIME CMD
		1000690+       1       0  0 18:49 ?        00:00:00 postgres
		1000690+      58       1  0 18:49 ?        00:00:00 postgres: logger
		1000690+      60       1  0 18:49 ?        00:00:00 postgres: checkpointer
		1000690+      61       1  0 18:49 ?        00:00:00 postgres: background writer
		1000690+      62       1  0 18:49 ?        00:00:00 postgres: walwriter
		1000690+      63       1  0 18:49 ?        00:00:00 postgres: autovacuum lau...
		1000690+      64       1  0 18:49 ?        00:00:00 postgres: stats collector
		1000690+      65       1  0 18:49 ?        00:00:00 postgres: logical replic...
		root       10058       0  0 20:45 ?        00:00:00 ps -ef
	
	The -t option specifies the PID of the running container as the target PID for the nsenter command.
	The -p option directs the nsenter command to enter the process or pid namespace. The -r option sets
	the top-level directory of the process namespace as the root directory, thus enabling commands to
	execute in the context of the namespace. You can also use the -a option to execute a command in all
	of the container's namespaces.

---

## FIND AND INSPECT CONTAINER IMAGES

### Container Image Overview

A container is an isolated runtime envrionment where applications are executed as isolated processes.
The isolation of the runtime envrionment makes sure that the application do not affect other containers
or system processes.

A container image containe the package version of the application with all of it dependencies for it
to be able to run. Images can exists without containers but containers depend om images. A container
uses a container imageto build the runtime environment to execute the applications.

Container can be split into

- Container images  
	Contains the immutable data that defines the application and it needed libraries

- Container instance  
	The running of the container image that are isolated by a set of kernel namespaces

A container image can be reused many times to create a container instance.


### Container Image Registries

Image registries are services that offer container images to download. Image creators and maintainers
can store and distribute container images in a controlled manner for public or private use.


#### Red Hat Registry

Red Hat distributes container images through the Red Hat Ecosystem Catalog, https://catalog.redhat.com/,
which provides a centralized searching utility. 

Because the Red Hat Ecosystem Catalog also contains software products other than container images, go
to https://catalog.redhat.com/software/containers/explore to search for specific container images.

The Red Hat internal security team validates all images in the container catalog. Red Hat rebuilds all
components to avoid known security vulnerabilities.

Red Hat container images provide the following benefits:

- Trusted source: All container images use sources that Red Hat knows and trusts.
- Original dependencies: None of the container packages are altered, and they include only known libraries.
- Vulnerability-free: Container images are without known critical vulnerabilities in the platform components
	or layers.
- Runtime protection: All applications in container images run as non-root users to minimize the exposure
	surface to malicious or faulty applications.
- Red Hat Enterprise Linux (RHEL) compatible: Container images are compatible with all RHEL platforms, 
	from bare metal to cloud.
- Red Hat support: Red Hat commercially supports the container images in the catalog.


#### Quay.io

Although the Red Hat Registry stores only images from Red Hat and certified providers, you can store your
own images with Quay.io, which is another public image registry that Red Hat sponsors. 


#### Private Registries

Image creators or maintainers can choose to make their images publicly available. However, other image
creators might prefer to keep their images private, for the following reasons:

- Company privacy and protection of secrets
- Legal restrictions and laws
- Avoidance of publishing images in development

In some cases, private images are preferred. Private images are more secure than images in public registries.
Private registries give image creators control over image placement, distribution, and usage.


#### Public Registries

Other public registries, such as Docker Hub and Amazon ECR, are also available for storing, sharing,
and consuming container images. These registries can include official images that the registry owners
or the registry community users create and maintain. 

Consuming container images from public registries brings risks. 

Before you use a container image from a public registry, review and test the container image for your
environment.


### Container Image Identifiers

Several objects provide identifying information about a container image.

- Registry  
	The server that stores and shares congainer images. Containe one or more repositories that contains
	tagged container images.
	
- Name  
	Identifies the container image repository (on the server).
	
- Tag  
	Is a label for the container image in the repository to provide a unique identifier for containers
	with the same name. Is often usesd as the version for the container image. It is seperated from 
	the name by a ":".
	
Example registry.com/hello/world:v23  

- "registry.com/hello": the server and the account or folder on the server where the container 
	image is stored
- "world": the image name
- "v23": the tag for tis specific image. Combined with the name gets you a unique container image

There is one thing left when it comes to identifying information about a container image.

- ID or Hash  
	Is a SHA code to locate, pull or verify an image within the regisry. It can not be chnaged and it
	references the same container image content.


### Container Image Components

A container image is composed of multiple components

- Layers  
	Container images are created from instructions, each instructions adds a layer to the container image,
	each layer consists of the differences between it and the previous layer. Layers are stacked on each
	other.
	
- Metadata  
	Includer the instructions and documentations for a container image.


### Container Image Instructions and Metadata

Container images consists of instructions or steps and metadata for building the image. They can be
overridden during container creation to adjust the container image to fit the needs.

Instructions

- ENV  
	Defines the available environment variables in the container. A container image might include multiple
	ENV instructions. Any container can recognize additional environment variables that are not listed
	in its metadata.

- ARG  
	Defines build-time variables, typically to make a customizable container build. Developers commonly
	configure the ENV instructions by using the ARG instruction. ARG is useful for preserving the build-time
	variables for run time.

- USER  
	Defines the active user in the container. Later instructions run as this user. It is a good practice
	to define a user other than root for security purposes. OpenShift does not honor the user in a container
	image, for regular cluster users. Only cluster administrators can run containers (pods) with their
	chosen user IDs (UIDs) and group IDs (GIDs).

- ENTRYPOINT  
	Defines the executable to run when the container is started.

- CMD  
	Defines the command to execute when the container is started. This command is passed to the executable
	that the ENTRYPOINT instruction defines. Base images define a default ENTRYPOINT executable, which
	is usually a shell executable, such as Bash.

- WORKDIR  
	Defines the current working directory within the container. Later instructions execute within this
	directory.

	Metadata is used for documentation purposes, and does not affect the state of a running container.
	You can also override the metadata values during container creation.

The following metadata is for information only, and does not affect the state of the running container:

- EXPOSE  
	Specifies the network port that the application binds to within the container. This metadata does
	not automatically bind the port on the host, and is used only for documentation purposes.

- VOLUME  
	Defines where to store data outside the container. The value shows the path where your container
	runtime mounts the directory inside the container. More than one path can be defined to create multiple volumes.

- LABEL  
	Defines a key-value pair in the metadata of the image for organization and image selection.

Container engines are not required to honor metadata in a container image, such as USER or EXPOSE. 
A container engine can also recognize additional environment variables that are not listed in the
container image metadata.


### Base Images

Is the container image form which a container image is built upon. Depending of which base image you use
it is based upon a Linux distribution and with it have different componenets.

- Package manager
- Init system
- File-system layout
- Preinstalled dependencies and runtimes

The base image can also influence factors such as image size, vendor support, and processor compatibility.

#### Red Hat images

Red Hat base images are created to the base operating system layer for container images. These images 
are intended to be a common staring point for containers and is known as universal base images (UBI).
Red Hat images are OCI compliant and contain portions of Red Hat Enterprise Linux. Uses DNF repositories
to add applications dependencies.

Different types of UBI images are provided free by Red Hat, all images uses RHEL at it core and are available
from the Red Hat Container Catalog. The differences are

- Standard  
	This image is the primary UBI, which includes DNF, systemd, and utilities such as gzip and tar.

- Init  
	This image simplifies running multiple applications within a single container by managing them
	with systemd.

- Minimal  
	This image is smaller than the init image and provides nice-to-have features. This image uses the
	microdnf minimal package manager instead of the full-sized version of DNF.

- Micro  
	This image is the smallest available UBI, and includes only the minimum packages. For example, this
	image does not include a package manager.


### Inspecting and Managing Container Images

Various tools can inspect and manage container images, including the *'oc image'* command and *'skopeo'*.


#### Skopeo

A tool to inspect and manage rempte container images. You can copy and sync container images from different
container registries and repositories. You are able to copy an image from a remote repository and save
it to local disk. Even delete it from a remote regisrty if you have the registry permissions. You are
able to inspect the configuration and content of a container image and list available tags.

Skopeo is executed with the CLI tool 'skopeo' wich can be installed with various package managers. A windows
version of this utility does not exists, but it does exists as an container image that can be used.

The 'skopeo' utility provides commands for managing and inspecting container images and container image
registries.

	'skopeo login quay.io'
	Login to quay.io using skopeo.

After a login if neede into a registry and when working with container images you must specify the *transport*
method that you can to use.

- 'docker': is used for container registries.
- 'dir' for local directories.

When working with other CLI tools you normally do not need to specify the transport when executing command
but skopeo does not define a default transport. The transport method together with the container image name
is used.

	'skopeo list-tags docker://registry.access.redhat.com/ubi10/httpd-24' -> ...
	Will list the tags on the specified image.
	
	Since we are connecting to a registry the transport 'docker' is used with "://" and the container 
	image full path.

Skopeo options / commands

- 'skopeo list-tags ...'  
	Listing tags for a container image.

- 'skopeo inspect ...'  
	View details about the container image, enviromental variables, tags and so on. Adding the "--config"
	flag you can also view the configuration, metadata and history.
	
		'skopeo inspect docker://registry.access.redhat.com/ubi10/httpd-24' ->
			{
				"Name": "registry.access.redhat.com/ubi10/httpd-24",
				"Digest": "sha256:ea8324928dfa88775daf55a76f1ec0ee643b3df926f78822c722b697fad3cbcf",
				"RepoTags": [
				...]
				...
			}
		
		'skopeo inspect --config docker://registry.access.redhat.com/ubi10/httpd-24' -> ...

- 'skopeo copy ...'  
	Copy an image from on location or repository to another. 
	
		'skopeo copy docker://quay.io/skopeo/stable:latest docker://registry.example.com/skopeo:latest'
		Copy an container image from quay.io to registry.example.com

- 'skopeo delete ...'  
	Delete a container image from a repository
	
		'skopeo delete docker://registry.example.com/skopeo:latest'

- 'skopeo sync ...'  
	Synchronize one or more images from one location to another. Use this command to copy all container
	images from a source to a destination.
	
		'skopeo sync --src docker --dest docker registry.access.redhat.com/ubi10/httpd-24 registry.example.com/httpd-24'
		'skopeo sync [command options] --src transport --dest transport SOURCE DESTINATION format'

Some registries require users to authenticate before accessing container images and metadata.

	'skopeo inspect docker://registry.redhat.io/rhel10/httpd-24' ->
		"FATA[0000] Error parsing image name "docker://registry.redhat.io/rhel10/httpd-24": unable to
		retrive auth token ..."
	
	When not being able to access you migh have to login to the registry

	'skopeo login registry.redhat.io'
	And you will need to provide your username and password.
	
	The credentials are stored in the "${XDG_RUNTIME_DIR}/containers/auth.json" file, ${XDG_RUNTIME_DIR} 
	refers to a directory that is specific to the current user. The credentials are encoded in Base64
	format.


#### 'oc image' command

It can be used to inspect, configure or retrive information about the container.

- 'oc image info ...'  
	Inspects and retrieves information about a container image. Identify the ID or Hash of the image.
	Also review container image metadata suchh as environmental variables, network ports and commands.
	
	If the image is provide for different architectures, AMD64, ARM64, ... you must include the flag
	"--filter-by-os"
	
		'oc image info registry.access.redhat.com/ubi10/httpd-24 --filter-by-os amd64' ->
			Name:          registry.access.redhat.com/ubi10/httpd-24:10.0
			Digest:        sha256:b765...
			...

- 'oc image append ...'  
	Use this command to add layers to container images, and then push the container image to a registry.
	
- 'oc image extract ...'  
	You can use this command to extract or copy files from a container image to a local disk. Use this
	command to access the contents of a container image without first running the image as a container. 
	A runing container engine is not required.

- 'oc image mirror ...'  
	Copy, or mirror, container images from one container registry or repository to another. For example,
	you can use this command to mirror container images between public and private registries. You can
	also use this command to copy a container image from a registry to a disk. The command mirrors the
	HTTP structure of a container registry to a directory on a disk. The directory on the disk can then
	be served as a container registry.


### Running Containers as Root

Running a contaoiner as root is a security risk, it a hacker exploits the appalication access the container
and access a vulnerability to excape from the containereized environment and access the host system.
Attackers might escape the containerized environment by exploiting bugs and vulnerabilities that are
typically in the kernel or the container runtime.

Traditionally, when an attacker gains access to the container file system by using an exploit, the 
root user inside the container corresponds to the root user on the host system. If an attacker escapes
 the container isolation, then they have access to elevated privileges on the host system, which provides
  a vector of attack that could cause damage.

Containers that do not run as the root user might prove unsuitable for use in your application because
of the following limitations

- Non-trivial Containerization  
	Some applications might require the root user. Depending on the application architecture, some 
	applications might not be suitable for non-privileged containers, or might require additional experience
	to containerize.

	For example, applications such as HTTPd and Nginx start a bootstrap process and then create a process
	with a non-privileged user, which interacts with external users. Such applications are non-trivial
	to containerize for non-privileged use.

	Red Hat provides containerized versions of HTTPd and Nginx that do not require root privileges for
	production usage. You can peruse these validated containers in the Red Hat container registry

- Required Use of Privileged Utilities  
	Non-privileged containers cannot bind to privileged ports, such as the 80 or 443 ports. Red Hat 
	advises against using privileged ports. Instead, use port forwarding to avoid the use of privileged
	ports within container definitions.

	Similarly, non-privileged containers cannot use the ping utility by default, because it requires
	elevated privileges to establish raw sockets.

---

## TROUBLESHOOT CONTAINERS AND PODS 

### Container Troubleshooting Overview

Container are desinged to be immutable and short lived. When chnages are needed or a new container is
available you can apply them without needed to remove the old ones.

Updating a running container should only be done for troubleshooting. You should not edit a running container
to fix an error in deployment. A change or edit to the container image should be done after troubleshooting
and then redeploy the containers with the change or edit.


### CLI Troubleshooting Tools

Various tools can be use to interact with, inspect or alter running containers. Commands like 'oc get'
can be used for specific resource types. Other commands are available for detailed instections of a 
resource or to update a resource in real time. 

'oc' options to validate the functions and environment for a container instance (running)

- 'oc describe ...': Display the details of a resource.
- 'oc edit ...': Edit a resource configuration by using the system editor.
- 'oc patch ...': Update a specific attribute or field for a resource.
- 'oc cp ...': Copy files and directories to and from containers.
- 'oc exec ...': Execute a command within a specified container.
- 'oc port-forward ...': Configure a port forwarder for a specified container.
- 'oc logs ...': Retrieve the logs for a specified container.
- 'oc rsync ...': Synchronize files and directories to and from containers.
- 'oc rsh ...': Start a remote shell within a specified container.

### Editing Resources

Troubleshooting begins with inspection and data gathering. The *describe* option can provide helpful
details about the resource such as definition and its purpose.

	'oc describe pod dns-default-lt13h' ->
		Name:               dns-default-lt13h
		Namespace:          openshift-dns
		Priority:           2000001000
		Priority Class Name: system-node-critical
		...
		
	Retrive information about a pod in the "openshift-dns" namespace

Different tools can be used to edit a resouce such as a running container.

The *edit* options opens up the specific resouce in the defualt editor in the terminal. The editor can
be set by the "KUBE_EDITOR" or the EDITOR environmental variable.

	'oc edit pod mongo-app-sw88b'
	Opens up the resource in an editor.

Instead of edit the resouce you can apply the change with the *patch* option. If will update the specific
fields.

	'oc patch pod valid-pod --type='json' \
	-p='[{"op": "replace", "path": "/spec/containers/0/image", \
	"value":"http://registry.access.redhat.com/ubi8/httpd-24"}]''
	
	The above patch will update the container image that the pod uses.


### Copy Files to and from Containers

Administators can copy files and folders to or from a container to inspect, update or correct functionality.
Adding a configuration file, retrieving an application log are exaples.

OBS!! For *'op cp ...'* to work the binary *'tar'* must be present in the container

	'oc cp apache-app-kc82c:/var/www/html/index.html /tmp/index.bak'
	Coping the file "/var/www/html/index.html" from the container instance "apache-app-kc82c" and saving
	it on the local folder at "/tmp/index.bak"

	'oc cp /tmp/index.html apache-app-kc82c:/var/www/html/'
	The reverse, coping a local file into a running container, into folder "/var/www/html/".

OBS!! If you are doing a copy to a pod with multiple container you need to use "-c" and specify the container
or by defalut the target will be the first container.

Instead of doing a copy you can do a *rsync* to sync files and folder between the local filesystem and
a running container.

	'oc rsync apache-app-kc82c:/var/www/ /tmp/web_files'
	Sync from the container to the local filesystem

OBS!! 'rsync' must be present in the container and local system. If missing from the local system a tar
file will be created and sent to the container and it will extract the files from the archive. If both
*tar* and *rsync* missingan error will occur and the *rsync* option fails.


### Remote Container Access

Exposing a port for a container is routine, specially for a container that provide a service. In a cluster
port forward connections are med through the *kubelet* that maps a local post in the system to a port
on a pod. Configuring a port forward creates a request through the Kubernetes API and creates a multiplexed
stream such as HTTP/2 with a port header that specifies the target port in the pod. The kubelet delivers
the stream data to the target pod and port and reverse for egress data from the pod.

Doing a troubleshoot on an application that normally runs without the need to connect locally you can
use port forward function to expose connectivity to the pod for investigation. This function gives an
administrator a way to connect on the new port and inspect the applicaion. When done the applicaion is
redeployed without the port-forward connection.

	'oc port-forward nginx-app-cc78k 8080:80' ->
		Forwarding from 127.0.0.1:8080 -> 80
		Forwarding from [::1]:8080 -> 80
	
	Port-forwarding a local port listing on 8080 and forward it to port 80 on a pod named "nginx-app-cc78k".


### Connect to Running Containers

Administrators use CLI tools to connect to a container via a shell for forensic inspections. With this
approach, you can connect to, inspect, and run any available commands within the specified container.

	'oc rsh tomcat-app-jw53r' -> Get a shell in the conatiner

Use "-c" to specify a conatiner in a pulti-container pod or connect to the first container.

You can also use the webconsole to connect to a running container. From the Pod details page use the
tab "Terminal" (select the container if more lives in the pod).


### Execute Commands in a Container

Passing commadn to be executed in a running container from the CLI is another way of troubleshoot a 
running container. Use this way to connect or to send command to the container.

Use "-c" to specify a conatiner in a pulti-container pod or connect to the first container.

	'oc exec -it mariadb-lc78h -- ls /' -> "bin boot dev etc ..."
	'oc exec mariadb-lc78h -- ls /' -> ...
	
	Sending a command to be executed inside the container.

Normally you add the "-it" flag to the *exec* option. These flafs instruct the stdin to the container
and stdout/stderr back to the terminal. Using them or not may impact the output.
 

### Container Events and Logs

Looking at historical actions for a container can offer information about is lifecycle and health of
the deployment. The cluster logs provides the chronological details of the containers actions. Administrator
inspects this log for information and issues for the running container.

Use "-c" to specify a conatiner in a pulti-container pod or connect to the first container.

	'oc logs BIND9-app-rw43j' -> ...
	Getting the logs for the specific container named "BIND9-app-rw43j".

In Kubernetes an event is a report of something that happended in the cluster. 

	'oc get event' ->
		LAST SEEN   TYPE     REASON           OBJECT                           MESSAGE
		...
		21m         Normal   AddedInterface   pod/php-app-5d9b84b588-kzfxd     Add eth0 [10.8.0.93/23] from ovn-kubernetes
		21m         Normal   Pulled           pod/php-app-5d9b84b588-kzfxd     Container image "registry.ocp4.example.com:8443/redhattraining/php-webapp:v4" already present on machine
		21m         Normal   Created          pod/php-app-5d9b84b588-kzfxd     Created container php-webapp
		21m         Normal   Started          pod/php-app-5d9b84b588-kzfxd     Started container php-webapp

	The events for the current namespace.


### Available Linux Commands in Containers

Using Linux commands for troubleshooting applications can help. But when connecting to a container only
the tools and applications within that container are available. You can add tolls to the container environment
for helping with the troubleshooting.

But

- Additional tools increase the size of the image, which might impact container performance.
- Tools might require additional update packages and licensing terms, which can impact the ease of updating
	and distributing the container image.
- Hackers might exploit tools in the image.

So add the tools with this in mind, redeploy the "faulty" container afterworth to reduce the above concerns.
Then fix the issue that you found.
	

### Troubleshooting from Inside the Cluster

Administrator can create and deploy a container within the cluster fot investigation and remediation.
Creating a container containing cluster troubleshooting tools you have an enviroment to perform these
tasks from any computer with a cluster access.

Administrator should create a container of this kind that contain the tools needed for troubleshooting
the cluster and its containerized applications. In this way, you deploy this "toolbox" container to
supplement the forensic process and to provide an environment with the required commands and tools for
troubleshooting problematic containers. For example, the toolbox container can test how resources operate
inside a cluster, such as to confirm whether a pod can connect to resources outside the cluster. Regular
cluster users can also create a toolbox container to help with application troubleshooting. For example,
a regular user could run a pod with a MySQL client to connect to another pod that runs a MySQL server.


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




