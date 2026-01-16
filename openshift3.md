# OPENSHIFT

Part 3/3

## APPLICATION HIGH AVAILABILITY WITH KUBERNETES

### Concepts of Deploying Highly Available Applications

Hight availablility (HA) is the goal to make applicaion robust and resistant to runtime failure. By
implementing HA techiques reduces the possibilty of the applicaion of being unavailable to users.

In general, HA can protect an applicaion from falure in the following context

- From itself in the form of application bugs
- From its environment, such as networking issues
- From other applications that exhaust cluster resources

Also HA practices can protect the cluster from applicaion such as one with a memory leak.


#### Writing Reliable Applications

HA tooling mitigate worst-case scenarios. HA is not a way for replacing application issues. Applications
must work in the cluster so that OpenShift can handle failure scenarios.

OpenShift expects the following from applications:

- Tolerates restarts
- Responds to health probes, such as the startup, readiness, and liveness probes
- Supports multiple simultaneous instances
- Has well-defined and well-behaved resource usage
- Operates with restricted privileges

Applicaion can run without these behaviours, applicaion with these behaviours better use the reliability
and HA feature that Kubernetes provides.

Many web based applicaions provide an endpoints for verification of applications health. The cluster
can monitor the endpoint for possible issues with the applicaion.

It is the application and its developer that is responsible for providing an endpoint so that decision
about the health (os state) of the application can be done. AS an exaple the health of an application
can depend on a databse connection, no connection is a bad state. But not all application have or need
a database so the database connection check is not needed. It is all up to the developers to provide
a way for checking the healt /stae of the applicaion.


### Kubernetes Application Reliability

When a application in a pod crashes it can not respond to requests, depending on the configuration the
cluster can automatically restart the pod. If the applicaion only fails without crashing the pod will
continue to exists bot can still not respond to reqests.

The cluster can detect and act on a running bit unhealth pod only with the appropriate health probes.

Kubernetes has the following HA techiques to improve application reliability

- Restarting pods  
	By configuring a restart policy on a pod, the cluster restarts misbehaving instances of an application.

- Probing  
	By using health probes, the cluster detects when applications cannot respond to requests, and can
	automatically act to mitigate the issue.

- Horizontal scaling  
	When the application load changes, the cluster can scale the number of replicas to match the load.


#### Restarting Pods

A container inside a pod can terminate for different reasons. The pod *restartPoliy' specifies how the
*kubelet' should handle the failure. The values are *Always*, *OnFailure* and *Never*.

| Policy    | Description |
|---        |---          |
| Always	| The kubelet always restarts a terminated container. This policy is the default. |
| OnFailure | The kubelet restarts a terminated container only if it exited with a non-zero exit code. |
| Never     | The kubelet never restarts a terminated container. |

NOTE!  
For application workloads, you typically use higher-level controllers such as Deployment or StatefulSet
to manage pods. These controllers ensure that a specified number of replicas are always running.
If a pod that is managed by a Deployment fails, then the controller automatically creates another pod
to replace it, which is a core HA mechanism. These controllers require the pod template's restartPolicy
field to be set to Always.


## Application Health Probes

### Kubernetes Probes

### Authoring Probe Endpoints

### Probe Types

#### Startup Probes

#### Readiness Probes


### Types of Tests

Specify on of the following types of test to perform when defining a probe.


