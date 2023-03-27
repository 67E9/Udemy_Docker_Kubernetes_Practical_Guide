# create container and put it on dockerhub

docker build -t kub-first-app .
docker tag kub-first-app pmssdtest/kub-first-app

# start minikube cluster

minikube start --driver=docker

# run with kubectl (imperative approach)

kubectl help 
- list of instructions

kubectl create deployment first-app --image=pmssdtest/kub-first-app
- first-app = name of deployment
- you will not be able to use images on your local isntallation because minikub runs on a different (virtual) machine
- there image must be pushed on dockerhub or similar

kubectl get deployments 
- see existing deployments

kubectl delete deployment first-app
- delete first-app deployment again

kubectl expose deployment first-app --type=LoadBalancer --port 8080
- exposes port 8080 on a deployment by creating a service
- types: ClusterIP (default), NodePort (accessible from outside), LoadBalancer (distributes traffic and provides static IP)

kubectl get services
- see running services
- in minikube external IP for Loadbalancer will stay pending!
- minikube can map a port to an IP: see below

minikube service first-app
- gives you address where you can see your deployment

kubectl get pods 
- see running pods

kubectl scale deployment/first-app --replicas=3
- run three instances of the pod
- loadbalancer will distribute requests to them

kubectl set image deployment/first-app kub-first-app=pmssdtest/kub-first-app:2
- updates container in pod
- replaces image kub-first-image with new version from dockerhub repository pmssdtest
- new images will only eb updated if they have a new tag in docker
-:2 is the new tag here (version)

kubectl rollout undo deployment/first-app
- rollback an update of the deployment (1 step)
- add --to-revision=<no of revision> to got to specific revision

kubectl rollout history deployment/first-app 
- optional: add --revision=<no of revision> for details on one revision

delete service first-app
delete deplyoment first app
- remove the application again


# notes on services
- services group pods and give them a fixed shared IP (in contrast to the fluid internal IP)
- services can be used to make pods reachable from the outside

# restarts
- when a pod crashed kuberstes will restart it
- when there is more than one crash kubertes will wait longer and longer between restarts to avoid infinite loops