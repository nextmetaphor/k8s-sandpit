# Demo
This demo walks through the build of simplistic golang and Java APIs, and demonstrates how each can be packaged as a container and deployed to Kubernetes.

## What Do Containers Look Like?
A decent reference is the diagram at <https://www.docker.com/what-container>.

## go-lang Based API
```bash
cd simple-golang-api
open .
```

### Build
```bash
# clean and verify the built file doesn't exist
go clean
ls -l simple-golang-api

# build and verify the built file exists
go build -i
ls -l simple-golang-api

# start the API
./simple-golang-api

# verify success
curl -i http://localhost:8080/
curl -s http://localhost:8080/ | json_pp
```

### Package as Container
```bash
# use the minikube Docker environment
eval $(minikube docker-env)

# set the target environment and build for a Linux container
export GOOS=linux
export GOARCH=amd64

# build a Docker image
go build -i
docker build . -t nextmetaphor/simple-golang-api:latest

# verify the image is created
docker images

# run and verify
docker run --rm -p 8080:8080 -d --name simple-golang-api nextmetaphor/simple-golang-api 
curl -i http://`minikube ip`:8080/
curl -s http://`minikube ip`:8080/ | json_pp

# kill the container
docker stop simple-golang-api 
```

## Java-Based API
```bash
cd java-microservice-api

# open the folder to show contents graphically
open .
```

### Build
```bash
# clean and verify that the war file doesn't exist
gradle clean
ls -l build/libs/java-microservice-api.war 

# build and verify that the file exists
gradle clean build
ls -l build/libs/java-microservice-api.war 

# start the API
java -jar build/libs/java-microservice-api.war 

# verify success
curl -i http://localhost:8080/person/200
curl -s http://localhost:8080/person/200 | json_pp
```

### Package as Container
```bash
# use the minikube docker environment
eval $(minikube docker-env)

# view the contents of the Dockerfile
cat Dockerfile

# build a Docker image
docker build . -t nextmetaphor/java-microservice-api:latest

# verify the image is created
docker images

# run and verify
docker run --rm -p 8080:8080 -d --name java-microservice-api nextmetaphor/java-microservice-api 

# verify success
curl -i http://`minikube ip`:8080/person/200
curl -s http://`minikube ip`:8080/person/200 | json_pp

# kill the container
docker stop java-microservice-api
```

## Deploy into Kubernetes
```bash
# open the dashboard
minikube dashboard

kubectl create -f _k8s/demo-namespace.yaml
```