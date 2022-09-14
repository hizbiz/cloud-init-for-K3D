This repository contains a minimalist cloud-init file, with which a [Ubuntu](https://ubuntu.com/) 20.04+ based [K3D](https://k3d.io/) [kubernetes](https://kubernetes.io/) environment can be created in a couple of minutes.

## Create [K3D](https://k3d.io/) [kubernetes](https://kubernetes.io/) environment with [multipass](https://multipass.run/)
In this section, we will demonstrate how to use [multipass](https://multipass.run/) and the cloud-init file in this repository to create a [K3D](https://k3d.io/) [kubernetes](https://kubernetes.io/) environment all automatically.

*If however, you want to use an existing [Ubuntu](https://ubuntu.com/) environment, refer to [this page](https://github.com/hizbiz/k3d-keda-rabbitmq-pika-example/wiki/Setup-K3D-kubernetes-develop-environment-manully) on how to do it manually.*

### Steps to create the environment
1. Install [multipass](https://multipass.run/) (if you don't have it already) following [the instruction from its official website](https://multipass.run/install).
2. Open a terminal, clone this repository and move into it.
3. Create [K3D](https://k3d.io/) [kubernetes](https://kubernetes.io/) environment with [multipass](https://multipass.run/)
```bash
multipass launch -n dev -c 2 -m 2G -d 40G --cloud-init ./cloud-init-k3d.yaml -vvvv
```
That's it! The example command above will create a [Ubuntu](https://ubuntu.com/) 20.04+ VM named *dev* with 2 CPU, 2G RAM, 40G HDD and the following pre-installed/configured:
  - [kubectl](https://kubernetes.io/docs/reference/kubectl/kubectl/) with bash auto-complete
  - [HELM](https://helm.sh/) with bash auto-complete
  - [K3D](https://k3d.io/) with bash auto-complete

### Recommended configuration
shell into the newly created VM and add a couple of [HELM](https://helm.sh/) repositories
```bash
multipass shell dev
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo add kedacore https://kedacore.github.io/charts
helm repo update
```
* Note that wWe have added [KEDA](https://keda.sh/)'s repository as well.*

## Play with the newly created environment.
### Create a K3D kubernetes develop/test cluster
1. shell into the newly created VM if you haven't yet
```bash
multipass shell dev
```

2. create cluster
```bash
k3d cluster create test
```
The above example command will create a cluster named *test* and points your [kubectl](https://kubernetes.io/docs/reference/kubectl/kubectl/) to the newly created cluster, *test*

### Install KEDA to its own namespace keda
```bash
kubectl create namespace keda
helm install keda kedacore/keda --namespace keda
```

### Install RabbitMQ to its own namespace rabbit
```bash
kubectl create namespace rabbit
helm install --set auth.username=rabbit --set auth.password=rabbit rabbit bitnami/rabbitmq --namespace rabbit
```
The above example command install RabbitMQ to its own namespace rabbit and set both username and password to rabbit.

#### Expose RabbitMQ
```bash
kubectl port-forward --namespace rabbit svc/rabbit-rabbitmq 15672:15672 --address=0.0.0.0
```
The above command will direct any traffic to HTTP Port (15672) on the host VM to the RabbitMQ Management Service running inside the cluster. For example, you can access RabbitMQ in the cluster from the host machine where you run multipass to create the Ubuntu VM.

1. identify multipass Ubuntu VM's IP address
  - From host machine where you install multipass
  ```bash
  multipass ls
  ```
  - From within the Ubuntu VM
  ```bash
  ip address
  ```
2. access RabbitMQ from web browser
open your favorate web browser and access: http://<VM-IP>:15672/, you will see RabbitMQ Manaement web page.
  
### Cleanup
Now it's time to cleanup the test cluster we have created:
```bash
k3d cluster delete test
```

That's it. I guess we have verified we have got a working [K3D](https://k3d.io/) [kubernetes](https://kubernetes.io/) environment.
