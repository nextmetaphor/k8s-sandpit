## Deploy into Kubernetes
```bash
# open the dashboard
minikube dashboard

# show nodes - these are the compute instances running kubernetes

# kubectl

# create a namespace to isolate our components
kubectl create -f _k8s/namespace/demo-namespace.yaml

# create deployments for our java api
kubectl create -f _k8s/java-sample-api/deployment.yaml
kubectl create -f _k8s/java-sample-api/service.yaml

curl -s http://`minikube ip`:30080/person/200 | json_pp

# let's scale up the number of pods
kubectl scale --replicas=3 -f _k8s/java-sample-api/deployment.yaml 

# kill some containers and watch the replication controller create some more

# create an autoscaler
kubectl autoscale deployment java-sample-api --cpu-percent=50 --min=1 --max=10 --namespace demo

# or manually...
kubectl create -f _k8s/java-sample-api/horizontal-autoscaler.yaml

kubectl get pods --namespace=kube-system
kubectl get hpa --namespace demo

# cleanup
kubectl delete -f _k8s/java-sample-api/service.yaml
kubectl delete hpa java-sample-api --namespace demo
kubectl delete -f _k8s/java-sample-api/deployment.yaml


open http://`minikube ip`:4194/containers/

# start command
minikube start --extra-config=controller-manager.HorizontalPodAutoscalerUseRESTClients=false

# set time command
minikube ssh -- docker run -i --rm --privileged --pid=host debian nsenter -t 1 -m -u -n -i date -u $(date -u +%m%d%H%M%Y)
```