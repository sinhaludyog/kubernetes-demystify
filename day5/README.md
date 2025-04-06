# README.md

## Before proceeding, ensure you have the metrics server installed. If not then apply metrics-server file
```bash
kubectl apply -f 1-service-account.yaml
kubectl apply -f 2-cluster-role.yaml
kubectl apply -f 3-role-binding.yaml
kubectl apply -f 4-cluster-role-binding.yaml
kubectl apply -f 5-service.yaml
kubectl apply -f 6-deployment.yaml
kubectl apply -f 7-api-service.yaml
```

## Now you can check the pod metrics, ServiceAccount, ClusterRole etc
```bash
kubectl top pods -n kube-system
kubectl get ServiceAccount -n kube-system | grep metrics
kubectl get ClusterRole  | grep system:aggregated-metrics-reader
kubectl get ClusterRole  | grep system:metrics-server
kubectl get rolebinding -n kube-system | grep metrics-server-auth-reader
kubectl get ClusterRoleBinding | grep metrics-server:system:auth-delegator
kubectl get ClusterRoleBinding | grep system:metrics-server
kubectl get sa -n kube-system | grep metrics-server
kubectl get APIService  | grep v1beta1.metrics.k8s.io
```

## Now Apply the Sample Application for HPA demo
```bash
kubectl apply -f 1-Deployment.yaml
kubectl apply -f 2-Server.yaml
kubectl apply -f 3-HPA.yaml
```

## We will start a container and send an infinite loop of queries to the service to increase the load on it, listening on port 8080. Open a new terminal and execute the below command:
```bash
kubectl run -i --tty load-generator --rm --image=busybox --restart=Never -- /bin/sh -c "while sleep 0.01; do wget -q -O- hpa-demo-service:8080; done"
```


## How to define Kubernetes node affinity?
Node affinity depends on the labels specified on the node. To begin, a label to the nodes using the kubectl label nodes command shown below:
```bash
kubectl label nodes colima name=node-affinity-test 
node/colima labeled
```


## You can see the labels configured on the specific node using the kubectl describe node command:
```bash
kubectl describe node colima | grep Labels: -A10
Labels:             beta.kubernetes.io/arch=amd64
                    beta.kubernetes.io/instance-type=k3s
                    beta.kubernetes.io/os=linux
                    kubernetes.io/arch=amd64
                    kubernetes.io/hostname=colima
                    kubernetes.io/os=linux
                    name=node-affinity-test
                    node-role.kubernetes.io/control-plane=true
                    node-role.kubernetes.io/master=true
                    node.kubernetes.io/instance-type=k3s
Annotations:        alpha.kubernetes.io/provided-node-ip: 192.168.5.1
```

## The kubectl taint command with the required taint allows us to add taints to nodes. The general syntax for the command is:
```bash
kubectl taint nodes <node name> <taint key>=<taint value>:<taint effect>
kubectl taint nodes colima testpod=true:NoSchedule

kubectl describe node colima | grep Taints:
Taints:             testpod=true:NoSchedule
```

## We can remove the taint by specifying the taint key and the taint effect with a minus(-) to signify the removal. The basic syntax of the command is:
```bash
kubectl taint nodes <node name> <taint key>:<taint effect>-
Now, let’s remove the previously added taint to the colima node.

kubectl taint nodes colima testpod=true:NoSchedule-
node/colima untainted                
```


## Network topology
```bash
kubectl label nodes colima zone=zonea 
kubectl label nodes colima zone-
```

## CRD
```bash
kubectl get api-resources
Steps to See API Server Groups 
kubectl proxy
open curl localhost:8001 open curl localhost:8001 | grep -i kimbal to check API group
```