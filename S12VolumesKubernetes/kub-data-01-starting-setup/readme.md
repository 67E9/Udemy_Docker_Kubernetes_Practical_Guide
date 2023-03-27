# intro

state: data that must survive

in kubenetes the volume lifetime by default equals the pod lifetime
    - survives container restarts and removals
    - volumes are removed if pods are destroyed/restarted
    - different drivers and types!

# setup

minikube status 
- check if minikube is up and running

kubectl apply -f=deployment.yaml -f=service.yaml
- run the deplyoment and service in minikube

kubectl get deployment
kubectl get pods
kubectl get service 
- verify it's runing

minikube service story-service
- expose service

# volumes 

- are inside of pods 
- share the pod's lifecycle (will be remvoed with the pod)
- are implemtend in the yaml for a deployment
- there are many different types for different kinds of storage e.g.
    - emptyDir: only available withn one pod
    - hostPath: folder on host machien/node, available accross pods on the same node/machine
    - CSI: Container Storage Interface (extensibale with drivers for different cloud services)
    - ...


# persistant volumes
- live on the cluster level, outside of pods and nodes
- persistant volume claims inside the pods => allows to access persitant volume
- detached from nodes and pods
- uses types like CSI
- HostPath is only available in dummy environment like minikube

# storage classes

kubectl get sc
- see storage classes

# setup with persistant valume:
- kubectl apply -f=host-pv.yaml
- kubectl apply -f=host-pvc.yaml
- kubectl apply -f=deployment.yaml

check if running:
- kubectl get pv
- kubectl get pvc
- kubectl get deployments
