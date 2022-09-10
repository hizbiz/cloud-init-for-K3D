This repository contains a sample Python application implemented to consume messages from a RabbitMQ queue.
Using this sample application, I will demonstrate:

+ Setup K3D kubernetes cluster environment
+ Create a test K3D kubernetes cluster
+ Install KEDA and RabbitMQ to the newly created test cluster
+ Expose RabbitMQ in the test cluster to the host machine
+ Deploy the sample application as KEDA ScaledJob with or without Helm Chart
+ Post job message from Web Browser on host machine or with kubernetes Job from within the cluster
+ Verify the deployed sample application (KEDA ScaledJob) is activated by KEDA and actually consumes the posted message

## Create K3D kubernetes develop/test environment
I will show two ways to create a K3D kubernetes environment on Ubuntu 20.04+: manual and automatic.

The recommended way is to use multipass, with which a standard K3D kubernetes environment (together with a brand new Ubuntu 20.04+ VM) can be created in a couple of minutes. See the instruction below:

*If however, you want to use an existing Ubuntu environment, refer to [this page](https://github.com/hizbiz/k3d-keda-rabbitmq-pika-example/wiki/Setup-K3D-kubernetes-develop-environment-manully) on how to do it manually.*

### Create K3D kubernetes develop/test enviornment automatically with multipass:
1. Install (if you don't have it already) [multipass](https://multipass.run/) following [the instruction from its official website](https://multipass.run/install).
2. Open a terminal, clone this repository and move into it.
3. Create K3D kubernetes environment with multipass
```bash
multipass launch -n dev -c 2 -m 2G -d 40G --cloud-init <path-to-cloud-init-k3d.yaml> -vvvv
```
  The example command above will create a Ubuntu 20.04+ VM named *dev* with the following packages pre-installed:
  - kubectl with bash auto-complete
  - helm with bash auto-complete
  - k3d with bash auto-complete

4. shell into the newly created VM
```bash
multipass shell dev
```
5. play with the newly created environment.
Refer to [this page](https://github.com/hizbiz/k3d-keda-rabbitmq-pika-example/wiki/Setup-K3D-kubernetes-develop-environment-manully#6-verify-the-environment) on how to verify the newly created VM.

### Create a K3D kubernetes develop/test cluster
1. shell into the newly created VM if you haven't yet
```bash
multipass shell dev
```
2. create cluster
```bash
k3d cluster create test -p 0.0.0.0:8080:80@loadbalancer
```
The above example command will create a cluster named test and map VM dev's port 8080 to the cluster test's ingress load balancer.
