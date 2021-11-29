# Helm Hello World

## Technologies
> helm  | k8s | ngnix | kind

## Requirements
- Kind - https://kind.sigs.k8s.io/docs/user/quick-start/
- Helm - `brew install helm`
- Kubernetes (Docker Desktop)
- Lens https://k8slens.dev/ or Kubernetes dashboard

## Kind cluster
  ```
  kind create cluster --name local --config=kind.yaml
  kind delete cluster --name local
  ```

## Build image
    cd helloworld-nginx
    docker build -t helloworld-nginx .

If you want test it local you can execute: `docker run -p 80:80 hello-world`

Open your browser and check http://localhost

## Push image for docker hub
    docker login
    docker tag helloworld-nginx joerison/helloworld-nginx
    docker push joerison/helloworld-nginx:latest

## Install, delete, upgrade helm chart
    helm install helloworld ./helloworld-chart
    helm delete helloworld
    helm upgrade helloworld ./helloworld-chart

## If you want to create new chart execute:
    helm create helloworld-chart

## kubectl commands
- Display services: `kubectl get svc --watch`

## Generate helm package
    helm package helloworld-chart --debug
    helm install helloworld helloworld-chart-0.1.0.tgz

## Kubernetes dashboard
### Install
    helm repo add kubernetes-dashboard https://kubernetes.github.io/dashboard/
    helm install dashboard kubernetes-dashboard/kubernetes-dashboard -n kubernetes-dashboard --create-namespace
    kubectl proxy
    http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:dashboard-kubernetes-dashboard:https/proxy/#/login

### Delete
    kubectl get deployments -A
    kubectl delete deployment dashboard-kubernetes-dashboard  --namespace=kubernetes-dashboard

### Generate access for kubernetes dashboard
    kubectl apply -f service-account.yaml
    kubectl describe serviceaccount admin-user -n kubernetes-dashboard
    kubectl describe secret admin-user-token-7pfdb -n kubernetes-dashboard
