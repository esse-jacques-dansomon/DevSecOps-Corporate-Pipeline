# AWS Infrastructure Setup

![img.png](images/04-instances.png)

## Table of content 
* Kubernetes instances
    - Master node 
    - Worker node 1
    - Worder node 2
* Sonarqube instance
* Nexus instance
* Jenkins instance
* Monitor instance

### Pre requis 
```
1. Navigate to security Ec2 dashboard > Network and Security > Security Group
then create a new secrity group with theses settings 
```

![02-inbound-rules.png](./images/02-inbound-rules.png)


### Kubernetes instances

```text
Create 3 instance of Ec2,

Software Image (AMI)
Canonical, Ubuntu, 22.04 LTS, amd64 jammy image build on 2024-04-11
ami-0e001c9271cf7f3b9

make sure to choose
a security group, a key pair and 20giga of volume

after rename the three instance as Master, Node-1, Node-2
```

![img.png](images/01-k8s.png)

#### Master Node installation 
```
```


### Sonarqube

```
Create an instance of EC2 as the Kubernetes but juste one instance 
```

### Nexus
```
Same as the Kubernetes but juste one instance 
```

### Jenkins
```
Create one instance of EC2 with these specs
```
![img.png](images/02-jenkins.png)

### Note 
```
You can setup elastic ip address in order to not have to change ip after a stop/restart 
```

### Connect to your instances with the key pair
```
On MacOs/Linux you can use Zoc8
On Windows you can use MobaXterm
```






