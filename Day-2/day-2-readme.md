----Kind installation and K8s local cluster setup on Arch Linux-----

--Introduction--
This week marks the beginning of my journey towards becoming a Certified Kubernetes Administrator (CKA). I focused on setting up my local environment and getting familiar with basic Kubernetes commands. Here’s a summary of what I accomplished in Week 1.

--Setting Up Kind on Linux--
To start with Kubernetes, I chose to use Kind, a tool for running local Kubernetes clusters using Docker container nodes. It’s a great way to get hands-on experience with Kubernetes without the need for cloud infrastructure. Here are the steps I followed:

--Installing Kind--
First, I installed Kind on my Arch Linux machine using the following commands:

# For AMD64 / x86_64
[ $(uname -m) = x86_64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.23.0/kind-linux-amd64

# For ARM64
[ $(uname -m) = aarch64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.23.0/kind-linux-arm64

chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind

--Creating Clusters--
Before this step, you need to make sure that the docker is installed in your local machine. If you haven’t installed docker yet, please follow the steps mentioned below:

To install Docker on an Arch Linux system, you can follow these steps:

# Update your system
sudo pacman -Syu

# Install Docker
sudo pacman -S docker 

# Start Docker service
sudo systemctl start docker

# Enable Docker service to start on boot
sudo systemctl enable docker

# Add current user to the Docker group
sudo usermod -aG docker $USER

# Log out and log back in to apply group changes (manual step)

# Test Docker installation
sudo docker run hello-world
--After installing Kind, I created two clusters:

kind create cluster --image kindest/node:v1.30.0@sha256:047357ac0cfea04663786a612ba1eaba9702bef25227a794b52890dd8bcd692e
 --name kind-cka-cluster1 #you can give whatever name,I just gave name as cka-cluster1 here

kind create cluster --image kindest/node:v1.30.0@sha256:047357ac0cfea04663786a612ba1eaba9702bef25227a794b52890dd8bcd692e
 --name kind-cka-cluster2

#checkout kind official documentation for the image name
# If you do the above steps, it will create clusters with 1 node (control plane)
# If you want to create multi node clusters, for example, create a file named config.yaml and add the below content:
touch config.yaml #creating a file named config.yaml
vim config.yaml     
# and type i to go to insert mode and then add the yaml content below


# three node (two workers) cluster config
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: worker
- role: worker

#then save by pressing esc and type :wq!

kind create cluster --image kindest/node:v1.30.0@sha256:047357ac0cfea04663786a612ba1eaba9702bef25227a794b52890dd8bcd692e
 --name kind-cka-cluster3 --config config.yaml
Switching Between Clusters
With multiple clusters set up, I needed to switch between them using kubectl. Before that If you haven't installed kubectl on your machine, follow these steps:

Installing kubectl on Arch Linux
# Update your system
sudo pacman -Syu 

# Install kubectl
sudo pacman -S kubectl 

# Verify kubectl installation
kubectl version --client
Now I need to switch between the two clusters using kubectl, let's do that:

# List all contexts
kubectl config get-contexts

# Switch to a specific context
kubectl config use-context kind-cka-cluster2 
Checking Cluster Nodes
To ensure that my clusters were up and running, I used the following command to display information about the nodes:

kubectl get nodes