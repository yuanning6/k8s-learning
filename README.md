# k8s-learning

## Software Requirements
* Minicube
* Virtual Machine: Docker
* kubectl

## Getting Start

### Create and check k8s cluster status
```
minikube start --driver=docker
minikube status
```
### Build docker image for hello app version 1
```
docker build . -t yuanningliu/k8s-web-hello
```
### Check if image is available
```
docker images | grep k8s-web
```
### Push image to DockerHub
1. Log into DockerHub 
  ```
  docker login
  ```
2. Push 
  ```
  docker push yuanningliu/k8s-web-hello
  ```
### Create k8s deployment
1. Check current deployment and service
  ```
  alias k=kubectl
  k get deploy
  k get svc
  ```
2. Create deployment
  ```
  k create deployment k8s-web-hello --image=yuanningliu/k8s-web-hello
  ```
3. Check Pods
  ```
  k get pods
  ```
### Create service
1. Expose port, create cluster IP
  ```
  k expose deployment k8s-web-hello --port=3000
  ```
2. SSH into node
  ```
  minikube ssh
  ```
3. Get message from express web server
  ```
  curl [Cluster IP]:[port]; echo
  ```
  <img width="344" alt="Screen Shot 2023-03-12 at 2 18 55 PM" src="https://user-images.githubusercontent.com/45492838/224567751-abf98dd8-554d-411e-ae2c-aba0ba750da0.png">

### Scale deployment
```
k scale deployment k8s-web-hello --replicas=4
```
<img width="351" alt="Screen Shot 2023-03-12 at 2 24 04 PM" src="https://user-images.githubusercontent.com/45492838/224568041-73e38955-ac3d-47a2-a7e5-adf589110622.png">

### Create NodePort service
```
k expose deployment k8s-web-hello --type=NodePort --port=3000
```
Get the service
```
minikube service k8s-web-hello
```
<img width="335" alt="Screen Shot 2023-03-12 at 2 30 00 PM" src="https://user-images.githubusercontent.com/45492838/224568436-0081b7b6-9d67-4bc8-a691-5790e056ab3b.png">

### Create LoadBalancer service
```
k expose deployment k8s-web-hello --type=LoadBalancer --port=3000
```

### Roll out update
```
k set image deployment k8s-web-hello k8s-web-hello=yuanningliu/k8s-web-hello:2.0.0
k rollout status deploy k8s-web-hello
```
<img width="414" alt="Screen Shot 2023-03-12 at 2 39 13 PM" src="https://user-images.githubusercontent.com/45492838/224568865-9c80721b-1a45-40a6-ba29-18d7dfc2b615.png">

### Apply yaml deployment and service file
```
k apply -f deployment.yaml
k apply -f service.yaml
```
### Delete deployment and service
```
k delete -f deployment.yaml -f service.yaml
```

## Stop minikube
```
minikube stop
```

