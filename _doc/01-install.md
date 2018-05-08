# Kubernetes Sandpit
Sample project to install `minikube` and deploy a number of kubernetes artifacts to demonstrate functionaility.


## Installing and Starting `minikube`
### Install `minikube`
At the time of writing, the current version is `0.26.1` and the install command is as follows:
```bash
$ curl -Lo minikube https://storage.googleapis.com/minikube/releases/v0.26.1/minikube-darwin-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/
```
Refer to <https://github.com/kubernetes/minikube/releases> for detailed instructions for installing `minikube`.

### Install `kubectl`
To install `kubectl` using Homebrew, the command is as follows:
```bash
$ brew install kubectl
```
Refer to <https://kubernetes.io/docs/tasks/tools/install-kubectl/> for further details on installing `kubectl`. 

### Install Appropriate Driver Plugin
To install the `Hyperkit` driver, the command is as follows:
```bash
$ curl -LO https://storage.googleapis.com/minikube/releases/latest/docker-machine-driver-hyperkit \
&& chmod +x docker-machine-driver-hyperkit \
&& sudo mv docker-machine-driver-hyperkit /usr/local/bin/ \
&& sudo chown root:wheel /usr/local/bin/docker-machine-driver-hyperkit \
&& sudo chmod u+s /usr/local/bin/docker-machine-driver-hyperkit
```
Refer to <https://github.com/kubernetes/minikube/blob/master/docs/drivers.md#xhyve-driver> for further details on installing driver plugins.

### Start `minikube`
Execute the following command:
```bash
minikube start --vm-driver=hyperkit --cpus 4 --memory 8192
 
# causes it to hang
# --extra-config=controller-manager.HorizontalPodAutoscalerUseRESTClients=false
```

### Enable the `metrics-server` and `heapster` Addons
```bash
# list the current addons and their status
$ minikube addons list
- addon-manager: enabled
- coredns: disabled
- dashboard: enabled
- default-storageclass: enabled
- efk: disabled
- freshpod: disabled
- heapster: enabled
- ingress: disabled
- kube-dns: enabled
- metrics-server: disabled
- registry: disabled
- registry-creds: disabled
- storage-provisioner: enabled

# enable the heapster addon
$ minikube addons enable heapster
heapster was successfully enabled

$ minikube addons enable metrics-server
metrics-server was successfully enabled
```

### Open `minikube` Dashboard
```bash
minikube dashboard
```

### Open `heapster` Dashboard
```bash
minikube addons open heapster
```

