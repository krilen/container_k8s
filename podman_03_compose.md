# PODMAN COMPOSE

Many applicaions today are developed as microservices, each microservice is it own independent container
image. Because one application can be composed of several containers it might be difficult to manage
the containers as a group.

When developing new microservices developers can implement one of the following strategies

- Create a *mock* for the service that the application requires, such as API endpoint that other microservices
	provide.
- Start the relevant parts of the application on local machine.

It would be ideal to run the full application on the local machine. When that is not possible developers
containerize mock service that approximate production environment.

Podman Compose is an open souce tool that lets you run *Compose files*. A Compose file ua YAML file that
specifies the container to manage with its dependencies.

	Compose file
	-----
	services:
		orders: #1
			image: quay.io/user/python-app #2
		ports:
			- 3030:8080 #3
		environment:
			ACCOUNTS_SERVICE: http://accounts #4
	
	Explain
	-----
	#1: Declare the orders container.
	#2: Use the python-app container image.
	#3: Bind the 3030 port on your host machine to the 8080 port within the container.
	#4: Pass the ACCOUNTS_SERVICE environment variable to the application.

Podman Compose compile with the *Compose Specifications* which are defines a YAML file-based schema to
manage multi-container applications for container run-times.

The Compose files should work with any container run-time some features might depend on specific
implementaions of the runtime. Compose files might be called *docker-compose.yaml*. Docker introduced
the docker-compose.yaml naming convention when they started the Compose project. Docker open sourced
the Compose Specifications which aslo defined the naming convention. Accoring to the Compose Specifications
the preferred name is compose.yaml.

Podman compose became popular for the following uses

- Development enviroments  
	A production envionment can be tested on your local machine by providing applications, databses 
	or cache systems in  a single file.
	
	With one file automation of a complex multi-container deployments can be done.

- Automated testing  
	Several testing suites can be defined anlong with their own configuration all within the same file.
	You might even automate a pipeline environment on a single machine.
	
	Developers can use their application with isolate database and clients. Developers can also debug
	complex issues in their local environment which increases the development speed.
	
	Using Podman Compose in a production environment is not recommended. Podman compose do not support
	advanced features that might be needed, such as load balancing, dirstrubute containers to multiple
	nodes or managing containers on a different nodes.
	
	When a production environment is needed then Kubernetes or OpenShift orchestration solution is a
	better option. Red Hats Openshift can run multicontainer applications on different nodes.

### The Compose File

A Compose file is YAML file that contains the following sections

- version (deprecated): Specifies the Compose version used.
- service: Defines the containers used.
- networks: Defines the networks used by the containers.
- volumes: Specifies the volumes used by the containers.
- configs: Specifies the configurations used by the cobtainers.
- secrets: Defines the secrets used by the containers.

The secrets and configs objects are mounted as a file in the containers.

	WARNING
	In YAML file the number os spaces (tabs) carries meaning.
	
		-----
		version: "3.9"
		services:
		backend: {}
		-----

	The previous YAML snippet carries a different meaning from the following snippet:
	
		-----
		version: "3.9"
		services:
		  backend: {}
		-----

	The backend keyword is preceded by two spaces; therefore the backend object is an attribute of 
	the services object.
	
	
---

## PODMAN PODS

Podman Pods is an alternative to orchestrate containers on a single host with the usage of Podman Pods.

Podman Pods can use and produce Kuberbetes YAML files, these files are a declarative way to define and
 manage multiple containers and their resources.
 
The 'podman generate kube' command generates a Kubernets Yaml file from an existing Podman containers,
pods and volumes. The 'podman play kube' reads the Kubernetes YAML file and then recreates the defined
resources on a local machine. These command help to ensure that a container that is developed with Podman
can migrate and work in a Kubernetes ecosystem.




































