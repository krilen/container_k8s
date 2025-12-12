# CONTAINER FILE

Building a "Containerfile" from Red Hat Academy and how to optimize it.

This Containerfile probably does NOT work outside Red Hat Academys lab but it is good info.

---

## Instructions

1. Create a Containerfile for the hello-server Node.js application.

	A. Create a file called Containerfile with the following contents:

		FROM registry.ocp4.example.com:8443/redhattraining/podman-ubi9.5
		
		COPY . /tmp/hello-server
		
		RUN dnf module enable -y nodejs:22
		
		RUN dnf install -y nodejs npm
		
		RUN cd /tmp/hello-server && npm install
		
		CMD cd /tmp/hello-server && npm start
		
	**Note**  
	This exercise uses a customized UBI image that replaces the default UBI repositories with an
	internal classroom repository to eliminate the impact of network errors.  
	By default, Red Hat serves UBI repositories at the https://cdn-ubi.redhat.com/content/public/ubi/
	public CDN. Classroom environments might experience problems when connecting to this CDN.  
	Under reliable network conditions, use the default UBI image.  
	For similar reasons, the npm install command uses an internal NPM registry, specified in the .npmrc file.

	B. Build a container image tagged hello-server:bad by using the Containerfile you created.

		'podman build --file Containerfile -t hello-server:bad'

	C. Create a container called hello-bad that uses the hello-server:bad image and binds the container
		port 3000 to the host port 3000.

		'podman run -d --rm --name hello-bad -p 3000:3000 hello-server:bad

	D. Make a request to the running container by using the curl command.

		'curl http://localhost:3000/greet ;echo' -> {"hello":"world"}

	E. Stop the hello-bad container.

		'podman stop hello-bad'

2. Update the Containerfile to reduce the number of image layers it produces and simplify directory usage.  

	A. Inspect the number of image layers in the hello-server:bad image.

		'podman image tree hello-server:bad'
			...
			Image Layers
			├── ID: 4c0b0f646c88 Size: 234.1MB
			├── ID: 708bb7310471 Size: 4.096kB
			├── ID: dce2ca12e25a Size: 3.072kB Top Layer of: [registry.ocp4.example.com:8443/redhattraining/podman-ubi9.5:latest]
			├── ID: 01459d3461c7 Size: 8.192kB
			├── ID: 398ee6af444e Size: 21.58MB
			├── ID: 3ae709fd1fc5 Size:   242MB
			└── ID: c2d5f5b1b118 Size:   132MB Top Layer of: [localhost/hello-server:bad]

	The layer IDs and sizes differ in your environment from the preceding example.

	Notice that the Containerfile creates several layers on top of the Red Hat UBI base image.

	B. Open the Containerfile that you created and refactor the file to use a WORKDIR instruction.
		Update the paths in the rest of the file to match.

		FROM registry.ocp4.example.com:8443/redhattraining/podman-ubi9.5
		
		WORKDIR /tmp/hello-server
		
		COPY . .
		
		RUN dnf module enable -y nodejs:22
		
		RUN dnf install -y nodejs npm
		
		RUN npm install
		
		CMD npm start
		
	By using WORKDIR, the other instructions no longer need to change directories. This makes reading
	the Containerfile easier and reduces potential mistakes in file paths.  
	The COPY . . instruction copies the content of the current directory 
	(~/DO188/labs/custom-containerfiles/hello-server) into the current directory in the container 
	(/tmp/hello-server).

	C. Update the Containerfile by merging the RUN instructions into a single instruction. 
	The final Containerfile contains the following instructions:

		FROM registry.ocp4.example.com:8443/redhattraining/podman-ubi9.5
		
		WORKDIR /tmp/hello-server
		
		COPY . .
		
		RUN dnf module enable -y nodejs:22 && \
		    dnf install -y nodejs npm && \
		    npm install

		CMD npm start

	D.Build the container image by using the updated Containerfile. Use the hello-server:better image tag.

		'podman build -t hello-server:better .'

	E. Inspect the number of image layers in the hello-server:better image.

		'podman image tree hello-server:better'
			...
			Image Layers
			├── ID: 4c0b0f646c88 Size: 234.1MB
			├── ID: 708bb7310471 Size: 4.096kB
			├── ID: dce2ca12e25a Size: 3.072kB Top Layer of: [registry.ocp4.example.com:8443/redhattraining/podman-ubi9.5:latest]
			├── ID: 3b7e06483376 Size: 8.192kB
			└── ID: 0d47a1b68a27 Size:   385MB Top Layer of: [localhost/hello-server:better]

	Notice that the Containerfile creates two layers on top of the podman-ubi9.5 image. Reducing the 
	number of RUN instructions also reduces the number of resulting image layers.

	F. Run and test the container to verify that it works.

		'podman run -d --rm --name hello-better -p 3000:3000 hello-server:better'
		'curl http://localhost:3000/greet ;echo' -> {"hello":"world"}

		'podman stop hello-better'
		
3. Update the Containerfile to use a better base image and use environment variables to adjust server settings.

	A. Open the Containerfile and change the FROM instruction to use the Node.js runtime image provided by
	Red Hat. This image is based on the Red Hat Universal Base Image (UBI) and includes the Node.js runtime.

		FROM registry.ocp4.example.com:8443/ubi9/nodejs-22-minimal:1

	B. Because the base image includes the Node.js runtime, remove the dnf module enable -y nodejs:22 
	and dnf install -y nodejs part of the RUN instruction.

		COPY . .
		
		RUN npm install
		
		CMD npm start

	This update does not change the number of image layers because the number of RUN instructions remains 
	the same.

	C. Add ENV instructions to set environment variables within the container that the application uses.

		FROM registry.ocp4.example.com:8443/ubi9/nodejs-22-minimal:1
		
		ENV SERVER_PORT=3000
		ENV NODE_ENV="production"
		
		WORKDIR /tmp/hello-server

	**Note**
	Setting the NODE_ENV environment variable to production instructs NPM to ignore the dependencies 
	listed in the devDependencies section of the package.json file.

	Because the devDependencies packages are not necessary to run the application, setting the NODE_ENV
	variable to production reduces the image size.

	The application uses the SERVER_PORT environment variable to determine the port to which it binds.

	D. Update the working directory to /opt/app-root/src, which is a better location than /tmp/hello-server.

		WORKDIR /opt/app-root/src
		
4. Apply appropriate metadata to the image.

	A. Add a LABEL instruction to indicate who is responsible for maintaining the image.

		FROM registry.ocp4.example.com:8443/ubi9/nodejs-22-minimal:1
		
		LABEL org.opencontainers.image.authors="Your Name"
		
		ENV SERVER_PORT=3000
		
	B. Add further LABEL instructions to provide hints as to the version and intended usage of the 
	container image.

		LABEL org.opencontainers.image.authors="Your Name"
		LABEL com.example.environment="production"
		LABEL com.example.version="0.0.1"

		ENV SERVER_PORT=3000

	C. Add an EXPOSE instruction to indicate that the application within the container binds to the 
	port defined in the SERVER_PORT environment variable.

		ENV SERVER_PORT=3000
		ENV NODE_ENV="production"

		EXPOSE $SERVER_PORT

		WORKDIR /opt/app-root/src
		
	The EXPOSE instruction serves for documentation purposes. It does not bind the port on the host
	running the container.

	D. The final Containerfile contains the following instructions:

		FROM registry.ocp4.example.com:8443/ubi9/nodejs-22-minimal:1

		LABEL org.opencontainers.image.authors="Your Name"
		LABEL com.example.environment="production"
		LABEL com.example.version="0.0.1"

		ENV SERVER_PORT=3000
		ENV NODE_ENV="production"

		EXPOSE $SERVER_PORT

		WORKDIR /opt/app-root/src

		COPY . .

		RUN npm install

		CMD npm start

	E. Build the container image by using the updated Containerfile. Use the hello-server:best image tag.

		'podman build -t hello-server:best .'

	F. Inspect the container image to observe the added metadata.

		podman inspect hello-server:best -f '{{.Config.Env}}' -> ...SERVER_PORT=3000 NODE_ENV=production]

	G. Verify that the application works.

		'podman run -d --rm --name hello-best -p 3000:3000 hello-server:best'
		'curl http://localhost:3000/greet ;echo' -> {"hello":"world"}
		'podman stop hello-best'
