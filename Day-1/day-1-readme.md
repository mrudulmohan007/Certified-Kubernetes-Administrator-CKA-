The Kubernetes (k8s) architecture is divided into two main components: the Control Plane (Master Node) and Worker Nodes. 

1. Control Plane / Master Node

The Control Plane is responsible for managing the overall Kubernetes cluster. It handles scheduling, maintaining the desired state of applications, and managing the lifecycle of applications. It includes several key components:
•
ETCD: This is a distributed key-value store that Kubernetes uses to store all cluster data. It’s like a central database where Kubernetes keeps information about the cluster’s state. Every component in the Control Plane can access ETCD to retrieve or update the cluster’s configuration data. ETCD stores information like configuration data, nodes, pods, and secrets. It’s highly available and consistent.
•
API Server: The API Server is the main interface for interacting with the Kubernetes cluster. It exposes the Kubernetes API, which is used by internal components, administrators, and external clients to communicate with the cluster. Whenever you want to create, modify, or delete Kubernetes resources, you send requests to the API Server. It’s the main access point for all administrative tasks and processes.
•
Controller Manager: This component ensures that the desired state of the cluster matches the current state. Controllers are processes that continuously monitor the state of the system and work to bring it to the desired state. For example, if a pod goes down, the Replication Controller ensures that the correct number of replicas is maintained by creating a new pod. The Controller Manager includes multiple controllers, such as the Node Controller (for node status), Replication Controller, and Endpoints Controller.
•
Scheduler: The Scheduler is responsible for assigning pods to nodes. When a new pod is created, the Scheduler selects an appropriate node based on factors like resource requirements (CPU, memory), affinity/anti-affinity rules, and other policies. Once it selects a node, the pod is assigned to it, and it becomes the Scheduler’s job to ensure that it runs on the designated node.

2. Worker Nodes

Worker Nodes are the machines (either physical or virtual) where your applications actually run. Each worker node contains the necessary services to manage, run, and monitor containers. Each node hosts one or more Pods (the smallest unit in Kubernetes, which encapsulates one or more containers). Here’s a breakdown of the key components on the Worker Node:
•
Kubelet: Kubelet is an agent that runs on each worker node. It takes instructions from the Control Plane (specifically the API Server) and makes sure the containers in the assigned pod are running as specified. The Kubelet monitors pod health and reports back to the Control Plane. It reads the container specs from the API Server and interacts with the container runtime to launch and manage containers.
•
Kube-Proxy: Kube-Proxy manages network rules and facilitates communication between pods on different nodes. It creates and manages network routes, allowing services to communicate both internally within the cluster and externally. Kube-Proxy works at the IP layer, managing load balancing, forwarding, and IP masquerading.

3. Pods and Containers

•
Pod: A pod is the smallest deployable unit in Kubernetes, containing one or more containers. Pods encapsulate application containers, storage resources, and unique network IPs. If you need to scale an application, you deploy multiple replicas of a pod. The containers inside a pod share the same network namespace, meaning they can communicate with each other directly via localhost.
•
Container: Containers are the actual applications running within each pod. Kubernetes supports various container runtimes, such as Docker, containerd, and CRI-O. Containers in a pod share storage and network resources, making it easier for them to work together.

Networking (Nginx Example)

In the diagram, there’s a mention of an nginx pod. This represents a service or application running on the cluster. Networking in Kubernetes allows pods and services to communicate within the cluster and with external networks. With components like Kube-Proxy and Cluster IP, Kubernetes manages these connections, enabling load balancing and service discovery.

Communication and Flow

1.
User Interaction: Users or administrators interact with the Kubernetes cluster through the API Server using kubectl, the Kubernetes command-line tool.
2.
Scheduling and Pod Creation: When a user deploys an application, the API Server receives the request, and the Scheduler determines which worker node should host the new pod.
3.
State Management: Controllers within the Controller Manager constantly monitor the state of resources in the cluster. For instance, if a node fails, the Controller Manager may reschedule pods on a different node.
4.
Kubelet: On each worker node, the Kubelet ensures that the containers for the pods are running as expected, reporting the status to the API Server.
5.
Networking with Kube-Proxy: Kube-Proxy manages network traffic to ensure seamless communication within the cluster and external clients. It handles requests from external sources and directs them to the appropriate pods, enabling load balancing.

Summary

The Control Plane manages and maintains the overall state of the cluster, ensuring resources are allocated as desired and system health is continuously monitored. Worker Nodes run the actual applications in the form of pods. The Control Plane components work together to ensure high availability, reliability, and scalability of the applications running within the Kubernetes cluster.

Each component plays a crucial role in maintaining the desired state, balancing load, managing resources, and enabling secure and efficient communication within the cluster


