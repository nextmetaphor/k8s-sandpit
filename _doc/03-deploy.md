## Deploy
#### Open The Dashboard
```bash
minikube dashboard
```

#### Show Nodes
These are the compute instances running Kubernetes.

```bash
$ kubectl get nodes
NAME       STATUS    ROLES     AGE       VERSION
minikube   Ready     master    17m       v1.10.0

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
  OutOfDisk        False   Wed, 09 May 2018 08:08:47 +0100   Wed, 09 May 2018 07:51:12 +0100   KubeletHasSufficientDisk     kubelet has sufficient disk space available
  MemoryPressure   False   Wed, 09 May 2018 08:08:47 +0100   Wed, 09 May 2018 07:51:12 +0100   KubeletHasSufficientMemory   kubelet has sufficient memory available
  DiskPressure     False   Wed, 09 May 2018 08:08:47 +0100   Wed, 09 May 2018 07:51:12 +0100   KubeletHasNoDiskPressure     kubelet has no disk pressure
  PIDPressure      False   Wed, 09 May 2018 08:08:47 +0100   Wed, 09 May 2018 07:51:12 +0100   KubeletHasSufficientPID      kubelet has sufficient PID available
  Ready            True    Wed, 09 May 2018 08:08:47 +0100   Wed, 09 May 2018 07:51:12 +0100   KubeletReady                 kubelet is posting ready status
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
  demo                       java-sample-api-774d564cdd-f9frb         500m (12%)    0 (0%)      0 (0%)           0 (0%)
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
  Type    Reason                   Age                From                  Message
  ----    ------                   ----               ----                  -------
  Normal  NodeHasSufficientPID     18m (x5 over 18m)  kubelet, minikube     Node minikube status is now: NodeHasSufficientPID
  Normal  NodeAllocatableEnforced  18m                kubelet, minikube     Updated Node Allocatable limit across pods
  Normal  NodeHasSufficientDisk    18m (x6 over 18m)  kubelet, minikube     Node minikube status is now: NodeHasSufficientDisk
  Normal  NodeHasSufficientMemory  18m (x6 over 18m)  kubelet, minikube     Node minikube status is now: NodeHasSufficientMemory
  Normal  NodeHasNoDiskPressure    18m (x6 over 18m)  kubelet, minikube     Node minikube status is now: NodeHasNoDiskPressure
  Normal  Starting                 17m                kube-proxy, minikube  Starting kube-proxy.
```


#### Create Namespace
Having a separate namespace helps to isolate the various components. Create and verify as follows.

```bash
$ kubectl create -f _k8s/namespace/demo-namespace.yaml
  namespace "demo" created

$ kubectl get ns
NAME          STATUS    AGE
default       Active    7m
demo          Active    56s
kube-public   Active    7m
kube-system   Active    7m

$ kubectl describe ns demo
Name:         demo
Labels:       <none>
Annotations:  <none>
Status:       Active

No resource quota.

No resource limits.
```

#### Create a deployment for the Java API
The deployment determines how many containers are instantiated and where.

```bash
$ kubectl create -f _k8s/java-sample-api/deployment.yaml
deployment.apps "java-sample-api" created

$ kubectl get deploy --namespace demo
NAME              DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
java-sample-api   1         1         1            1           3s

$ kubectl describe deploy java-sample-api --namespace demo
Name:                   java-sample-api
Namespace:              demo
CreationTimestamp:      Wed, 09 May 2018 08:05:35 +0100
Labels:                 app=java-sample-api
Annotations:            deployment.kubernetes.io/revision=1
Selector:               app=java-sample-api
Replicas:               1 desired | 1 updated | 1 total | 1 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=java-sample-api
  Containers:
   java-sample-api:
    Image:      nextmetaphor/java-microservice-api
    Port:       8080/TCP
    Host Port:  0/TCP
    Requests:
      cpu:        500m
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      True    MinimumReplicasAvailable
  Progressing    True    NewReplicaSetAvailable
OldReplicaSets:  <none>
NewReplicaSet:   java-sample-api-774d564cdd (1/1 replicas created)
Events:
  Type    Reason             Age   From                   Message
  ----    ------             ----  ----                   -------
  Normal  ScalingReplicaSet  45s   deployment-controller  Scaled up replica set java-sample-api-774d564cdd to 1
```

#### Create a Service for the Java API
The service is the effectively how the application is 'published'.

```bash
$ kubectl create -f _k8s/java-sample-api/service.yaml
service "java-sample-api" created

$ kubectl get service --namespace demo
NAME              TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
java-sample-api   NodePort   10.105.14.247   <none>        8080:30080/TCP   4m

$ kubectl describe service java-sample-api --namespace demo
Name:                     java-sample-api
Namespace:                demo
Labels:                   <none>
Annotations:              <none>
Selector:                 app=java-sample-api
Type:                     NodePort
IP:                       10.105.14.247
Port:                     <unset>  8080/TCP
TargetPort:               8080/TCP
NodePort:                 <unset>  30080/TCP
Endpoints:                172.17.0.7:8080
Session Affinity:         None
External Traffic Policy:  Cluster
Events:                   <none>
```

#### Test the Java API
Execute a `curl` command against the service.

```bash
$ curl -s http://`minikube ip`:30080/person/200 | json_pp
{
   "id" : 200,
   "workAddressModel" : null,
   "name" : "person-name",
   "age" : 37,
   "homeAddressModel" : {
      "addressLine2" : "home-address-line-2",
      "addressLine1" : "home-address-line-1",
      "houseName" : "house-name",
      "county" : "county",
      "houseNumber" : 1,
      "town" : null,
      "country" : "country",
      "addressLine3" : "home-address-line-3",
      "postcode" : "postcode"
   }
}
```

#### Scale up the Number of Pods
Execute the following command, then delete some pods from dashboard and observe replacements being automatically created.

```bash
$ kubectl scale --replicas=3 -f _k8s/java-sample-api/deployment.yaml 
deployment.apps "java-sample-api" scaled

$ kubectl describe deploy java-sample-api --namespace demo
Name:                   java-sample-api
Namespace:              demo
CreationTimestamp:      Wed, 09 May 2018 17:33:11 +0100
Labels:                 app=java-sample-api
Annotations:            deployment.kubernetes.io/revision=1
Selector:               app=java-sample-api
Replicas:               3 desired | 3 updated | 3 total | 3 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=java-sample-api
  Containers:
   java-sample-api:
    Image:      nextmetaphor/java-microservice-api
    Port:       8080/TCP
    Host Port:  0/TCP
    Requests:
      cpu:        500m
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Progressing    True    NewReplicaSetAvailable
  Available      True    MinimumReplicasAvailable
OldReplicaSets:  <none>
NewReplicaSet:   java-sample-api-774d564cdd (3/3 replicas created)
Events:
  Type    Reason             Age   From                   Message
  ----    ------             ----  ----                   -------
  Normal  ScalingReplicaSet  1m    deployment-controller  Scaled up replica set java-sample-api-774d564cdd to 1
  Normal  ScalingReplicaSet  22s   deployment-controller  Scaled up replica set java-sample-api-774d564cdd to 3
```

#### Create a Horizontal Autoscaling Group
Generate a horizontal autoscaling group for the deployment which will scale the number of pods should the CPU rise to 50%.

```bash
$ kubectl create -f _k8s/java-sample-api/horizontal-autoscaler.yaml
horizontalpodautoscaler.autoscaling "java-sample-api" created

# ...or use a command such as ...
# kubectl autoscale deployment java-sample-api --cpu-percent=50 --min=1 --max=10 --namespace demo

$ kubectl get hpa --namespace demo
NAME              REFERENCE                    TARGETS         MINPODS   MAXPODS   REPLICAS   AGE
java-sample-api   Deployment/java-sample-api   <unknown>/50%   1         10        0          13s

# ...wait 30 seconds before running the command below in order that the metrics are collected
$ kubectl get hpa --namespace demo
NAME              REFERENCE                    TARGETS   MINPODS   MAXPODS   REPLICAS   AGE
java-sample-api   Deployment/java-sample-api   0%/50%    1         10        1          38s

$ kubectl describe hpa java-sample-api --namespace demo
Name:                                                  java-sample-api
Namespace:                                             demo
Labels:                                                <none>
Annotations:                                           <none>
CreationTimestamp:                                     Wed, 09 May 2018 08:32:16 +0100
Reference:                                             Deployment/java-sample-api
Metrics:                                               ( current / target )
  resource cpu on pods  (as a percentage of request):  0% (2m) / 50%
Min replicas:                                          1
Max replicas:                                          10
Conditions:
  Type            Status  Reason            Message
  ----            ------  ------            -------
  AbleToScale     True    ReadyForNewScale  the last scale time was sufficiently old as to warrant a new scale
  ScalingActive   True    ValidMetricFound  the HPA was able to successfully calculate a replica count from cpu resource utilization (percentage of request)
  ScalingLimited  True    TooFewReplicas    the desired replica count is increasing faster than the maximum scale rate
Events:           <none>
```

#### Generate Load using `jmeter` 
Open `jmeter` and run the provided `_jmeter/jmeter-test.jmx` script, changing the server address to the IP address of the `minikube` instance, which can be found by executing `minikube ip`. Observe the number of pods increase accordingly. Also observe the number of pods decrease when the test is stopped.

```bash
$ minikube service --url golang-sample-api --namespace demo
```

#### Cleanup
Remove the horizontal autoscaling group, service, deployment and namespace.

```bash
$ kubectl delete -f _k8s/java-sample-api/horizontal-autoscaler.yaml
horizontalpodautoscaler.autoscaling "java-sample-api" deleted

$ kubectl delete -f _k8s/java-sample-api/service.yaml
service "java-sample-api" deleted

$ kubectl delete -f _k8s/java-sample-api/deployment.yaml
deployment.apps "java-sample-api" deleted

$ kubectl delete -f _k8s/namespace/demo-namespace.yaml
namespace "demo" deleted
```