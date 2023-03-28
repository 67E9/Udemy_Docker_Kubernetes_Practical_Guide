# check prerequisites

start Docker Desktop

minikube status
- check if minikube is running

minikube start --driver=docker 
- start minikube

kubectl get deployments
kubectl get services
- check if there is a running deployment, custom service

# users-app

note: replace axios calls with dummy objects of the correct type until the auth app is available

cd users-api
docker build -t pmssdtest/kub-demo-users .
docker push pmssdtest/kub-demo-users          
kubectl apply -f=kubernetes/user-deployment.yaml

# service

note: gives us stable address and allow aoutside world access

kubectl apply -f=kubernetes/user-service.yaml

minikube service users-service => get external access in minikube

# preparation for auth-app

notes: 
- axios calls in users-app where commented in again
- then, the url for this case was made configurable using env vars

# auth-app

docker build -t pmssdtest/kub-demo-auth auth-api/.
docker push pmssdtest/kub-demo-auth

note: add second image to the pod inside user-deployment
=> two containers in one pod and deployment
=> refer to user-deployment.yaml

# update user-app
docker build -t pmssdtest/kub-demo-users users-api/.
docker push pmssdtest/kub-demo-users

# pod internal communication between containers

notes: 
- use localhost 
- refer to "env" in user-deployment.yaml

kubectl apply -f=kubernetes/user-deployment.yaml

kubectl get pods
NAME                              READY   STATUS    RESTARTS   AGE
users-deployment-bd6b69b6-hncm4   2/2     Running   0          2m41s
=> 2/2 means both containers are running

# sidenote: debugging, how to get logs?
kubectl logs <podname> name=<myLabel> ctl