# k3d-keda-rabbitmq-pika-example

This is a sample repository that shows a step to step guide on how to setup a [K3D](https://k3d.io) [Kubernetes](https://kubernetes.io/) cluster environment on [Ubuntu](https://ubuntu.com/). We will create a [K3D](https://k3d.io) [kubernetes](https://kubernetes.io/) cluster, install [KEDA](https://keda.sh/) and [RabbitMQ](https://www.rabbitmq.com/) in it, then create a sample Python application with pika to publish jobs to the RabbitMQ queue and consume messages from it (to simulate worker that does the job). Addionally a [helm charts](https://helm.sh/) to install this sample application into the cluster will be demonstrated as well.

## 1. Environment setup
### Startpoint
Assuming we have an [Ubuntu](https://ubuntu.com/) 20.04+ environment as starting point.

### Install required packages
```bash
sudo apt update && sudo apt install -y  -y ca-certificates curl gnupg lsb-release software-properties-common
```

### Install [Docker and Docker Compose](https://docs.docker.com/get-started/overview/)
```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update && sudo apt install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin
# It is recommened to add your user name to docker group
# sudo usermod -aG docker <your-user-name>
```

### Install [kubectl](https://kubernetes.io/docs/reference/kubectl/kubectl/)
```bash
sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo apt update && sudo apt install -y kubectl
sudo kubectl completion bash | sudo tee /etc/bash_completion.d/kubectl
```

### Install [Helm](https://helm.sh/)
```bash
curl https://baltocdn.com/helm/signing.asc | sudo gpg --dearmor -o /usr/share/keyrings/helm.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/helm.gpg] https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt update && sudo apt install -y helm
helm completion bash | sudo tee /etc/bash_completion.d/helm
```

### Install [K3D](https://k3d.io/)
```bash
curl -s https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh | bash
k3d completion bash | sudo tee /etc/bash_completion.d/k3d
```

### Add Helm repository
```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo add kedacore https://kedacore.github.io/charts
helm repo update
```

## 2. Create cluster
```bash
# use 'k3d cluster create -h' to see help on how to create cluster
# here we create one cluster named 'dev' and map the host port 8080 to the default load balancer in the created cluster
k3d cluster create dev -p 0.0.0.0:8080:80@loadbalancer
```
Now you should be able to see the newly created cluster with:
```bash
kubectl cluster-info
```
or:
```bash
k3d cluster ls
```
