# Install
Instructions on installing and configuring `minikube`.

## Installing `minikube`
### Download `minikube`
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
To install the `hyperkit` driver, the command is as follows:

```bash
$ curl -LO https://storage.googleapis.com/minikube/releases/latest/docker-machine-driver-hyperkit \
&& chmod +x docker-machine-driver-hyperkit \
&& sudo mv docker-machine-driver-hyperkit /usr/local/bin/ \
&& sudo chown root:wheel /usr/local/bin/docker-machine-driver-hyperkit \
&& sudo chmod u+s /usr/local/bin/docker-machine-driver-hyperkit
```
Refer to <https://github.com/kubernetes/minikube/blob/master/docs/drivers.md#xhyve-driver> for further details on installing driver plugins.

## Starting and Configuring `minikube`
### Start `minikube`
Execute the following command to start `minikube` using the correct driver, and allocating a decent amount of resource to run it. 

```bash
$ minikube start --vm-driver=hyperkit --cpus 4 --memory 8192
 ```

### Enable the `metrics-server` and `heapster` Addons
In order for horizontal pod autoscaling to work, we need to ensure that the `heapster` and `metrics-server` addons are enabled. Refer to <https://github.com/kubernetes/minikube/tree/master/deploy/addons> for more information concerning addons.

To check the current status of the `minikube` addons, execute the following command.
```bash
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
```

Ensure that the `heapster` and `metrics-server` addons are enabled by executing the following commands.
```bash
$ minikube addons enable heapster
heapster was successfully enabled

$ minikube addons enable metrics-server
metrics-server was successfully enabled
```

Check that all components are running:

```bash
$ kubectl get pods --namespace=kube-system
NAME                                    READY     STATUS    RESTARTS   AGE
etcd-minikube                           1/1       Running   0          42m
heapster-5wx5l                          1/1       Running   0          43m
influxdb-grafana-hlgtg                  2/2       Running   0          43m
kube-addon-manager-minikube             1/1       Running   0          43m
kube-apiserver-minikube                 1/1       Running   0          42m
kube-controller-manager-minikube        1/1       Running   0          42m
kube-dns-86f4d74b45-5vd6w               3/3       Running   0          43m
kube-proxy-f87hq                        1/1       Running   0          43m
kube-scheduler-minikube                 1/1       Running   0          42m
kubernetes-dashboard-5498ccf677-jc5sb   1/1       Running   0          43m
metrics-server-85c979995f-rmn2d         1/1       Running   0          43m
storage-provisioner                     1/1       Running   0          43m
```

### Open `minikube` Dashboard
This is the main `minikube` dashboard.

```bash
minikube dashboard
```

### Open `heapster` Dashboard
This is a separate operational monitoring dashboard, provided by the 

```bash
minikube addons open heapster
```

### View the `minikube` Logs
Examine the `minikube` logs as follows:

```bash
$ minikube logs
```

### Tail the `minikube` Logs
Tail (and follow) the `minikube` logs as follows:

```bash
$ minikube logs -f
```

### Resetting the Date and Time in the `minikube` VM
If the date/time on the `minikube` VM gets out of sync, use the following command to reset it.

```bash
minikube ssh -- docker run -i --rm --privileged --pid=host debian nsenter -t 1 -m -u -n -i date -u $(date -u +%m%d%H%M%Y)
```

## Stopping and Removing `minikube`
### Stop `minikube`
Stop the `minikube` VM as follows:

```bash
$ minikube stop
```

### Delete `minikube`
Completely delete the `minikube` VM as follows:

```bash
$ minikube delete
```