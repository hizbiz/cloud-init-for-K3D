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
The recommended way to create K3D kubernetes develop/test environment on Ubuntu is to use multipass, with which a standard K3D kubernetes environment (together with a brand new Ubuntu 20.04+ VM) can be created in a couple of minutes.

If however, you want to use an existing Ubuntu environment, refer to [this page](https://github.com/hizbiz/k3d-keda-rabbitmq-pika-example/wiki/Setup-K3D-kubernetes-develop-environment-manully) to create the environment manually.

The steps to create K3D kubernetes develop/test enviornment automatically with multipass:
1. Install (if you don't have it already) [multipass](https://multipass.run/)
2. Open a terminal, clone this repository and move into it.
3. Create K3D kubernetes environment with multipass
```bash
multipass launch -n test -c 2 -m 2G -d 40G --cloud-init <path-to-cloud-init-k3d.yaml> -vvvv
```
  The example command above will create a Ubuntu 20.04+ VM named *test* with the following pre-installed:
  - kubectl with bash auto-complete
  - helm with bash auto-complete
  - k3d with bash auto-complete

4. shell into the newly created VM
```bash
multipass shell test
```

5. play with the newly created environment.
