# prepare this for deployment
- add free tier mongodb atlas url and credentials
- build images

# create AWS EKS cluster

- name and create a new cluster
1. configure the cluster
    - Kubernetes version: pick newest
    - Cluster Service Role: permissions for underlying ec2 => based on IAM roles
        - create IAM roles in IAM console: 
            1. choose EKS
            2. choose EKS cluster
            3. asign role name
            4. click create role
        - select newly created role
2. configure network for the cluster
    - open cloudformation in new tab:
        1. create stack
        2. https://docs.aws.amazon.com/eks/latest/userguide/creating-a-vpc.html#create-vpc 
        3. paste in url with network template
        4. name stack
        5. create stack => creates a vps netowrk
    - select newly created vpc
    - Cluster Endpoint Access? => set to public and private
    - review settings
    - "create" => create clusters
3. add Worker Nodes:
    - go to compute / add node group
    - name node group
    - add IAM role for nodes
        1. IAM console
        2. select EC2 common use case
        3. next permissions: 
            - add AmazonEKSWorkerNodePolicy
            - add AmazonEKS-CNI_Policy
            - add AmazonEC2ContainerRegistryReadOnly
        4. name IAM role
        5. click create role
        6. select newly created IAM role in node group wizard
    - keep default operating system image
    - select instance type t3 small => in t3 micro apps may fail
    - set scaling policy:
        - select 2 nodes
    - click create to spin up worker nodes and install kubelets, kube proxy etc.

_side notes:_
1. allow remote access to nodes allows to conenct to nodes via ssh
2. in ec2 you can see the running instances


# connect to AWS EKS with kubectl

- folder in user folder: .kube
- config-file => configuration for connection to kubernetes cluster
- edit config:
    - copy original file for backup
    - easily overwrite file using aws-cli
        - install aws-cli using installer
        - click on account in in aws (top right)/my security credentials
        - access keys / create new access key
        - downlaod key file, keep it safe!
        - run cmdline: aws configure => enter credentials from key file
        - select same region as the cluster was created in
        - leave output format blank
        - wait until cluster is running
        - run cmdline: aws eks --region <region> update-kubeconfig --name <cluster-name>
- now kubectl connects to aws eks cluster

# deploy application to AWS EKS

kubectl apply -f=<filename.yaml>

kubectl get services =>
AWS gives use a use for th external IP of services

# adding volumes: 

- EmptyDir and HostPath are only useful in a test environment like Minikube 
    - EmptyDir will only be available in one single pod
    - HostPath will only be created on one single node
- CSI-type is useful in AWS:
    - there is a csi integration for AWS EFS
    - run installation command from aws efs csi driver github page
    - https://github.com/kubernetes-sigs/aws-efs-csi-driver 

- create Elastic File System in AWS Console:
    - EC2 Service Page => create security group for EFS
        - create new security group
        - name it, e.g. eks-efs
        - network: choose your eks vpc
        - inboudn rules / add rule
        - select NFS rule (port 2049)
        - Source: custom, then add the IPv4CIDR copied from your VPC settings
    - EFS Service Page
        - create filesystem
        - name it, e.g. eks-efs
        - network: choose your eks vpc
        - click customize, not create!
        - on 2nd page remove security groups and add your newly created security group for both availability zones
        - leave file system policy as is
        - click create
        - note down filesystem id

- create new yaml for persistant volume or add it inside existing yaml
    - kind: PersistantVolume
    - (for this add add an empty users folder to project!)
    - add metadata: and name: 
    - setting capacity in efs doesn't matter in EFS!
    - volumeMode: FileSystem
    - accessModes: 
            - ReadWriteMany
    - storageClassName: efs-sc
    - csi:
        driver: efs.csi.aws.com
        volumeHandle: <filesystem-id from efs>    
- add storageClass as another resource to the yaml file
    - kind: StorageClass
    - apiVersion: storage.k8s.io/v1
    - medata: 
        name: efy-sc
    - provisioner: efs.csi.aws.com
    - refer to https://github.com/kubernetes-sigs/aws-efs-csi-driver for and example
- add volumes-option in the deployment:
    - persistantVolumeClaim:
        - ClaimName: <pvc-name> (see below)
    -volumeMounts:
        name: <volume-name>
        mountPath: /app/users
- create persistant VOlume CLaims for the Persitant Volume above

=> verify this works by looking at EFS monitor in AWS Console


