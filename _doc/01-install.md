# Install
Instructions on installing and configuring `minikube` on macOS.

## Installing `minikube`
### Download `minikube`
At the time of writing, the recommended approach is to use `homebrew` should be used; the install command is as follows:
```bash
$ brew install minikube
```
Refer to <<https://minikube.sigs.k8s.io/docs/start/>> for detailed instructions for installing `minikube`.

### Install `kubectl`
To install `kubectl` using Homebrew, the command is as follows:

```bash
$ brew install kubernetes-cli
```
Refer to <https://kubernetes.io/docs/tasks/tools/install-kubectl-macos/#install-with-homebrew-on-macos> for further details on installing `kubectl`. 

## Starting and Configuring `minikube`
### Start `minikube`
Execute the following command to start `minikube`, allocating a decent amount of resource to run it. 

```bash
$ minikube start --cpus 4 --memory 8192
 ```

### Enable the `metrics-server` and `heapster` Addons
In order for horizontal pod autoscaling to work, we need to ensure that the `heapster` and `metrics-server` addons are enabled. Refer to <https://github.com/kubernetes/minikube/tree/master/deploy/addons> for more information concerning addons.

To check the current status of the `minikube` addons, execute the following command.
```bash
$ minikube addons list
|-----------------------------|----------|--------------|
|         ADDON NAME          | PROFILE  |    STATUS    |
|-----------------------------|----------|--------------|
| ambassador                  | minikube | disabled     |
| auto-pause                  | minikube | disabled     |
| csi-hostpath-driver         | minikube | disabled     |
| dashboard                   | minikube | disabled     |
| default-storageclass        | minikube | enabled ✅   |
| efk                         | minikube | disabled     |
| freshpod                    | minikube | disabled     |
| gcp-auth                    | minikube | disabled     |
| gvisor                      | minikube | disabled     |
| helm-tiller                 | minikube | disabled     |
| ingress                     | minikube | disabled     |
| ingress-dns                 | minikube | disabled     |
| istio                       | minikube | disabled     |
| istio-provisioner           | minikube | disabled     |
| kubevirt                    | minikube | disabled     |
| logviewer                   | minikube | disabled     |
| metallb                     | minikube | disabled     |
| metrics-server              | minikube | disabled     |
| nvidia-driver-installer     | minikube | disabled     |
| nvidia-gpu-device-plugin    | minikube | disabled     |
| olm                         | minikube | disabled     |
| pod-security-policy         | minikube | disabled     |
| registry                    | minikube | disabled     |
| registry-aliases            | minikube | disabled     |
| registry-creds              | minikube | disabled     |
| storage-provisioner         | minikube | enabled ✅   |
| storage-provisioner-gluster | minikube | disabled     |
| volumesnapshots             | minikube | disabled     |
|-----------------------------|----------|--------------|
```

Ensure that the `metrics-server` addon is enabled by executing the following command.
```bash
$ minikube addons enable metrics-server
    ▪ Using image k8s.gcr.io/metrics-server/metrics-server:v0.4.2
🌟  The 'metrics-server' addon is enabled
```

Check that all components are running:

```bash
$ kubectl get pods --namespace=kube-system
NAME                               READY   STATUS    RESTARTS   AGE
coredns-74ff55c5b-m2npc            1/1     Running   0          3m
etcd-minikube                      1/1     Running   0          3m15s
kube-apiserver-minikube            1/1     Running   0          3m15s
kube-controller-manager-minikube   1/1     Running   0          3m15s
kube-proxy-fvhlb                   1/1     Running   0          3m
kube-scheduler-minikube            1/1     Running   0          3m14s
metrics-server-7894db45f8-qxnt4    1/1     Running   0          51s
storage-provisioner                1/1     Running   1          3m14s
```

### Open `minikube` Dashboard
This is the main `minikube` dashboard.

```bash
$ minikube dashboard
```

### View the `kubernetes` Nodes
```bash
$ kubectl get nodes
NAME       STATUS   ROLES                  AGE     VERSION
minikube   Ready    control-plane,master   8m29s   v1.20.2
```
```bash
$ kubectl get nodes
NAME       STATUS   ROLES                  AGE     VERSION
minikube   Ready    control-plane,master   8m29s   v1.20.2
k8s-sandpit $ kubectl describe nodes minikube
Name:               minikube
Roles:              control-plane,master
Labels:             beta.kubernetes.io/arch=amd64
                    beta.kubernetes.io/os=linux
                    kubernetes.io/arch=amd64
                    kubernetes.io/hostname=minikube
                    kubernetes.io/os=linux
                    minikube.k8s.io/commit=15cede53bdc5fe242228853e737333b09d4336b5
                    minikube.k8s.io/name=minikube
                    minikube.k8s.io/updated_at=2021_05_05T06_20_21_0700
                    minikube.k8s.io/version=v1.19.0
                    node-role.kubernetes.io/control-plane=
                    node-role.kubernetes.io/master=
Annotations:        kubeadm.alpha.kubernetes.io/cri-socket: /var/run/dockershim.sock
                    node.alpha.kubernetes.io/ttl: 0
                    volumes.kubernetes.io/controller-managed-attach-detach: true
CreationTimestamp:  Wed, 05 May 2021 06:20:18 +0100
Taints:             <none>
Unschedulable:      false
Lease:
  HolderIdentity:  minikube
  AcquireTime:     <unset>
  RenewTime:       Wed, 05 May 2021 06:29:01 +0100
Conditions:
  Type             Status  LastHeartbeatTime                 LastTransitionTime                Reason                       Message
  ----             ------  -----------------                 ------------------                ------                       -------
  MemoryPressure   False   Wed, 05 May 2021 06:27:53 +0100   Wed, 05 May 2021 06:20:14 +0100   KubeletHasSufficientMemory   kubelet has sufficient memory available
  DiskPressure     False   Wed, 05 May 2021 06:27:53 +0100   Wed, 05 May 2021 06:20:14 +0100   KubeletHasNoDiskPressure     kubelet has no disk pressure
  PIDPressure      False   Wed, 05 May 2021 06:27:53 +0100   Wed, 05 May 2021 06:20:14 +0100   KubeletHasSufficientPID      kubelet has sufficient PID available
  Ready            True    Wed, 05 May 2021 06:27:53 +0100   Wed, 05 May 2021 06:20:31 +0100   KubeletReady                 kubelet is posting ready status
Addresses:
  InternalIP:  192.168.49.2
  Hostname:    minikube
Capacity:
  cpu:                4
  ephemeral-storage:  61255492Ki
  hugepages-1Gi:      0
  hugepages-2Mi:      0
  memory:             8401816Ki
  pods:               110
Allocatable:
  cpu:                4
  ephemeral-storage:  61255492Ki
  hugepages-1Gi:      0
  hugepages-2Mi:      0
  memory:             8401816Ki
  pods:               110
System Info:
  Machine ID:                 73c9fceff7724090ba72b64bf6e8eff8
  System UUID:                5f3206b7-95ab-4cef-912c-d9f799288f63
  Boot ID:                    82518345-c9f6-49c4-8c83-4eb3186e4cc1
  Kernel Version:             4.19.121-linuxkit
  OS Image:                   Ubuntu 20.04.1 LTS
  Operating System:           linux
  Architecture:               amd64
  Container Runtime Version:  docker://20.10.5
  Kubelet Version:            v1.20.2
  Kube-Proxy Version:         v1.20.2
PodCIDR:                      10.244.0.0/24
PodCIDRs:                     10.244.0.0/24
Non-terminated Pods:          (10 in total)
  Namespace                   Name                                         CPU Requests  CPU Limits  Memory Requests  Memory Limits  Age
  ---------                   ----                                         ------------  ----------  ---------------  -------------  ---
  kube-system                 coredns-74ff55c5b-m2npc                      100m (2%)     0 (0%)      70Mi (0%)        170Mi (2%)     8m35s
  kube-system                 etcd-minikube                                100m (2%)     0 (0%)      100Mi (1%)       0 (0%)         8m50s
  kube-system                 kube-apiserver-minikube                      250m (6%)     0 (0%)      0 (0%)           0 (0%)         8m50s
  kube-system                 kube-controller-manager-minikube             200m (5%)     0 (0%)      0 (0%)           0 (0%)         8m50s
  kube-system                 kube-proxy-fvhlb                             0 (0%)        0 (0%)      0 (0%)           0 (0%)         8m35s
  kube-system                 kube-scheduler-minikube                      100m (2%)     0 (0%)      0 (0%)           0 (0%)         8m49s
  kube-system                 metrics-server-7894db45f8-qxnt4              100m (2%)     0 (0%)      300Mi (3%)       0 (0%)         6m26s
  kube-system                 storage-provisioner                          0 (0%)        0 (0%)      0 (0%)           0 (0%)         8m49s
  kubernetes-dashboard        dashboard-metrics-scraper-f6647bd8c-h2rfl    0 (0%)        0 (0%)      0 (0%)           0 (0%)         5m13s
  kubernetes-dashboard        kubernetes-dashboard-968bcb79-9lxq9          0 (0%)        0 (0%)      0 (0%)           0 (0%)         5m13s
Allocated resources:
  (Total limits may be over 100 percent, i.e., overcommitted.)
  Resource           Requests    Limits
  --------           --------    ------
  cpu                850m (21%)  0 (0%)
  memory             470Mi (5%)  170Mi (2%)
  ephemeral-storage  100Mi (0%)  0 (0%)
  hugepages-1Gi      0 (0%)      0 (0%)
  hugepages-2Mi      0 (0%)      0 (0%)
Events:
  Type    Reason                   Age    From        Message
  ----    ------                   ----   ----        -------
  Normal  Starting                 8m50s  kubelet     Starting kubelet.
  Normal  NodeHasSufficientMemory  8m50s  kubelet     Node minikube status is now: NodeHasSufficientMemory
  Normal  NodeHasNoDiskPressure    8m50s  kubelet     Node minikube status is now: NodeHasNoDiskPressure
  Normal  NodeHasSufficientPID     8m50s  kubelet     Node minikube status is now: NodeHasSufficientPID
  Normal  NodeNotReady             8m50s  kubelet     Node minikube status is now: NodeNotReady
  Normal  NodeAllocatableEnforced  8m50s  kubelet     Updated Node Allocatable limit across pods
  Normal  NodeReady                8m40s  kubelet     Node minikube status is now: NodeReady
  Normal  Starting                 8m34s  kube-proxy  Starting kube-proxy.
```

### Tag and Untag the `kubernetes` Nodes
Tags can be added to nodes to ensure that containers are correctly targeted. 

```bash
$ kubectl label nodes minikube az=az1
node/minikube labeled
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

### Complete Cleanup
To remove all trace of the `minikube` VM, use the following command:

```bash
$ minikube delete --all --purge
```