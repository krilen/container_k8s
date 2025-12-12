# CONTAINER FILE

Lab review from RedHat Academy about Podman

This Containerfile probably does NOT work outside Red Hat Academys lab but it is good info.  
Since we do not have access to any of the materials needed.  

But it is a good read

---

## Instructions

Deploy a social media application consisting of multiple containerized components:

- A PostgreSQL database
- A Spring Boot API
- A React UI hosted with NGINX

Use what you have learned throughout this course to create and start a set of containers that host the
application and its resources. The Beeper application consists of a UI and an API. The API uses a PostgreSQL
database to persist user messages.

### Outcomes

You should be able to:

- Run a container from a public image.
- Manage a container's lifecycle.
- Build efficient container images.
- Create and publish container images.
- Manage container communication and Podman networking.
- Troubleshoot containers.
- Use container storage.

### Specifications

- Create two Podman networks called beeper-backend and beeper-frontend that have DNS enabled.
- Create a PostgreSQL database container that matches the following criteria:
	- Use the registry.ocp4.example.com:8443/rhel9/postgresql-13:1 container image.
	- Use beeper-db for the name of the container.
	- Connect the container to the beeper-backend network.
	- Attach a new volume called beeper-data to the /var/lib/pgsql/data directory in the container.
	- Pass the following environment variables to the container:
		- Name: POSTGRESQL_USER, value: beeper
		- Name: POSTGRESQL_PASSWORD, value: beeper123
		- Name: POSTGRESQL_DATABASE, value: beeper
- Create a container image for the Beeper API that matches the following criteria:
	- Use a multi-stage build with a builder image to compile the Java application.
	- The build stage should perform the following actions:
		- Use the registry.ocp4.example.com:8443/ubi8/openjdk-17:1.12 container image. This image 
			uses /home/jboss as the working directory.
		- Copy the contents of the /home/student/DO188/labs/comprehensive-review/beeper-backend host
			directory to the image's working directory. Change the owner to the jboss user.
		- Use the mvn -s settings.xml package command to create the /home/jboss/target/beeper-1.0.0.jar
			binary file.
	- The execution stage should perform the following actions:
		- Use the container image registry.ocp4.example.com:8443/ubi8/openjdk-17-runtime:1.12.
		- Copy the beeper-1.0.0.jar binary file from the builder image to the working directory of the
			final image.
		- Run the java -jar beeper-1.0.0.jar command to start the Beeper API.
		- Tag the image with beeper-api:v1.
- Create a container for the Beeper API that matches the following criteria:
	- Use the beeper-api:v1 image that you created.
	- Use beeper-api for the name of the container.
	- Pass an environment variable to the container called DB_HOST with beeper-db as the value.
	- Connect the container to the beeper-backend and beeper-frontend networks.
- Create a container image for the Beeper UI that matches the following criteria:
	- Use a multi-stage build with a builder image to compile the TypeScript React application.
	- The build stage should perform the following actions:
		- Use the registry.ocp4.example.com:8443/ubi9/nodejs-22:1 container image. This image uses 
			/opt/app-root/src as the working directory.
		- Set root as the user.
		- Copy the contents of the /home/student/DO188/labs/comprehensive-review/beeper-ui host directory
			into the image's working directory.
		- Run the npm install command in the application directory to install build dependencies within
			the image.
		- Run the npm run build command in the application directory to build the application. This
			command saves the built artifacts in the /opt/app-root/src/dist directory.
	- The execution stage should perform the following actions:
		- Use the registry.ocp4.example.com:8443/ubi8/nginx-118:1 container image.
		- Copy the /home/student/DO188/labs/comprehensive-review/beeper-ui/nginx.conf host file into
			the /etc/nginx/ directory of the execution stage.
		- Copy the built application artifacts from the builder stage into the /usr/share/nginx/html
			directory of the execution stage.
		- Use the nginx -g "daemon off;" command to start the application.
		- Tag the image with beeper-ui:v1.
- Create a container for the Beeper UI that matches the following criteria:
	- Use the beeper-ui:v1 image that you created.
	- Use beeper-ui for the container name.
	- Map container port 8080 to host port 8080.
	- Connect the container to the beeper-frontend network.
- With the three containers running and configured correctly, the UI is available at http://localhost:8080.
	- Click New Beep to create a message that is persisted after restarting or recreating the containers.

### Solution

A tip when creating the "Containerfile"s rememer to place your file in folder where the files that you
wish to copy

1. Create podman networks
	
		'podman network create beeper-backend'
		'podman network create beeper-frontend'
	
1. Create a volume (for the next step)
	
		'podman volume create beeper-data'
	
1. Start the database container (beeper-db)
	
		'podman run --rm -d --name beeper-db -e POSTGRESQL_USER=beeper -e POSTGRESQL_PASSWORD=beeper123 \
		-e POSTGRESQL_DATABASE=beeper -v beeper-data:/var/lib/pgsql/data --net beeper-backend \
		registry.ocp4.example.com:8443/rhel9/postgresql-13:1'
		
1. Create the Containerfile for API (beeper-api) image

		Create a Containerfile in the directory that contains the files that you wish to copy
		'touch Containerfile'
	
		Containerfile
		-----
		FROM registry.ocp4.example.com:8443/ubi8/openjdk-17:1.12 as builder
		COPY --chown=jboss . .
		RUN mvn -s settings.xml package
		
		FROM registry.ocp4.example.com:8443/ubi8/openjdk-17-runtime:1.12
		COPY --from=builder /home/jboss/target/beeper-1.0.0.jar .
		ENTRYPOINT ["java", "-jar", "beeper-1.0.0.jar"]
		-----

1. Build a container image for API (beeper-api)
		
		'podman build -t beeper-api:v1 .'
		
1. Start the API container

		'podman run -d --rm --name beeper-api --net beeper-backend,beeper-frontend -e DB_HOST=beeper-db \
		beeper-api:v1'

1. Create the Containerfile for UI (beeper-ui) image

		Create a Containerfile in the directory that contains the files that you wish to copy
		'touch Containerfile'
	
		Containerfile
		-----
		FROM registry.ocp4.example.com:8443/ubi9/nodejs-22:1 AS builder
		USER root
		COPY . .
		RUN npm install && \
			npm run build

		FROM registry.ocp4.example.com:8443/ubi8/nginx-118:1
		COPY nginx.conf /etc/nginx/
		COPY --from=builder /opt/app-root/src/dist /usr/share/nginx/html
		CMD nginx -g "daemon off;"
		-----
		
1. Build a container image for UII (beeper-ui)
		
		'podman build -t beeper-ui:v1 .'
		
1. Start the UI container

		'podman run -d --rm --name beeper-ui --net beeper-frontend -p 8080:8080 beeper-ui:v1'
		
1. All containers should be up and you should be able to go to 'http://localhost:8080'
		
		
		