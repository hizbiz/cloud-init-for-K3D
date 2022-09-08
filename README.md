# k3d-keda-rabbitmq-pika-example

This will be a step to step guide on how to setup [K3D](https://k3d.io) on [Ubuntu](https://ubuntu.com/). We will create a [K3D](https://k3d.io) kubernetes cluster and install [KEDA](https://keda.sh/) and [RabbitMQ](https://www.rabbitmq.com/) in it, then we will create a sample Python application with pika to post messages to the RabbitMQ queue and consume messages from it. Addionally you will learn to create helm charts to install this sample application into the cluster as well.
