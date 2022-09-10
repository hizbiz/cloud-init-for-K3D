This repository contains a sample Python application implemented to consume messages from a RabbitMQ queue.
Using this sample application, I will demonstrate:

+ Setup K3D kubernetes cluster environment
+ Create a test K3D kubernetes cluster
+ Install KEDA and RabbitMQ to the newly created test cluster
+ Expose RabbitMQ in the test cluster to the host machine
+ Deploy the sample application as KEDA ScaledJob with or without Helm Chart
+ Post job message from Web Browser on host machine or with kubernetes Job from within the cluster
+ Verify the deployed sample application (KEDA ScaledJob) is activated by KEDA and actually consumes the posted message

## Setup K3D kubernetes develop/test environment
Refer to [this page](https://github.com/hizbiz/k3d-keda-rabbitmq-pika-example/wiki/Setup-K3D-kubernetes-develop-environment-manully) to setup the environment manually if you have a working Ubuntu 20.04 environemnt.

Or, preferable, you can install multipass and use the cloud-init.yaml in this repostiory to create one Ubuntu VM with everything configured in a couple of minutes all automatically. 
