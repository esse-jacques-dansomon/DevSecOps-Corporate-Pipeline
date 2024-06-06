# Kubernetes setup 

```
Login into Master, node-1 and node-2
```

## 1. Install kubeadm, kubelet and kubectl on Master, Node-1 and Node-2

```shell
vi 01.sh
```
**copy and paste**

```shell
sudo apt-get update

sudo apt install docker.io -y
sudo chmod 666 /var/run/docker.sock

sudo apt-get install -y apt-transport-https ca-certificates curl gnupg
sudo mkdir -p -m 755 /etc/apt/keyrings

curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.28/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.28/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list

sudo apt update

sudo apt install -y kubeadm=1.28.1-1.1 kubelet=1.28.1-1.1 kubectl=1.28.1-1.1
sudo apt install -y kubelet=1.26.5-00 kubeadm=1.26.5-00 kubectl=1.26.5-00
```

```shell
sudo chmod +x 01.sh 
./01.sh 
```

## 2. Initialize the control in Master node 

**ONLY ON MASTER NODE**
```shell
sudo kubeadm init --pod-network-cidr=10.244.0.0/16
```

After this commande you will see a command to add workers node to your cluster 
Copy it and paste into the workers node instances to join the cluster


```shell
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config

kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml

kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v0.49.0/deploy/static/provider/baremetal/deploy.yaml

kubectl get nodes
```

![02_AWS_Infrastructure_Setup/images/05.png](/02_EC2_Instances_Setup/images/05.png)


## 3. Setup namespace and Users for the CICD



