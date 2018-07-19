# Install
Instructions on installing and configuring `minikube`.

## Installing `minikube`
### Download `minikube`
At the time of writing, the current version is `0.28.1` and the install command is as follows:

```bash
curl -Lo minikube https://storage.googleapis.com/minikube/releases/v0.28.1/minikube-darwin-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/
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
$ minikube dashboard
```

### Open `heapster` Dashboard
This is a separate operational monitoring dashboard, provided by the 

```bash
$ minikube addons open heapster
```

### View the `kubernetes` Nodes
```bash
$ kubectl get nodes
NAME       STATUS    ROLES     AGE       VERSION
minikube   Ready     master    1d        v1.10.0

$ kubectl describe nodes minikube
Name:               minikube
Roles:              master
Labels:             beta.kubernetes.io/arch=amd64
                    beta.kubernetes.io/os=linux
                    kubernetes.io/hostname=minikube
                    node-role.kubernetes.io/master=
Annotations:        node.alpha.kubernetes.io/ttl=0
                    volumes.kubernetes.io/controller-managed-attach-detach=true
CreationTimestamp:  Wed, 09 May 2018 07:51:16 +0100
Taints:             <none>
Unschedulable:      false
Conditions:
  Type             Status  LastHeartbeatTime                 LastTransitionTime                Reason                       Message
  ----             ------  -----------------                 ------------------                ------                       -------
  OutOfDisk        False   Fri, 11 May 2018 07:08:52 +0100   Wed, 09 May 2018 08:47:13 +0100   KubeletHasSufficientDisk     kubelet has sufficient disk space available
  MemoryPressure   False   Fri, 11 May 2018 07:08:52 +0100   Wed, 09 May 2018 08:47:13 +0100   KubeletHasSufficientMemory   kubelet has sufficient memory available
  DiskPressure     False   Fri, 11 May 2018 07:08:52 +0100   Wed, 09 May 2018 08:47:13 +0100   KubeletHasNoDiskPressure     kubelet has no disk pressure
  PIDPressure      False   Fri, 11 May 2018 07:08:52 +0100   Wed, 09 May 2018 07:51:12 +0100   KubeletHasSufficientPID      kubelet has sufficient PID available
  Ready            True    Fri, 11 May 2018 07:08:52 +0100   Thu, 10 May 2018 08:01:26 +0100   KubeletReady                 kubelet is posting ready status
Addresses:
  InternalIP:  192.168.64.25
  Hostname:    minikube
Capacity:
 cpu:                4
 ephemeral-storage:  16055720Ki
 hugepages-2Mi:      0
 memory:             8175052Ki
 pods:               110
Allocatable:
 cpu:                4
 ephemeral-storage:  14796951528
 hugepages-2Mi:      0
 memory:             8072652Ki
 pods:               110
System Info:
 Machine ID:                 83b8e8b9102b473db7f4077339d3d911
 System UUID:                349B11E8-0000-0000-942C-6A0000EC8DE0
 Boot ID:                    c574e16b-e2a5-43cb-9054-d42761e8fe47
 Kernel Version:             4.9.64
 OS Image:                   Buildroot 2017.11
 Operating System:           linux
 Architecture:               amd64
 Container Runtime Version:  docker://17.12.1-ce
 Kubelet Version:            v1.10.0
 Kube-Proxy Version:         v1.10.0
ExternalID:                  minikube
Non-terminated Pods:         (13 in total)
  Namespace                  Name                                     CPU Requests  CPU Limits  Memory Requests  Memory Limits
  ---------                  ----                                     ------------  ----------  ---------------  -------------
  demo                       java-sample-api-774d564cdd-pv8bp         500m (12%)    0 (0%)      0 (0%)           0 (0%)
  kube-system                etcd-minikube                            0 (0%)        0 (0%)      0 (0%)           0 (0%)
  kube-system                heapster-5wx5l                           0 (0%)        0 (0%)      0 (0%)           0 (0%)
  kube-system                influxdb-grafana-hlgtg                   0 (0%)        0 (0%)      0 (0%)           0 (0%)
  kube-system                kube-addon-manager-minikube              5m (0%)       0 (0%)      50Mi (0%)        0 (0%)
  kube-system                kube-apiserver-minikube                  250m (6%)     0 (0%)      0 (0%)           0 (0%)
  kube-system                kube-controller-manager-minikube         200m (5%)     0 (0%)      0 (0%)           0 (0%)
  kube-system                kube-dns-86f4d74b45-5vd6w                260m (6%)     0 (0%)      110Mi (1%)       170Mi (2%)
  kube-system                kube-proxy-f87hq                         0 (0%)        0 (0%)      0 (0%)           0 (0%)
  kube-system                kube-scheduler-minikube                  100m (2%)     0 (0%)      0 (0%)           0 (0%)
  kube-system                kubernetes-dashboard-5498ccf677-jc5sb    0 (0%)        0 (0%)      0 (0%)           0 (0%)
  kube-system                metrics-server-85c979995f-rmn2d          0 (0%)        0 (0%)      0 (0%)           0 (0%)
  kube-system                storage-provisioner                      0 (0%)        0 (0%)      0 (0%)           0 (0%)
Allocated resources:
  (Total limits may be over 100 percent, i.e., overcommitted.)
  CPU Requests  CPU Limits  Memory Requests  Memory Limits
  ------------  ----------  ---------------  -------------
  1315m (32%)   0 (0%)      160Mi (2%)       170Mi (2%)
Events:
  Type    Reason        Age               From               Message
  ----    ------        ----              ----               -------
  Normal  NodeNotReady  23h               kubelet, minikube  Node minikube status is now: NodeNotReady
  Normal  NodeReady     23h (x2 over 1d)  kubelet, minikube  Node minikube status is now: NodeReady
```

### Tag and Untag the `kubernetes` Nodes
Tags can be added to nodes to ensure that containers are correctly targeted. 

```bash
$ kubectl label nodes minikube az=az1
node "minikube" labeled
```

Tags can also be removed from nodes.

```bash
$ kubectl label nodes minikube az-
node "minikube" labeled
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
$ minikube ssh -- docker run -i --rm --privileged --pid=host debian nsenter -t 1 -m -u -n -i date -u $(date -u +%m%d%H%M%Y)
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