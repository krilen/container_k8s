# OPENSHIFT

Part 2/2

## DEPLOY APPLICATIONS FROM AN IMAGE AND FROM A TEMPLATE

### Deploying Applications

Container-based infrastructures support most types of traditional and modern applications.

The application term can refer to a software system or to a service within it. Given this ambiguity,
it is clearer to refer directly to resources, services, and other components.


### Resources and Resource Definitions

Kubernetes manages applications or services as a collection of resources. The resources ar configured
as pieces for compenents in the cluster. When a resouce is created the component is not created right
away. It is the cluster that creates the component.

A *resource type is a specific component type as a pod. Kubernetes has many default resource types some 
overlaps. OpenShift uses the Kubernetes defaults and also adds it own resource types. To add resouce
types you can create andimport CRD.


### Managing Resources

Reources can be added, viewed and edited in different ways, Yaml and Json format.

A resouce can be deleted in batch using a label selector or by deleing the entire namespace (project)

	'oc delete deployment -l app=my-app' -> "deployment.apps "my-app" deleted"
	Deleting deployments with the label app=my-lab

As with creation of a resouce the deletion is not immediate but is a request for a deletions.

### Common Resource Types and Their Uses

The following resources are standard OpenShift resources:

#### Templates

Template are an additon from OpenShift.

A template is amanifest that contains parameterized definitions of one or more resources. Predefined
templates are provided in the *openshift* namespace.

A template can be processed into a list using the *process* option which replaces values and generates
resource definitions. The definitions creates or updates resources in the cluster by using *apply* options.

	'oc process -f mysql-template.yaml -o yaml' ->
		apiVersion: v1
		items:
		- apiVersion: v1
		  kind: Secret
		  ...
		- apiVersion: v1
		  kind: Service
		  ...
		- apiVersion: v1
		  kind: PersistentVolumeClaim
		  ...
		- apiVersion: apps/v1
		  kind: Deployment
		  ...
	
	Processes the "mysql-template.yaml" template file and creates 4 resouces definitions.
	
	'oc process -f mysql-template.yaml --parameters' ->
		NAME                    DESCRIPTION 						...
		...
		MYSQL_USER              Username for MySQL user				...
		MYSQL_PASSWORD          Password for the MySQL connection	...
		MYSQL_ROOT_PASSWORD     Password for the MySQL root user	...
		MYSQL_DATABASE          Name of the MySQL database accessed	...
		VOLUME_CAPACITY         Volume space available for data,	...

	Using the "--parameters" flag instead displays the parametes of a template.

	'oc new-app --template mysql-persistent' ->
		Deploying template "db-app/mysql-persistent" to project db-app
		...
		The following service(s) have been created in your project: mysql.

			Username: userQSL
			Password: pyf0yElPvFWYQQou
		Database Name: sampledb
		Connection URL: mysql://mysql:3306/
		...
		* With parameters:
		  * Memory Limit=512Mi
		  * Namespace=openshift
		  * Database Service Name=mysql
		  * MySQL Connection Username=userQSL # generated
		  * MySQL Connection Password=pyf0yElPvFWYQQou # generated
		  * MySQL root user Password=HHbdurqWO5gAog2m # generated
		  * MySQL Database Name=sampledb
		  * Volume Capacity=1Gi
		  * Version of MySQL Image=8.0-el8

		Creating resources ... #1
		secret "mysql" created
		service "mysql" created
		persistentvolumeclaim "mysql" created
		deployment.apps "mysql" created
		--> Success
		...
	
	Using the template with the 'new-app' option in OpenShift. Using the 'new-app' option command with
	the "mysql-persistent" template to create a MySQL application and its supporting resources
	
	Explaination
	#1: Several resources are created to meet the requirements of the deployment, including a secret,
	a service, and a persistent volume claim.

It is possible to use template, helm carts, builder imgaes or dev iles to create aopplications from the
OpenShift web console. Using The "Developer Catalog" and as a developer go to "+Add" menus and click
"All Services" in the "Developer Catalog" section på open the catalog. Enter the an application name
to filter and search for an applicaion's template. Now you can change the default values, GIt repo, 
memory limit, ...

It is also possible to create an application from the "Topology" menu and click "Start building application"
(or click the book icon).


#### Pod

The pod is the smallest comput unit that can be dined and manages in OpenShift. A pod run one or more
containers that represent a single application. The containers in the pod share resources such as networking
and storage

	Example of a definition of a pod
	-----
	apiVersion: v1
	kind: Pod #1
	metadata: #2
	  annotations: {}
	  labels:
		deployment: docker-registry-1
	  name: registry
	  namespace: pod-registries
	spec: #3
	  containers:
	  - env:
		- name: OPENSHIFT_CA_DATA
		  value:
		image: openshift/origin-docker-registry:v0.6.2
		imagePullPolicy: IfNotPresent
		name: registry
		ports:
		- containerPort: 5000
		  protocol: TCP
		resources: {}
		securityContext: {}
		volumeMounts:
		- mountPath: /registry
		  name: registry-storage
	  dnsPolicy: ClusterFirst
	  imagePullSecrets:
	  - name: default-dockercfg-at06w
	  restartPolicy: Always
	  serviceAccount: default
	  volumes:
	  - emptyDir: {}
		name: registry-storage
	status: #4
	  conditions: {}
	
	Explanation for the pod definition
	-----
	#1: The resource kind is set to Pod.
	#2: Information that describes your application, such as the name, project, attached labels, and
		annotations.
	#3: Section where the application requirements are specified, such as the container name, the container
		image, environment variables, volume mounts, network configuration, and volumes.
	#4: Indicates the last condition of the pod, such as the last probe time, the last transition time,
		the status setting as true or false, and more.


#### Deployments

The deploument describe the state of the application as apod template (componenet) should have when 
deployed.

	Example of a definition of a deployment
	-----
	apiVersion: apps/v1
	kind: Deployment 1
	metadata:
	  name: hello-openshift 2
	spec:
	  replicas: 1 3
	  selector:
		matchLabels:
		  app: hello-openshift
	  template: 4
		metadata:
		  labels:
		    app: hello-openshift
		spec:
		  containers:
		  - name: hello-openshift 5
		    image: openshift/hello-openshift:latest 6
		    ports: 7
		    - containerPort: 80
		    
	Explanation for the deployment definition
	-----
	#1: The resource kind is set to Deployment.
	#2: Name of the deployment resource.
	#3: Number of running instances.
	#4: Section to define the metadata, labels, and the container information of the deployment resource.
	#5: Name of the container.
	#6: Resource of the image for creating the deployment resource.
	#7: Port configuration, such as the port number, the name of the port, and the protocol.


#### ProjectS

Openshift add projects to Kubernetes namespace to enhance the function. A project is a Kubernetes namespace
with additional annotations and is the primary way to manage access to resources for regular users. 
Project can be created from templates and must use RBAC for orginization and permission management.
Administrators grants cluster users access to a project. If a cluster user is allowed to create projects
then they are automatically have acces to create projects.

Projects provide logical and organizational isolation to separate your application component resources.
Resources in one project can access resources in other projects, but not by default. 

	Example of a definition of a project
	-----
	apiVersion: project.openshift.io/v1
	kind: Project #1
	metadata:
	  name: test #2
	spec:
	  finalizers: #3
	  - kubernetes

	Explanation for the project definition
	-----
	#1: The resource kind is set to Project.
	#2: Name of the project.
	#3: A finalizer is a special metadata key that tells Kubernetes to wait until a specific condition
		is met before it fully deletes a resource.


#### Services

A service is used to configure internal pod-to-pod network communication in OpenShift. Applications sends
request to the service name and port. OpenShift provides a virtual network that reroutes such requests
to the pods that the service target by using labels.

	Example of a definition of a service
	-----
	apiVersion: v1
	kind: Service #1
	metadata:
	  name: docker-registry #2
	  namespace: test #3
	spec:
	  selector:
		app: MyApp #4
	  ports:
	  - protocol: TCP #5
		port: 80 #6
		targetPort: 9376 #7 

	Explanation for the service definition
	-----
	#1: The resource kind is set to Service.
	#2: Name of the service.
	#3: Project name where the service resource exists.
	#4: The label selector identifies all pods with the attached app=MyApp label and adds the pods to
		the service endpoints.
	#5: Internet protocol is set to TCP.
	#6: Port that the service listens on.
	#7: Port on the backing pods, which the service forwards connections to.


#### Persistent Volume Claims (PVC)

OpenShift uses Kubernetes persistent volume (PV) frameworks to enable cluster administratiors to provision
persistent storage for a cluster. Developers can use a PVS to request a PV resource without having specific
knowledge of the underlying storage infrastructure. After a PV is bounded to a PVS that PV can not be
bounded to any additional PVCs. PVS are namespaced objects and PV are globally scoped objects. This binding
effectively scopes a bound PV to a singel namespace until the PVC is deleted.

	Example of a definition of a persistent volume claim
	-----
	apiVersion: v1
	kind: PersistentVolumeClaim #1
	metadata:
	  name: mysql-pvc #2
	spec:
	  accessModes:
		- ReadWriteOnce #3
	  resources:
		requests:
		  storage: 1Gi #4
	  storageClassName: nfs-storage #5
	status: {}

	Explanation for the pvc definition
	-----
	#1: The resource kind is set to PersistentVolumeClaim.
	#2: Name of the service.
	#3: The access mode, to define the read/write and mount permissions.
	#4: The storage of the PVC.
	#5: Name of the StorageClass that the claim requires.


#### Secrets

Secret provides a way to hold sensitive information, passwords, credentials, configuration files, ssh
keys, ... A secret is mounted into the container using a volume plugin. Kubernetes can also use secrets to
perform actions, as on pods, declare environmental variables. Secret can store any type of data. Kubernetes and OpenShift
support different types of secrets, such as service account tokens, SSH keys and TLS certificates.

	Example of a definition of a secret
	-----
	apiVersion: v1
	kind: PersistentVolumeClaim #1
	metadata:
	  name: mysql-pvc #2
	spec:
	  accessModes:
		- ReadWriteOnce #3
	  resources:
		requests:
		  storage: 1Gi #4
	  storageClassName: nfs-storage #5
	status: {}

	Explanation for the secret definition
	-----
	#1: The resource kind is set to Secret.
	#2: Name of the service.
	#3: Project name where the service resource exists.
	#4: Specifies the type of secret.
	#5: Specifies the encoded string and data.
	#6: Specifies the decoded string and data.


### Managing Resources from the Command Line

Many Kubernetes and OpenShift commands can create or modify cluster resources. Some commands are part
of the core Kubernetes where others are exclusive additions to OpenShift.

Resouce management falls into two categories

- An *imperative* command instructs what the cluster does.
- A *declarative* command defines the state that the cluster attempts to match.


#### Imperative Resource Management

Using the *create* option is the imperative ways to create resiurces and is included in both the 'oc'
and 'kubectl' command.

	'oc create deployment my-app --image example.com/my-image:dev' -> "deployment.apps/my-app created"
	The imperative command to create a deployment named "my-app" that will create pods based on a specific
	image.
	
	'oc set env deployment/my-app TEAM=red' -> "deployment.apps/my-app updated"
	Using the set options to define attributes on a resouce like environmental variables, like "TEAM=red"
	environmental variable in a existing deployment.

You can also use the 'run' option to create a resouce using the imperative approach

	'Example of using the 'run' option the imperative way to create the "example-pod"
	'oc run example-pod \ #1
	--image=registry.access.redhat.com/ubi8/httpd-24 \ #2
	--env GREETING="Hello from the awesome container" \ #3
	--port 8080' #4
	
	Explanation for the secret definition
	-----
	#1: The pod .metadata.name definition
	#2: The image for the single container in this pod
	#3: The environment variable for the single container in this pod
	#4: The port metadata definition

The imperative way commands are faster way of creating pods becase no pod object definition is required.
Versioning and incremental changes require declarative manifests.

Generally developers test a deployemnt by using imperative commands and use them to generate the pod
definitions. Using the "--dry-run=client" flag to avoid the object being created in OpenShift. Also using
the output (Yaml or JSON) to define a definition format.

	Example of a create a Yaml definition using output from a imperative command
	-----
	'oc run example-pod \
	--image=registry.access.redhat.com/ubi8/httpd-24 \
	--env GREETING='Hello from the awesome container' \
	--port 8080 \
	--dry-run=client -o yaml' -> example-pod.yaml
	
	example-pod.yaml
	-----
	apiVersion: v1
	kind: Pod
	metadata:
	  creationTimestamp: null
	  labels:
		run: example-pod
	  name: example-pod
	spec:
	  containers:
	...

	Creating the definition is imperative but then using the definion is declarative


#### Declarative Resource Management

Using the 'create' option and providing a manifest where the resource is defined is declarative way.

	'oc create -f my-app-deployment.yaml' -> "deployment.apps/my-app created"
	The declarative way, using a manifest file container the resources

OpenShift adds the 'new-aåå' option which is another way of declarative way to create resouces. A way
to determin which types of resources to create based on the specified parameters.

	'oc new-app --file=./example/my-app.yaml' -> 
		...
		Creating resources ...
		imagestream.image.openshift.io "my-app" created
		deployment.apps "my-app" created
		services "my-app" created
		...
	
	Deploys the my-app application by creating several resources, including a deployment resource, from
	a YAML manifest file

In both 'cresate' and 'new-app' options you declare the intended state of the resources, what the state
should be when all have been deployed.

The 'new-app' option can be used with template for resource managemant. The template tells the intended
state of the resourcethat muste be created before for an applicaion to run, such as deployments and services.
Supplying the template using the 'new-app' option is a form of declarative resource management.

The 'new-app' options also includes flags such as "--param" that can customize an appliaction deployment
declarative. 

	'oc new-app --template mysql-persistent \
 --param MYSQL_USER=operator --param MYSQL_PASSWORD=myP@55 \
 --param MYSQL_DATABASE=mydata \
 --param DATABASE_SERVICE_NAME=db' ->
		Deploying template "db-app/mysql-persistent" to project db-app
		...
		The following service(s) have been created in your project: db.
		  Username: operator
		  Password: myP@55
		  Database Name: mydata
		  Connection URL: mysql://db:3306/
		...
		* With parameters:
		  * Memory Limit=512Mi
		  * Namespace=openshift
		  * Database Service Name=db
		  * MySQL Connection Username=operator
		  * MySQL Connection Password=myP@55
		  * MySQL root user Password=tlH8BThuVgnIrCon
		  ...
		Creating resources ...
		  secret "db" created
		  service "db" created
		  persistentvolumeclaim "db" created
		Success
		...

	When the 'new-app' is used with a template you can include the "--param" flag to override a parameter
	value in the template

Simular to the 'create' option you can use the 'new-app' flag imperatively. When using a conatiner image
with the 'new-app' you instruct the cluster what to do rather then declaring the intended outcome.

	'oc new-app --image example.com/my-app:dev' -> 
		...
		Creating resources ...
		imagestream.image.openshift.io "my-app" created
		deployment.apps "my-app" created
		...
		
	Deploys the "example.com/my-app:dev" imageby createing severla resources, including a deployment

You are also able to use a Git repository with the "new-app" command

	'oc new-app https://github.com/apache/httpd.git#2.4.56' ->
		...
		Creating resources ...
		imagestream.image.openshift.io "httpd24" created
		deployment.apps "httpd24" created
		...

	Create an application named "httpd24" by using a Git repository


### Retrieving Resource Information

You can supply all arguments to commands to specify a list of common resource types. But all options
does not inslude every reouse type. Instead all is a shorthand for a predefined subset of types. When you
use this command argument ensure that all includes any intended types to address.

You can get detailed information about a resource, such as defined parameter, by using the 'describe' option.

	'oc describe template mysql-ephemeral -n openshift' ->
		Name:		mysql-ephemeral
		Namespace:	openshift
		...
		Parameters:
		  Name:		MEMORY_LIMIT
		  Display Name:	Memory Limit
		  Description:	Maximum amount of memory the container can use.
		  Required:		true
		  Value:		512Mi

		  Name:		NAMESPACE
		  Display Name:	Namespace
		  Description:	The OpenShift Namespace where the ImageStream resides.
		  Required:		false
		  Value:		openshift
		  ...
		Objects:
		  Secret				                        ${DATABASE_SERVICE_NAME}
		  Service				                        ${DATABASE_SERVICE_NAME}
	
	OpenShift provides template int eh "openshift" project to use with the 'oc new-app' command.
	The above displays detailed infomration about the "mysql-ephemeral" template.

The 'oc describe' command can not generate a structured output like Yaml or JSON formats. A structured
format is required for the 'ic desrbie' command to parse or filter the output with tools auch as "JSONPath"
or "Go template". Instead use the 'oc get' command to generate and to parse the structured output of
a resource.

---

## MANAGE LONG-LIVED AND SHORT-LIVED APPLICATIONS BY USING THE KUBERNETS WORKLOAD API

### Kubernetes Workload Resources

The Kuberenetes workdloads API consists of resouces that represent various types of cluster tasks, such
as jobs and workloads, The Workload APIs is composed of various Kubernetes APIs and extensions that simplify the
deployment and management of applications.

Workload API resource types that are ofter used

- Jobs
- CronJobs
- Deployments
- DeamonSets
- StatefulSets

#### Jobs

A *job* resource is a ont-task* to peform on the cluster, jobs are executed as pods. If the job's pod
fails then the cluster retries a number of times that the job defines. If the job succeed it will not
restart.

A *job* differ from using the *'oc run'* command that only creates a pod. The pod is not managed by
high-level controllerar. If the node where the pod is running fails the pod will not be rescheduled, the
pod is not automatically resytarted if its process exists with an error code.

Common uses for jobs

- Initializing or migrating a database
- Calculating one-off metrics from information within the cluster
- Creating or restoring from a data backup

	Example of a command creates a job that logs the date and time
	-----
	'oc create job \ #1
	date-job \ #2
	--image registry.access.redhat.com/ubi9/ubi \ #3
	-- /bin/bash -c "date"' #4
	
	Explanation for the job command
	-----
	#1: Creates a job resource.
	#2: Specifies a job name of date-job.
	#3: Sets registry.access.redhat.com/ubi9/ubi as the container image for the job pods.
	#4: Specifies the command to run within the pods.

A job can also be created in the OpenShift web console in "Workloads -> Jobs", then click "Create Jobs"
to customize the Yaml manifest.

 
#### Cron Jobs

A *cron job* resource builds a regilare job by specifing of often the job should be run. *Cron jobs* are
useful to create periodic jobs that reoccur, such as backupjobs or report generation. Cron jobs also
schedule individual tasks for a specific time, when there is low activity period. Simulare to the crontab
on Linux system the CronJob resource uses Cron format to schedule cron jobs. A cronJob resource creates
a Job resource based on the time zone configureed on the control plane node that runs the cron job controller.

In OpenShift you specify the time zone for cronjobs schedule to ensure correct timing regardless of
the nodes locations.

	Example of a command creating a cronjob that prints the system time every minute
	-----
	'oc create cronjob date-cronjob \ #1
	--image registry.access.redhat.com/ubi9/ubi \ #2
	--schedule "*/1 * * * *" \ #3
	-- date' #4
	
	Explanation for the cronjob command
	-----
	#1: Creates a cron job resource with a name of date-cronjob.
	#2: Sets the registry.access.redhat.com/ubi9/ubi as the container image for the job pods.
	#3: Specifies the schedule for the job in Cron format.
	#4: The command to execute within the pods.

A cronjob can alos be created using the OpenShift web console, "Workloads -> CronJobs" menu and click
"Create CronJob" to customize the Yaml manifest.


#### Deployments

A deployment creates a *replica set* to maintain pods. A *Replica set* maintain the specific number of
replicas of a pod. Replicasets uses selectors, labels to identify pods that are part of the set. Pods
are created or removed until the replicas reached the number that the deployment specifies. Replica 
sets are not managed directly but throuugh deployments that indirectly manage replica set.

Unlike a Job deployment's pods are re-created after a creash or deletions, because of the usage of replica
sets.

	Example of a command creating a deployment (which contains a replica set)
	-----
	'oc create deployment \ #1
	my-deployment \ #2
	--image registry.access.redhat.com/ubi9/ubi \ #3
	--replicas 3 #4
	
	Explanation for the deployment command
	-----
	#1: Creates a deployment resource.
	#2: Specifies my-deployment as the deployment name.
	#3: Sets registry.access.redhat.com/ubi9/ubi as the container image for the pods.
	#4: Sets the deployment to maintain three instances of the pod.

OpensShift web console can create a deployment, "Workloads -> Deployments" menu and click "Create Deployment"
and customize the form or Yaml manifest.

Pods in a replica set are identified and match the pod template in the replica set definition. If the
number is not met then an new pod gets deleted or created.

Labels are a type of resource metadata that are reprented as sting key-value pair. A label indicates a
common trait from resources with that label. AS an exaple you might attach "layer=frontend" to resources
that relates to an applicaions UI component.

Many 'oc' commands accepts a lable to filter affected resources.

	'oc delete pod -l environment=testing'
	Deleting all pod with the label "environment=testing"

By applying labels to resources you can easy cross-referance resources and craft precise selectors.
A selector us a query object that describes the attributesof matchning resources.

Certain resources us selectors to find other resources

	Example of a replica set that uses selecotsor to match its pods.
	-----
	apiVersion: apps/v1
	kind: ReplicaSet
	...
	spec:
	  replicas: 1
	  selector:
		matchLabels:
		  app: httpd
		  pod-template-hash: 7c84fbdb57
	...


#### Daemonsets

A daemon sets ensures that all or some nodes run a copy of a pod. As nodes are added to the cluster pods
are added to them. As nodes are removed from the cluster, those pods are garbage collected. Delete the
daemon set to clean up the pods it created.

Common uses for daemon sets inslude running storage daemons, lg collectors or monitoring agents on every
node.

	Example of a command creating a daemin set
	-----
	'oc create daemonset my-daemonset \ 1
--image registry.access.redhat.com/ubi9/ubi' #2
	
	Explanation for the daemon set command
	-----
	#1: Creates a daemon set resource.
	#2: Sets registry.access.redhat.com/ubi9/ubi as the container image for the pods.

A daemon set can also be created in OpenShift web console, "Workloads -> DaemonSets". Click "Create DaemonSet"
and customize the Yaml manifest.


#### Stateful Sets

Like deployments, stateful sets manages a set of pods based on the container specifications. But each
pod that are created by the stateful set is unique. Pod uniqueness is useful when a pod needs a unique
network identifier or persistent storage.

As their names implies stateful sets are for pods that require a state within the cluster.
Deployment are used for stateless pods.

---

## KUBERNETES POD and SERVICE NETWORKS

### The Software-defined Network

Kubernetes uses software-defined networking (SDN) by Open Virtual Network (OVN) Kubernetes to manages
the network infrastructure of the cluster. OVN-Kubernetes creates an virtual network that includes all
cluster nodes. The virtual network enables communication between any container or pod within the cluster.
Cluster node processes that Kubernetes pods manage have access to the OVN-Kubernetes network. But the
OVN-Kubernetes network is not accessible from outside of the cluster or by regular processes on cluster
nodes.

Using the OVN-Kubernetes model you can manage network service through the abstraction of multiple layers.
Upi can also managenetwork traffic and network resources programmatically the enable orginization teams
to determine how to expose their applications.

The OVN-Kubernetes implementation creates a model that is compatible with traditional networking practices.
The model makes pods that are similar to virtual machines in terms of port allocation, IP address leasing,
and reservation.

With OVN-Kubernetes design  you do not need to modify how applications components communicate with each
other. The OVN-Kubernetes design assists in containerizing legacy applications. If your application
consists of multiple services that communicate over the TCP/UDP stack, then this approach remains
effective because containers within a pod use the same network stack.

The following diagram illustrates how all pods connect to a shared network:

![From REDHAT Academy](openshift/network-sdn-pod-network.svg)

Among the features of OVN-Kubernetes, open standards enable vendors to propose solutions for centralized
management, dynamic routing, and tenant isolation.


### Kubernetes Networking

In Kubernetes networking gives an scalable method for communication between containers and offers the
following.

- Highly coupled container-to-container communications
- Pod-to-pod communications
- Pod-to-service communications
- External-to-service communication

Kubernetes automatically assigns IPs to each pod. The IPs are only valid during the life of the pod.
Pods gets created and deleted across the cluster. An example would be when you role out a new version
of the application that exists in multiple pods. Kubernetes will deploy new pods to replace the old ones.

All containers within a pod share network resources (and storage resources). The IP and MAC address that
are assigned to the pod are shared among all containers in the pod. This gives that every container can
access each other ports using loopback, *localhost*. Ports that are bound to localhost are available to
all containers within the pod but not to anyone outside of the pod.

Default pods are able to communicate with each other even if they run on different cluster nodes or
belong to different Kubernetes namespace. Every pod is assigned an IP address in a flat shared networking
namespace. that has full communication with other physical computers and containers acress the network.
All pods are assigned an unique IP address from a Classless Inter-Domain Routing (CIDR) range of host
addresses. The shared address range places all pods in the same subnet.

When all pods are on the same subnet pods on all nodes can communicate with pods on any other node without
the help of Network Address Translation (NAT). Kubernetes also provides a service subnet which links
the stable IP addresses of a service resource to a set of specific pods. The traffic is forwarded in
a transparent way to the pods, an agnent, which OVN-Kubernetes manages, handeles routing rules to direct
traffic to pods that match the service resource selectors. Pods functions more like virteual machines
or to physical hosts when it comes to port allocation, networking, naming, service discovery, load balancing,
application configuration and migration.

The following illustration gives further insight into how the infrastructure components work along with
the pod and service subnets to enable network access between pods inside an OpenShift instance.

![From REDHAT Academy](openshift/sdn-relation.svg)

The shared networking namespace pf pods enables a straightforward communication model. But the dynamic
nature of pods is a problem. Pods can be added (scaled up) on the fly to handle more traffic and pods
can also be removed (scaled down). If a pod fails Kubernets automatically replaces the pod with a new
one. These events changes the IP addresses of the pods.

![From REDHAT Academy](openshift/multicontainer-design-lecture.svg)

In the above picture the "before" shows a fron-end container that is running in a pod with IP 10.8.0.1.
The container also shows a back-end container that is running in a pod with IP 10.8.0.2. Something happens
to the back-end container causes it to fail. In response to the pod failing Kubernetes now creates a
new pod to replace the nack-end container but the new pod gets a new IP 10.8.0.4.

In the above picture the "after"  the front-end container now has an invalid refrence to the back-end
container because of the IP change. Kubernetes resolves this problem with service resources.


### Using Services

Container inside pods must not directly connect to each oters dynamic Ips. Instead Kubernetes assigns
a stable IP address to a serice resource that is linked to a set od specified pods. The service acts
as a virtual network load balancer for the pods that are linked to it.

If pods are restarted, replicated or rescheduled to a different nodes the the service endpoints are
updated. The updated endpoints provide scalability and fault tolerance for the application. Unlike IPs
for pods the IPs for services do not change. Service stability provides scalability and fault tolerance
for the application.

![From REDHAT Academy](openshift/multicontainer-fail-with-service.svg)

In the above picture the "before" shows that a front-end reference a spable IP of the back-end service,
instead of the IP of the back-end container. When the back-end container fails a new back-end container
will be created by Kubernetes to replace the failed pod. In response to the change Kubernetes removes 
the failed pod from the service endpoints. Kubernetes then adds the Ip of the new back-end pod to the
service endpoints.

With the service requests from the fron-end container to the back-end container keeps working. The service
dynamically updates with the IP chnage. A service provides a permanent statis IP address for a group of
pods that belong to the same deployment or replica set for an applicaion. Until it is deleted the assigned
IP will not chnage and the cluster does not reuse it.

Most application do not run as a single pod. Applications needs to scale horizontally. Multiple pods
run the same container to meet growing user demand. A deployment resource manages multiple pods that
runs the same container. A service provide a single IP address for the entire set. The service performs
load balancing for client requests among member pods.

With services, containers in a pod can establish network connections to containers in other pod. The pods
that the service tracks do not need to exists on the same compute node or in the same namespace or project.
Because a service provides a stable IP address for other pods to use a pod does not need to discover
the new IP address for another pod after a restart. The service provides a stable IP address to use no
matter which compute node runs the pod after restart.

![From REDHAT Academy](openshift/network-sdn-service-network.svg)

The service object provides a stable IP address for the client container on "Node X" The stable IP address
enables the client to send request to any of the API containers.

Kubernets uses labels on pods to select those pods that are associated with a service. To include a pod
in a service the pod labels must include each of the selector fields of the service.

![From REDHAT Academy](openshift/network-sdn-service-selector.svg)

In the above example the selector has a key-value pair "app: myapp". The pods with a matching label of
"app: myapp" are included in the set that are associated with the service. The selector attribute of
a service identifies the set of pods that from the endpoints for the service. Each pod in the set is an
endpoint for the service.

To create a service for a deployment use the 'expose' option.

	'oc expose deployment/<deployment-name> [--selector <selector>]
	[--port <port>][--target-port <target port>][--protocol <protocol>][--name <name>]'
	
The 'oc expose' uses the "--selector" flag to specify label selecors. When it is omitted a selector that
matches the replication controller or replica set is used. 

The "--port" flag specifies the port that the service listens on. This port is availbe only to pods	within
the cluster. If a port is not provided the the port is copied from the configuration of the deployment.

The "--target-port" flag specifies the name or number of the container port that the service uses to
communicate with the pods. If a port value is not provided the the port is copied from the configuration
of the deployment.

The "--protocol" flag determin the network protocol for the serveice, TCP is used by default.

The "--name" flag names the service, if no name is provided then the service usesthe same name as the
deployment.

	'oc get service db-pod -o wide' ->
		NAME    TYPE        CLUSTER-IP      EXTERNAL-IP PORT(S)     AGE     SELECTOR
		db-pod  ClusterIP   172.30.108.92   <none>      3306/TCP    108s    app=db-pod
	
	To view the selector for a servie you must use the wide output.
	
	'oc get endpoints' -> 
		NAME     ENDPOINTS                       AGE
		db-pod   10.8.0.86:3306,10.8.0.88:3306   27s
	
	See the endpoints that a service uses.	
	The name of the service is "db-pod" and pods must use "app=db-pod" label (see above) to be included
	in the host list for the "db-pod" service
	
	'oc describe deployment db-pod' ->
		Name:                   db-pod
		Namespace:              deploy-services
		CreationTimestamp:      Wed, 18 Jan 2023 17:46:03 -0500
		Labels:                 app=db-pod
		Annotations:            deployment.kubernetes.io/revision: 2
		Selector:               app=db-pod
		...
	
	To see the deployment selector use 'oc describe deployment ...'
	
	'oc get deployment/db-pod -o yaml'
		...
		  selector:
		    matchLabels:
		      app: db-pod
		...
	
	To get the output in Yaml format and the deployment resource from the spec.selector.matchLabels object.


### Kubernetes DNS for Service Discovery

Kubernetes uses an Internal Domain Name System (DNS) server that the DNS operator deploys. The DNS operator
creates a default cluster DNS name and assigns DNS names to services that are defined. The DNS operator
implements the DNS API from the operator.openshift.io API group. The operator deploys CoreDNS, creates
the service for CoreDNS and configures the kubelet component to instruct pods pods to use the CoreDNS
service IP address for name resolution. When a service does not have a cluster IP the DNS operator assigns
a DNS record that resolves to the set of IP addresses of the pods behind the service.

The DNS server discovers a service from a pod by using the internal DNS server that are only visable
for pods. Each service is dynamically assigned a Fully Qualified Domain Name (FQDN) that uses the format:
"SVC-NAME.PROJECT-NAME.svc.CLUSTER-DOMAIN"

	'cat /etc/resolv.conf' ->
		nameserver 172.30.0.10
		search deploy-services.svc.cluster.local svc.cluster.local cluster.local
		options ndots:5

	When a pod is created Kubernetes provides the resolv.conf file.
	"deploy-services" is the project name for the pod
	"cluster.local" is the cluster domain

The nameserver directive provides the IP address of the Kubernerts internal DNS server. The "options ndots"
specifies the number of dots that must be in a name to qualify for an initial absolute query. Alternative
hostname values are derived by appending values from the search directive to the name that you send 
to the DNS server.

In the "search" directive the "svc.cluster.local" enables any pod to communicate with another pod using
the service name and project name, "SVC-NAME.PROJECT-NAME".

The first entry in the "search" directive enables a pod to use the service name to specify another pod
in the same project. In OpenShift a project is also a namespace for a pod. The service name alone is
enough for pods in the same project, "SVC-NAME"


### Kubernetes Networking Drivers

Container Network Interface (CNI) plugins gives a common interface between the network provider and 
the container runtime. CNI defines the specifications for plugins that configure network interfaces
inside containers. Plugins are written to the specification enable different network providers to control
the OpenShoft cluster network.

Red Hat provides the following CNI plug-ins for a RHOCP cluster

- OVN-Kubernetes: The default plug-in for first-time installations of RHOCP 4.10 and later versions.
- OpenShift SDN: An earlier plug-in from RHOCP 3.x; it is incompatible with some later features of RHOCP 4.x.

Certified CNI plugins from other vendors are also complatiable with OpenShift cluster.

The SDN uses CNI plugin to create Linux namespaces to partition the usage of resources and processes 
on virtual and physical hosts. With the implementation containers inside pods can share resources such
as devices, IP stacks, firewall rules and routing table. The SDN provide a unique routable IP to each
pod so the pod is accessable from any oter service in the same network.

In OpenShift 4.18, OVN-Kubernetes serves as the default network provider and OpenShift SDN is deprecated.

OVN-Kubernetes uses OVN to manage the cluster network. A cluster that uses the OVN-Kubernetes plugins
also runs Open vSwitch (OVS) on each node. OVN configures OVS on each node to implement the declared
network configuration.


#### The OpenShift Cluster Network Operator

OpenShift provide a Cluster Network Operator (CNO) that configures OpenShift cluster networking. The CNO
serves as an OpenShift cluster operator that loads and configures CNI plugins.

	'oc get -n openshift-network-operator deployment/network-operator' ->
		NAME              READY   UP-TO-DATE  AVAILABLE   AGE
		network-operator  1/1     1           1           41d
		
	As a cluster admin you can observe the status of the CNO

An administrator configures the CNO at installation time

	'oc describe network.config/cluster' ->
		Name: cluster
		...
		Spec:
		  Cluster Network:
		  Cidr: 10.128.0.0/14 #1
		  Host Prefix: 23
		  External IP:
		    Policy:
		      Network Type: OVNKubernetes
		      Service Network:
		        172.30.0.0/16 #2
	
	Explain the output from the abobe command
	-----
	#1: The Cluster Network CIDR defines the range of IP addresses for all pods in the cluster.
	#2: The Service Network CIDR defines the range of IP addresses for all services in the cluster.
	
	Use the above command to view the configuration.

---

## SCALE AND EXPOSE APPLICATIONS TO EXTERNAL ACCESS

### IP Addresses for Pods and Services

Application might need multiple pods to scale horixintally to meet growing demands. OpenShift deploys
many pods with the same conytainer from a single pod resource definition. A service resource defines a
single IP and port combination. The resource assign a single IP to the stable IP ta a pool of pods.
The resource balances client request to all pods.

OpenShift assigns each service an unique IP for client to connect to. Round-Robin is used for load balancing.
The service IP comes from internal OVN-Kubernetes network which is seperate from the podntwork and only
accessible to pods within the cluster. OpenShift adds each pod to match the selecor of the service as an endpoint.

### Service types

OpenShift offers several different service types to mmet the needs of an applicaion, cluster infrastructure
and security requirements.

- ClusterIP  
	The ClusterIP type which is the default type. Assigns a cluster internal IP to a service. The assignment
	makes the service accessible only within the cluster.
	
	ClusterIP service ebales pod to pod communication. OpenShift gets Ips from a dedicated service network.
	The network accessable only inside of the network supports internal communication. Kubernetes automates
	management of the service.
	
- LoadBalancer  
	The LoadBalancer type OpenShift to provision a load balancer of the cloud provider. An external accessible
	IP is assigned to provide access to the applicaion.
	
	When deploying a LoadBalancer services a cost will occur when assigning the service to every applicaion.
	The service are exposing applicaions to external networks. Additional security configuration is required
	to prevent unauthorized access.
	
- ExternalIP  
	The ExternalIp service redirect traffic from the virtual IP address on a cluster node to a pod. A 
	cluster administrator assigns the virtual IP address to the node and confifures failover to another
	node if needed.
	
	OpenShift configure NAT rules to route traffic from the virtual address to the pod. Administrators
	must make sure that the external IP is routed to the node. Additional security measures protect 
	the cluster from external access.
	
	WARNING!!!  
	Starting with RHOCP 4.18, ExternalIP is a legacy feature due to security concerns. Direct network
	connections to cluster nodes, which this service requires, conflict with most security policies.
	Use the LoadBalancer or the NodePort type unless the ExternalIP type meets specific requirements.
	
- NodePort  
	Exposes service exposes a service on a specific port, default range 30000-32767 on the IP of each
	cluster node. Each node redirect traffic from this port to the endpoints of the service.
	
	WARNING!!!  
	NodePort services require direct network connections to cluster nodes, which poses a security risk.
	Implement strict firewall rules and access controls to mitigate risks.
	
- ExternalName  
	The ExternalName service map a Kubernetes service to an external DNS name that is soecified in the
	"externalName" field. The Kubernetes DNS server return the EexternalName" in a CNAME record. The
	record directs clients to resolve the external name to an IP address.

### Using Routes for External Connectivity

OpenShift provides *Route* resources to expose applications to external networks. A route resource supports
HTTP, HTTPS, TCP an oter protcols. It prioritize HTTP and TLS based applicaions for external exposure.
None HTTP applicaions such as databases typically remains internal. *Route* and *Ingress* resources handle
ingress traffic i OpenShift.

A *Route* resource assigns a public accessible hostnmae to an application. The OpenShoft ingress controller, 
HAProxy based, redirect traffic from a public IP address to pods. The OpenShift ingress controller is 
the default but support for third-party ingress controllers that are deployed in parallel. *Route* resources
offer advanced features that is better then Kubernetes I*Ingress* capabilities. Features include TLS 
re-encryption, passthrough and split traffic for blue-gree deployments.

	'oc expose service api-frontend --hostname api.apps.acme.com'
	Create a route using the oc CLI
	
	The "--target-port" flag with the 'oc expose' command specifies the name or number of the port container
	that is uses to communicate with the pods. If no port value is provided then the port is copied
	from the deployment

	Not using "--hostname" flag makes OpenShift to generate a hostname in
	"<route-name>-<project-name>.<default-domain>" format, a "frontend" route in an "api" project with a 
	wildcard domains of "apps.example.com" becomes " frontend-api.apps.example.com".

IMPORTANT!!!  
The DNS server for the wildcard domain resolves all names to configured IPs. The server does not recognize
specific route hostnames. The OpenShift ingress controller treats route hostnames as HTTP virtual hosts.
Invalid or non-existent route hostnames trigger an HTTP 503 error.

To create a route configure the following steps

1. Service Name: Specifies the service to determine target pods.
1. Hostname: Sets the route hostname as a subdomain of the cluster wildcard domain, which RHOCP can autogenerate.
1. Path: Defines an optional path for path-based routing.
1. Target Port: Matches the targetPort in the service, and specifies the listening port of the application.
1. Encryption Strategy: Chooses secure (TLS) or insecure routing.

	An examplr of a minimal route definition
	-----
	kind: Route
	apiVersion: route.openshift.io/v1
	metadata:
	  name: a-simple-route #1
	  labels: #2
		app: API
		name: api-frontend
	spec:
	  host: api.apps.acme.com #3
	  to:
		kind: Service
		name: api-frontend #4
	  port:
		targetPort: 8443 #5
		
	Explaination to the route definition
	-----
	#1: Specifies a unique route name.
	#2: Defines selectors for the route.
	#3: Sets the hostname, which is a subdomain of the cluster wildcard domain.
	#4: Identifies the service to redirect traffic to, by determining target pods.
	#5: Maps the router port to the endpoint port of the service.

NOTE!!!
Some ecosystem components integrate with Ingress resources but not with Route resources. Starting with
RHOCP 4.18, creating an Ingress object automatically generates a managed Route object, which is deleted
when the Ingress object is removed.

	'oc delete route a-simple-route'
	To delete route

It is also possible to create a route using OpenShift web console. "Networking -> Routes", click "Create Route"
customize the name, hostname, path and target service.


### Using Ingress Objects for External Connectivity

A Kubernetes *Ingress* reource provides a simular function as the OpenShift route resouce. It supports
HTTP, HTTPS, Server Name Indication (SNI) and TLS with SNI. The OpenShift ingress controller processes
Ingress objects and enables features such as TLS temination, path redirect and sticky sessions.

NOTE!!!  
Note
Although Ingress resources are standard in Kubernetes, Route resources are preferred for advanced features
and native integration with the OpenShift ingress controller.

	'oc create ingress ingr-sakila --rule="ingr-sakila.apps.ocp4.example.com/*=sakila-service:8080"'
	Create an Ingress object
	
	Example of a minimal Ingress definition
	-----
	apiVersion: networking.k8s.io/v1
	kind: Ingress
	metadata:
	  name: frontend #1
	spec:
	  rules:
	  - host: www.example.com #2
		http:
		  paths:
		  - path: /
		    pathType: Prefix
		    backend:
		      service:
		        name: frontend #3
		        port:
		          number: 80 #4
	  tls:
	  - hosts:
		- www.example.com
		secretName: example-com-tls-certificate #5
	
	Explanation of the minimal Ingress definition
	-----
	#1: Specifies a unique name for the Ingress object.
	#2: Sets the hostname for inbound traffic.
	#3: Identifies the service to redirect traffic to.
	#4: Defines the port for the service back end.
	#5: Configures TLS for secure paths, which requires a matching host and a certificate secret.
	
	'oc delete ingress frontend'
	Delete an Ingress object


### Sticky Sessions

A sticky session ensures that a stateless applicaions traffic reaches the same pod for a session. The
OpenShift ingress controller can use cokkies to manage session persistence for Route and Ingress resources.
The controller selects a pod for the session, the controller generates the cookie, the controller includes
the cookie in the response, tehe clients sends the cookie with subsequent requests which enables the
controller to route traffic to the same pod.

	'oc annotate ingress ingr-example ingress.kubernetes.io/affinity=cookie'
	To configure session persistence for an Ingress object by using a cookie
	
	'oc annotate route route-example router.openshift.io/cookie_name=myapp'
	To configure session persistence for a Route object with a custom cookie named myapp
	
	'ROUTE_NAME=$(oc get route route-example -o jsonpath='{.spec.host}')'
	To capture the route hostname
	
	'curl $ROUTE_NAME -k -c /tmp/cookie_jar'
	To access the route and save the cookie
	
	'curl $ROUTE_NAME -k -b /tmp/cookie_jar'
	To use the saved cookie for subsequent requests, use the following command:

Using the saved cookie ensures that requests reach the same pod.

NOTE!!!  
Cookies cannot be set on passthrough routes because the HTTP traffic is encrypted, and the router does
not terminate TLS or read the contents of the request.


### Load Balance and Scale Applications

Administrators and developers can scale the number of replicas in development to handle traffic surges
or to conserve resources.

	'oc scale --replicas=5 deployment/scale'
	To scale a deployment, increaste of decrease the number of pods to 5

The deployment updates the replica set. The replica set creates or deletes pods to match the intended
replica count.

Avoid modifying a replica set directly, because the deployment controller manages these changes.


#### Load Balance Pods

A kubernetes Service resource functions as a internal load balancer by providing access to a workload
via the service name. The service distributes incomming connections across all replicated pods. The OpenShift
 controller uses the selector of the service to identify the service and endpoints. When both the controller
 and a service provide load balancing OpenShift relies on the controller to direct traffic to pods.
 The controller detects chnages in IPs of the service, the controller update the configuration. Custom
 controllers can propagate API object chnages to external solutions.

The OpenShift ingress controller maps external hostnames. The controller balances service endpoints
over protocols that include distinguishing information, such as HTTP host headers.

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




