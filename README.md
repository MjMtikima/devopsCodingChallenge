# devopsCodingChallenge

#PROCESS FOR DEPLOYING AND MONIORING AND APPLICATION TROUGHT A KUBERNETES CLUSTER

Our environment is made up of 3 virtual machines (VPC) running with the ubuntu 20.04 operating system. One of them will be used as a master node which will hold the kubernetes master node and the other will be used as worker node. 

1. Main Architecture
![image](https://github.com/MjMtikima/devopsCodingChallenge/assets/116918088/2b9477ae-489e-4c8b-b3c3-53f4077e0d45)

2. Installing KUBERNETES IN A MASTER AND WORKER NODE

Installation of package
kubelet
kubeadm
kubectl
kubernetes-cni
Disabling swap memory on virtual machine
Setting an unique hostname on each node
For making each node seeing bridged traffic, we should ensure that net.bridge.bridge-nf-call-iptables is set to 1.

3. Installing DOCKER CE RUNTIME for running container in our node

Adding a correspondent repository
Installing package:
container.io
docker-ce
Docker-ce-cli
Adding docker as a service for managing with systemd
	

4. Initialisation of master node

Use kubeadm for initializing the master node: sudo kubeadm init –pod-network-cidr=10.244.0.0/16 
After running this command, we will get in terminal the command to use in worker node for joining the cluster
kubeadm join master-node:6443 --token sr4l2l.2kvot0pfalh5o4ik \
    --discovery-token-ca-cert-hash sha256:c692fb047e15883b575bd6710779dc2c5af8073f7cab460abd181fd3ddb29a18 \
    --control-plane 

 and another command to run in master node before starting the cluster
Next we should run this second command
`mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config`

5. Setting up pod network from the master node

Pod network facilitates communication between servers and it’s necessary for the proper functioning of kubernates
First, we allow connection to port 6443 on tcp protocol through firewall
`sudo ufw allow 6443`
`sudo ufw allow 6443/tcp`
Secondly, we run this command:
`kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml`
It will deploy a network for our pod
After this command ended, run this: `kubectl get pods –all-namespaces` to confirm that everything was successful
We can also see the health of the kubernetes components with this command `kubectl get componentstatus`

6. Joining worker nodes to the kubernetes cluster from worker node

Now that the master node is up and the pod network is ready, we can join our worker node to the cluster.
Run the command saved when we initialized the master node in step 4.
We should have an output message displaying the result
Once we joined the cluster from the worker node, we can now go to master and run this command: `kubectl get nodes` for displaying the status of our nodes.

![kubernetes_master_status (1)](https://github.com/MjMtikima/devopsCodingChallenge/assets/116918088/19113548-fff9-4ff7-99a2-b7a842d0eb3b)


7. Deploying an application in the cluster
The application to deploy should have it image in docker hub. So you should have an account on docker hub
If the image is ready, from the master node, we should create a deployement config file with all reqirements
After creating the deployment (deployment.yaml) file, we create a service (service.yaml) config file
Now we should run the command below for creating the pods deployment and service:”kubectl apply -f deployment.yaml” and “kubectl apply -f service.yaml”
With the command “kubectl get pods” we should see our pod running

![kubernates_kuard_pod (1)](https://github.com/MjMtikima/devopsCodingChallenge/assets/116918088/4c2a57b2-1853-42b3-bfa2-6501f7049d29)


8. Installing prometheus (master node)

Prometheus is used for monitoring our kubernetes cluster
First we should create a namespace “monitoring” with `kubectl create namespace monitoring`
Normally, we should create a persistant volume for saving prometheus data, but in our test case, we left that.
Creating a namespace limit the permission of default roles in kubernates cluster, so we need to give prometheus access to all cluster resources by creating a cluster role file
We created a clusterRole.yaml file and apply it with command `kubectl apply -f clusterRole.yaml`
The next step is the creation of prometheus configMap (config-map.yaml) which describe how the monitoring should work
The last step is to create a prometheus deployment (prometheus-deployment.yaml) and service (prometheus-service.yaml) files and then apply it.
We can access prometheus through our public ip with 30909 port 

![prometheus_details (1)](https://github.com/MjMtikima/devopsCodingChallenge/assets/116918088/f6bde2cf-61e9-4151-99e1-3b65888dbdd1)


9. Installing grafana
Grafana is a visualization and analytics software.It helps us to visualize data produced by prometheus.
Create a grafana config file (grafana-datasource-config.yaml) and apply it 


![grafana](https://github.com/MjMtikima/devopsCodingChallenge/assets/116918088/1bb578b4-8585-4cca-ad2e-59177e2db40d)
