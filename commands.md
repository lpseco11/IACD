Comandos projeto 1 IACD

## Assignment 1 - Container Basics

(mongo) - docker pull mongo
(backend) - docker build -t backend backend/
(frontend) - docker build -t frontend frontend/

(backend) docker run -d -p 80:80(port mapping) --name back_container backend(image)

(frontend) docker run -d -p 3000:3000(port mapping) --name front_container frontend(image)

(mongo) docker run -d -p 27017:27017 --name mongo_container mongo

Assignment 2 - Container connectivity and Volumes

(network) - docker network create net

(backend) - docker build -t backend:network backend/

(mongo) - docker volume create mongo_vol

(backend) - docker volume create backend_vol

(mongo) docker run -d --name mongo_container2 --network net -v mongo_vol:/data/db mongo

(backend) - docker run -d --name back_container2 --network net -v backend_vol:/logs -p 80:80 backend:network

(frontend) - docker run -d --name front_container2 -p 3000:3000 frontend

## Assignment 3 - Docker compose

docker tag backend:network lpseco11/iacd_backend:network

docker tag frontend lpseco11/iacd_frontend

docker push lpseco11/iacd_backend:network 

docker push lpseco11/iacd_frontend 

docker compose up -d

docker compose down 

## Assignment 4 - Docker compose

### Imperative approach
minikube start

kubectl create namespace iacd

kubectl create deployment mongodb --namespace=iacd --image=mongo --port=27017 
kubectl expose deployment mongodb --namespace=iacd --port=27017 --target-port=27017 --name=mongodb

docker build -t project-backend ./backend
minikube image load project-backend
kubectl create deployment backend --namespace=iacd --replicas=2 --image=project-backend --port=80
kubectl edit deployment backend -n iacd (change the image pull policy to Never)
kubectl expose deployment backend --namespace=iacd --type=LoadBalancer --port=80 --target-port=80 --name=backend-service

docker build -t project-frontend ./frontend
minikube image load project-frontend
kubectl create deployment frontend --namespace=iacd --replicas=3 --image=project-frontend --port=3000
kubectl edit deployment frontend -n iacd (change the image pull policy to Never)
kubectl expose deployment frontend --namespace=iacd --type=LoadBalancer --port=3000 --target-port=3000 --name=frontend-service

minikube tunnel

### Declarative approach

kubectl apply -f ./deployments
kubectl apply -f ./services




## Assignment 5 - Persistant Data Claim

kubectl apply -f ./data_persistance




kubectl get deployments -n iacd
kubectl get services -n iacd
kubectl get pods -n iacd

kubectl delete all --all -n iacd
kubectl logs mongodb-5dfcb4ff5b-w9z46 -n iacd
