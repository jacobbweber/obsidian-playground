---
tags:
  - kubernetes
---
## Overview
### Summary
- super summary
- little more detail summary
### References
- Visuals
	- 
- Docs
	- 
## Troubleshooting
### TroubleTopic1
- 
### Terminology

#kubernetes/kcna -adding this tag here makes flash cards have a folder style , this is useful if I want to also add a different set of flashcard decks to this same note down below. 

Prometheus::tool for gathering and utilizing metric data
Resource::an object of a certain type in the kubernetes API
Command::for listing all available resource types in the cluster kubectl api-resources
Command::for creating own custom resources CustomResourceDefinition
Command::for getting documentation of resource type kubectl explain
Init container::Run a task before a pod's main container starts up
ReplicaSet::Ensure certain number of replica Pods are running at any given time
Deployment::Declarative updates for ReplicaSets and Pods. For scaling STATELESS applications.
Default deployment strategy::Rolling update. Zero downtime for deployments.
Imperative command to create a deployment::kubectl create deploy <name> --image=<image> --replicas=<replicas>
StatefulSet::Similar to deployment but for stateful applications. Replica pods have sticky identity (they keep their position).
What kind of deployment is required to create a headless Service?::StatefulSet
DaemonSet::Dynamically runs a replica pod on each Node or some Nodes
Job::Runs a containerized task to completion; automatically retries if it fails
CronJob::Runs Jobs repeatedly according to a schedule.
WorkerNode::Responsible for running container workloads
Control Plane::Set of components that manage the cluster
API Server::Control plane component. Other components use it to interact with the cluster.
etcd::Control plane component. Distributed object storage used by API Server.
Scheduler::Control Plane Component. Assigns new pods to worker Node
Controller Manager::Control Plane Component. Bundles controllers, each provide some functionality to cluster.
Cloud Controller Manager::Control Plane Component. Bundles controllers that interact with cloud provider API.
Control Plane Components::API Server, Controller Manager, Cloud Controller Manager, etcd, Scheduler,
kube-proxy::Worker Node Component. Manages local routing rules to route network traffic to worker Nodes.
kubelet::Worker Node Component. Kubernetes agent that works with container runtime to run containers on the Node.
Container Runtime::Worker Node Component. Software that runs containers; Containerd, CRI-O.
Kubernetes CRI::Standard interface for container runtimes. Any runtime that implements CRI should work with Kubernetes.
Kubernetes API::HTTP interface for Kubernetes, Control Plane Components, Worker Node Components, and external components use it to communicate.
Kubernetes API uses ______ and ______ objects to represent and change state.::REST; RESTful
Formats _____ or ______ can be used to communicate with the API. But they are still considered ______ objects when persisted/stored.::YAML; JSON; REST
Linux control groups are used to provide container with::isolation
Can you have more than one container per Pod::Yes
Text file that contains commands to build an image::Dockerfile
A container is...::A single process. Isolated from other processes on the host. Packages code with all dependencies it needs.
A pod...::Is a Kubernetes object. Defines and tracks state of containers. Can have more than one container per pod.
What does Scheduler take into account when deciding which Node to assign to.::resource requirements, Pod affinity, taints/tolerations
Kubernetes Security::Kubernetes features to secure cluster
Kubernetes Networking::Managing network communication to and from containers.
Orchestration::Using automation to manage the running and managing of containerized workloads: assigning containers to servers, spinnign them up, restarting them.
Container Runtime Interface::Standard protocol for communication between kubelet and container runtime
4 Cs of Cloud Native Security::Cloud, Clusters, Containers, and Code
Client Certificates::API Server uses a signed X509 client certificate to authenticate user
Open ID Connect::Uses JSON Web Token (JWT) signed by external identity provider to authenticate user
OPA Gatekeeper::Create rules to determine what can and can't be done in a cluster. Validates incoming requests. Exists outside of Kubernetes, can be used in other contexts.
Kubernetes uses a _______ to allow containers to communicate transparently, regardless of which Node they are in.::virtual cluster network
The cluster Domain Name Server allows containers to ________ by hostname::discover Services
Network policies::Control what network traffic is allowed in the cluster network and determine whether pods are isolated or non-isolated.
Pods are _____ by default. All network traffic is allowed,::non-isolated
If any Network Policy selects a pod, the pod is ____::Isolated. Only network traffic allowed by Network Policy is allowed.
Isolation is ________ for incoming traffic (ingress) and outgoing traffic (egress)::treated differently
________ expose an application running on a set of replica pods as a network service::Services
Service Types::determine what service is gonna do and how it's going to behave
ClusterIP service::Exposes internally within cluster network
NodePort::Exposes externally on a port on each Node
LoadBalancer Service::Exposes using cloud provider's load balancer. Only works if cloud integration set up.
ExternalName::Provide DNS name for external service
Headless Service::Service with no cluster IP address
Service without a selector::Requires any endpoints to be created manually
<!--SR:!2024-08-08,1,230-->
Two main service discovery methods in Kubernetes::DNS and environment tables
Ingress::Exposes application externally and routes traffic to a Service, can provide additional functionality
Service Mesh::Tool that manages communication between application components. Can add functionalities.
Two main components of service mesh::control plane & service proxy/data plane
Sidecar::An additional container running in a pod alongside the original container
Service Mesh Interface::standard interface for managing Kubernetes service meshes that support the standard
Volume::Provide external storage to your Kubernetes containers to store application data
PersistentVolume::define dynamically consumable storage resource
PersistentVolumeClaim::Binds to PersistentVolume and allows you to mount storage resource in a Pod.
Rook::Automates storage management with self-managing, self-scaling, self-healing storage services
PersistentVolume reclaim policies::Retain: reclaim manually, Recycle: automatic reclamation via data scrub, Delete: underlying storage resource is deleted
Retain persistentVolume reclaim policy::Reclaim manually
Recycle persistentVolume reclaim policy::Automatic reclamation via simple data scrub.
Delete reclaim policy::Underlying storage resource is deleted
Use command ________ with ConfigMaps and Secrets to mark the data as unchangeable::immutable: true
By default, data stored in Secrets is __________::Not encrypted. base64-encoded.
Cloud Native technology is important because it ____::Removes roadblocks to innovation
Loosely couples microservices::A practice in cloud native architecture, several microservices that work together versus one big service
Open standards::technology specification open to public adoption; technologies that support same open standard can work together
Open Container Initiative (OCI)::Organization that creates open standards for container formats and runtimes.
Image-spec::OCI open standard for container image format
<!--SR:!2024-08-08,1,230-->
Runtime-spec::OCI Open standard for container runtime
Reference implementation for runtime-spec::runc
Telemetry::Collecting data: metrics, log data, about system.
Observability::Ability to understand and measure state of system by looking at the metrics for that system
Container Logs::Understand what is going on within containers. Each container has its own log.
________ and _______ go into the container logs.::standard output and error streams
command to get logs from container::kubectl logs <pod_name> -c <container_name>
Distribute system tracing::Track requests across complex application consisting of multiple components and services
a unique identifier::in distributed system tracing, each request or interaction is tagged with
Trace::Data bout a request as it moves through the system
Span::Request moving through part of system, represents part of trace
Prometheus::open-source tool for monitoring and alerting: gather metric data
Counter::prometheus metric type single number that can only increase or reset to zero
Gauge::prometheus type a single number that can go up or down
Histogram::prometheus type counts observations that fit into configurable buckets
Summary::prometheus type similar to histogram; quantiles over sliding time window; buckets determined automatically.
<!--SR:!2024-08-08,1,230-->
Grafana::Build visual representations of data
FinOps::Practice of using observability to support automation and data-driven decisions
Application delivery::Process and techniques used to ship code to customers: deployment
Deployment Woes::Issues caused by deployment of new code
CI/CD::Continuous integration/Continuous delivery
GitOps::Using Git to manage application infrastructure. Source of truth for declarative infrastructure/applications.
Automation::_____ what is in Git implements; to make changes to application, just have to change what is in Git
How GitOps Works::Git repository contains files describing cluster's desired states;tool watches Git repository; tool automatically makes changes implementing what is in Git
GitOps Flux tool::Syncs YAML manifests in Git repository with your cluster; Built on top of GitOps toolkit; written in Go
Argo CD tool::Uses GitOps to provide continuous delivery in Kubernetes.
In continuous integration, compiling, integration, and automated tests _______ when a dev ______::occur automatically;pushes code changes to a repository like Git
Kubeadm::tool to simplify process of setting up Kubernetes cluster
what do control groups provide when it comes to containers?::isolation
What command can you use to get documentation about a Kubernetes resource type?::kubectl explain
You need to run a stateless application in Kubernetes, and you want to be able to scale easily and perform rolling updates