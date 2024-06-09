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
```

```shell
sudo chmod +x 01.sh 
./01.sh 
```

## 2. Initialize the control in Master node 

**ONLY ON MASTER NODE**
```shell
sudo kubeadm init --pod-network-cidr=10.244.0.0/16


--------------------
sudo kubeadm reset
kubeadm join 172.31.55.142:6443 --token 54951k.h24z65ggn97aosur \
        --discovery-token-ca-cert-hash sha256:b81aa38f51b84edb78883d8527a87aa8399c4a45a9b5250120c620706da8ffea 
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

### Generate a token 

#### 1. Create a new namespace

```shell
kubectl create ns webapps
```
#### 2. Create a new `ServiceAccount`

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: jenkins
  namespace: webapps
```
save the file as **svc.yaml**

```shell
kubectl apply -f svc.yaml -n webapps
```

#### 3. Create a new `Role`

```groovy
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: app-role
  namespace: webapps
rules:
  - apiGroups:
      - ""
      - apps
      - autoscaling
      - batch
      - extensions
      - policy
      - rbac.authorization.k8s.io
    resources:
      - pods
      - secrets
      - componentstatuses
      - configmaps
      - daemonsets
      - deployments
      - events
      - endpoints
      - horizontalpodautoscalers
      - ingress
      - jobs
      - limitranges
      - namespaces
      - nodes
      - pods
      - persistentvolumes
      - persistentvolumeclaims
      - resourcequotas
      - replicasets
      - replicationcontrollers
      - serviceaccounts
      - services
    verbs: ["get", "list", "watch", "create", "update", "patch", "delete"]
```

save the file as **role.yaml**

```shell
kubectl apply -f role.yaml -n webapps
```

#### 4. Bin the `Role` to a  `ServiceAccount`

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: jenkins
  namespace: webapps
```
save the file as **bind.yaml**

```shell
kubectl apply -f bind.yaml 
```

#### 5. create a secret for the service account

```yaml
apiVersion: v1
kind: Secret
type: kubernetes.io/service-account-token
metadata:
  name: mysecretname
  annotations:
    kubernetes.io/service-account.name: jenkins
```
save the file as **sec.yaml**

```shell
kubectl apply -f sec.yaml -n webapps
```

