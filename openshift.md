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
	
		- "--namespaced=true": specify the namespace
		- "--api-group appsapps": Limit the resouces in a spcific API group, for core resource use "--api-group"
		- "--sort-by name": If not empty sort listed resources by the field, "name" or "kind".

	'oc api-resources --namespaced=true --api-group apps --sort-by name' ->
		NAME                  SHORTNAMES   APIVERSION   NAMESPACED   KIND
		controllerrevisions                apps/v1      true         ControllerRevision
		daemonsets            ds           apps/v1      true         DaemonSet
		deployments           deploy       apps/v1      true         Deployment
		replicasets           rs           apps/v1      true         ReplicaSet
		statefulsets          sts          apps/v1      true         StatefulSet

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




