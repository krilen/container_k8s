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

---

## The Compose File

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

Information that is defined in a compose file will override the default commands in an container image

	Compose file
	-----
	services:
	  backend:
	    image: quay.io/example/backend
	    ports:
	      - "8081:8080"
	    command: sh -c "COMMAND"
	  db:
	    image: registry.redhat.io/rhel8/postgresql-13
	    environment:
	      POSTGRESQL_ADMIN_PASSWORD: redhat
	-----
	
	In the above compose file the command to run the backend container will override anything that
	is defined in the container image. Also the compose file can configure the enviromental variables
	that are required to start a container.

---

## Start and Stop Containers with Podman Compose

By using the the command 'podman-compose up' you can execute a Compose file and it will create the defined
objects like volumes and networks and start the defined containers.

'podman-compose ...' command, options that can be used

- "up": Start and create the objects.
- "stop": Stop the runing containers 
- "down":  Stop and remove the objects that are defined.
- "ps": Get the running containers
- "logs": Get the logs

Flags that can be used with the options

- "-d" / "--deattach": Start in the background
- "--force-recreate": Re-create containers on start.
- "-V", "--renew-anon-volumes": Re-create anonymous volumes.
- "--remove-orphans": Remove containers that do not correspond to services that are defined in the 
	current Compose file.

If network is not defined in the compose file podman-compose will create a default network, the name
of the network will be name of the directory where the compose file exists and suffix "_default". If
a network name was defined the network would still start with the name of the directory but end with
the name of the network

The naming of containers follows a simulare format DIRECTORY_SERVICE_NUMBER. 

You are able to override the naming convention by explicitly configuring the objects name

	Compose file: /home/student/myfirstcompose/compose.yaml
	-----
	services:
	  web:
		image: nginx
		ports:
		  - "8080:80"
	-----
	
	'podman-compose up -d'
	Starting the compose
	
	'podman network ls' ->
		NETWORK ID    NAME                    DRIVER
		724005aa0527  myfirstcompose_default  bridge
		2f259bab93aa  podman                  bridge

	The network "myfirstcompose_default" was created by podman
	
	'podman ps' ->
		CONTAINER ID  IMAGE         COMMAND      CREATED     STATUS    PORTS     NAMES
		8b2ad79109e9  docker.io...  nginx -g...  4 minut...  Up 4 ...  0.0.0...  myfirstcompose_web_1
	
	The name of the nginx service if the direcory, name of service and a "1".
	
	Defining a network and the container name.
	
	Compose file: /home/student/myfirstcompose/compose.yaml
	-----
	services:
	  web:
		image: nginx
		container_name: primary
		ports:
		  - "8080:80"
		networks:
 	     - first

	networks:
	  first: {}
	-----
	
	'podman-compose up -d'
	
	'podman network ls' ->
		NETWORK ID    NAME                  DRIVER
		298cd2afd53c  myfirstcompose_first  bridge
		2f259bab93aa  podman                bridge
		
	The networks name suffix have changed from "_defulat" to the defined network name
	
	'podman ps' ->
		CONTAINER ID  IMAGE  COMMAND  CREATED  STATUS  PORTS  NAMES
		5062b99e0d9c  ...    ...      ...      ...     ...    primary
	
	Since we defined an explicit name for the container it is that what we see

---

## Networking

The network when using Compose is importat if the network is not defined it will automatically created
when the compose file is executed and it will be DNS enabled by default.

When creating a compose file you can define which container should be attached to which network.
You also need the define each network. If thsi is not done correctly there will be an error when starting.

	Example of a compose file 
	-----
	services:
	  frontend:
		image: quay.io/example/frontend
		networks: #1
		  - app-net
		ports:
		  - "8082:8080"
	  backend:
		image: quay.io/example/backend
		networks: #2
		  - app-net
		  - db-net
	  db:
		image: registry.redhat.io/rhel8/postgresql-13
		environment:
		  POSTGRESQL_ADMIN_PASSWORD: redhat
		networks: #3
		  - db-net

	networks: #4
	  app-net: {}
	  db-net: {}
	-----
	
	Explaination
	#1: The frontend service is part of the app-net network.
	#2: The backend service is part of the app-net and db-net networks.
	#3: The db service is part of the db-net.
	#4: Definition of the networks.

---

## Volumes

Volumes can also be defined when using compose. You do now need to create the volume before the first start
it will automatically be created. When you run the "down" option the volume will REMAIN. The naming of
the volume will follow the previous naming convention in cpmpose.

	Compose file 
	-----
	services:
	  web:
		image: nginx
		ports:
		  - "8080:80"
		volumes:
		  - db-vol:/var/db
		    
	volumes:
	  db-vol: {}
	-----
	
	'podman-compose up -d'
	Starting WITHOUT creating the volume before
	
	'podman volume ls'
		DRIVER      VOLUME NAME
		local       myfirstcompose_db-vol
	
	The volume "myfirstcompose_db-vol" was created by compose
	
	'podamn-compose down'
	
	'podman volume ls'
		DRIVER      VOLUME NAME
		local       myfirstcompose_db-vol
	
	'ls -la /home/student/.local/share/containers/storage/volumes/myfirstcompose_db-vol/_data/"
	The path where the volume was created still exist after compose have been stopped and removed with
	"down"

If you wish to handle the volume by yourself you can define this in the compose file.

	'podman volume create extra'
	'touch /home/student/.local/share/containers/storage/volumes/extra/_data/file'
	
	Compose file
	-----
	services:
	  web:
		image: nginx
		ports:
		  - "8080:80"
		volumes:
		  - extra:/var/extra
		    
	volumes:
	  extra:
		external: true
	-----
	
	'podman-compose up -d'
	
	By setting "external: true" instead of the empty object "{}" where the volumes are defined you 
	want to manage the volume outside of compose
	
	'podman volume ls' ->
		DRIVER      VOLUME NAME
		local       extra
	
	'podman exec myfirstcompose_web_1 ls -l /var/extra/' ->
		-rw-r--r--. 1 root root 0 Dec 12 11:40 file

You can also use bind mount in Dompse by providing the absolute or relative path on the host machine.
Like before when working with bind volumes you must think about SELinux. But you need not define
the volumes when working with bind mounts.

	Compose file
	-----
	services:
	  web:
		image: nginx
		ports:
		  - "8080:80"
		volumes:
		  - /home/student/myfirstcompose/bind.file:/var/tmp/b.file:Z
	-----
	
	'podman-compose up -d'
	
	'podman exec myfirstcompose_web_1 ls -l /var/tmp/' ->
		-rw-r--r--. 1 root root 0 Dec 12 11:50 b.file
	
	The file is now bind mounted in the container.
	
You can also just like before define a folder to be bind mounted.

---

## BUILD DEVELOPER ENVIRONMENTS with COMPOSE

### Podman compose and Podman

You use 'podman-compose' to translate the compose files into Podman CLI cammands. Podamn Compose interacts
directly with Podman and NOT the Podman's API socket. While Docker compose communicates directly with
the Docker deamin which runs as root by using the REST API.

By not directly interact Podman's API socket Podman Compose remove the need to run the Podamn service
that provides the API socket.Becase Podman Compose inteacts with Podman it has better support for rootless
containers than Docker Compose.

Besides being available as a regular package in your distrubution Podman Compose is also avavilable as a Python package.

	'pip install podman-compose'
	Using pip command to install it.
	
	It is also possible to install the latest development version from the Podman Compose GitHub repository
	'pip install https://github.com/containers/podman-compose/archive/devel.tar.gz'

### Multi-container Developer Environments with Compose

With Podman Compose you can configure the services neede to develop your application. All required service
can be placed in a compose file and developers can start these services with 'podman-compose up' command.
Every enviroment will be isolated from each other and not a common developer environment.

For example to develop a backend application you can set up a database and its admin containers in a single
compose file. You are also able to load production data using volume mount.

Sometime congtainers have depenencies on other containers. Like the aove example the admin container
is dependent of the database container. It is possible to make sure that different dependencies are are
meet in the compose file

	Compose file with a depencies
	-----
	services:
	  database:
		image: "registry.redhat.io/rhel9/postgresql-13"
		....
	  database-admin:
		image: "registry.connect.redhat.com/crunchydata/crunchy-pgadmin4:ubi8-4.30-1"
		container_name: "appdev-pgadmin"
		depends_on:
		 - database
	-----
	
	The "database-admin" service (container) is dependent that the "database" service is started before
	by adding "depends_on" and listing the services (containers) 

If the appication depends on external services you could add a "mock" service and provide within it all
of the dependencies needed.

	Compose file with a mock
	-----
	services:
	  ...
	  mock_service:
		image: "MOCK_SERVICE_IMAGE"
		volumes:
		  - ./mocks/SERVICE/ENDPOINTS:TARGET_DIRECTORY:Z
		  - ./mocks/SERVICE/FIXED_RESPONSES:TARGET_DIRECTORY:Z
	  ...
	-----

---

## PODMAN PODS

Podman Pods is an alternative to orchestrate containers on a single host with the usage of Podman Pods.

Podman Pods can use and produce Kuberbetes YAML files, these files are a declarative way to define and
 manage multiple containers and their resources.
 
The 'podman generate kube' command generates a Kubernets Yaml file from an existing Podman containers,
pods and volumes. The 'podman play kube' reads the Kubernetes YAML file and then recreates the defined
resources on a local machine. These command help to ensure that a container that is developed with Podman
can migrate and work in a Kubernetes ecosystem.




































