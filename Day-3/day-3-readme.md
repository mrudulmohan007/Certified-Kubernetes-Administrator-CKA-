----CKA preparation Day3- Pods in k8s--

A pod in Kubernetes (k8s) is a group of one or more containers that share resources and are treated as a single unit. Pods are the smallest deployable units in Kubernetes and are used to model an application-specific logical host.

A pod can be created in two ways:

a) Imperative way(Through command or API calls):

Lets say Iam creating an nginx pod through imperative way

kubectl run nginx-pod --image=nginx:latest
To check whether the pod is running,

kubectl get pods
Ready 1/1 means the pod which we have created has one container and out of one container one is running.

b) Declarative way ( By creating manifest files)

Create a file named pod.yaml and then add the following content:

# This is a sample pod yaml

apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    env: demo
    type: frontend
spec:
  containers:
  - name: nginx-container
    image: nginx
    ports:
    - containerPort: 80
How do we find the kind and apiVersion that we can use in the above yaml file?

kubectl explain pod
To create the pod out of this yaml file

kubectl create -f pod.yaml
If we update the yaml file

kubectl apply -f pod.yaml
In summary, use kubectl create -f pod.yamlfor new resources and kubectl apply -f pod.yamlfor both creating and updating existing resources.

To troubleshoot any issues with the container and yaml file,

kubectl describe pod pod_name
If we have to make changes directly on the running pod we will use

kubectl edit pod pod_name

#This will open a vi editor,once we update the yaml file, we dont have to 
#apply the changes because we have already made some changes in running pod
To enter inside a pod we use the command

kubectl exec -it pod_name --sh
If we dont want to type all the contents of a pod.yaml file, we can just use the following command:

kubectl run nginx --image=nginx --dry-run=client -o yaml >pod-new.yaml 
By using kubectl run, you are instructing Kubernetes to create a new Pod named "nginx" with the specified image "nginx". The --dry-run=client flag indicates that you want to simulate the command without making any changes to the cluster, which is particularly useful for testing and validation.The -o yaml option formats the output in YAML, a human-readable data serialization format, which is widely used in Kubernetes for configuration files. By redirecting the output to pod-new.yaml, you are effectively saving the generated configuration to a file for future use or modification. This approach allows developers to review, edit, and version control their configurations before applying them to the cluster, ensuring a more controlled deployment process.

To know all the details about a pod

kubectl describe pod pod_name
To show the labels associated with a particular pod

kubectl get pods pod_name --show-labels
To get the details about the node in which the particular pod is running

kubectl get pods -o wide
To know more about a node

kubectl get nodes -o wide