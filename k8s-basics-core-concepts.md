## K8S - Basics 

### Introduction. 

[Book Mark this page] (https://kubernetes.io/docs/reference/kubectl/conventions/)


```



Create an NGINX Pod

kubectl run nginx --image=nginx


Generate POD Manifest YAML file (-o yaml). Don't create it(--dry-run)

kubectl run nginx --image=nginx --dry-run=client -o yaml


Create a deployment

kubectl create deployment --image=nginx nginx


Generate Deployment YAML file (-o yaml). Don't create it(--dry-run)

kubectl create deployment --image=nginx nginx --dry-run=client -o yaml


Generate Deployment YAML file (-o yaml). Don't create it(--dry-run) with 4 Replicas (--replicas=4)

kubectl create deployment --image=nginx nginx --dry-run=client -o yaml > nginx-deployment.yaml

Save it to a file, make necessary changes to the file (for example, adding more replicas) and then create the deployment.
```


## Namespaces

***Handy commands***

```
kubectl get pods --namespace=dev
```

The below commands will set the namespace at the context level, (Try it out- to be confirmed)

```
kubectl config set-context $(kubectl config current context) --namespace=dev
```

### Resource quota 
Links?

examples?

```
apiVersion:
kind: ResourceQuota
metadata:
  name: comp-quota
  namespace: dev
spec:
  hard:
    pod: "10"
    requests.cpu: "4"
    requests.memory: "5Gi"
    limits.cpu: "10"
    limits.memory: "10Gi"
```

### Imperative / Declarative

#### Imperative:
Tell what is needed and How it should be done?. 
```
kubectl run --image=nginx nginx
kubectl create deployment --image=nginx nginx
kubectl expose deployment nginx --port 80
kubectl edit deployment
kubectl scale deployment nginx --replicas=5
kubectl set image deployment nginx nginx=nginx:1.18
kubectl create -f nginx.yaml
kubectl replace -f nginx.yaml
kubectl delete -f nginx.yaml
```

Useful for exam
Making changes in the live objects.

Update objects
```
kubectl eedit deployment nginx
kubectl replace -f nginx.yaml
kubectl replace --force -f nginx.yaml
kubectl create -f nginx.yaml
```
When you want to deal with imperative way, you need to know the current configuration to resolve it.

#### Declarative:
Just Declare..! and executed by the system.
System is intelligent enough to carryout the instructions.
```
kubectl apply -f nginx.yaml
```
apply is intelligent enough to make the necessary changes. 

Tip: Use imperative commands 

Refer (Add the link here): Kubernetes document. Manage kubernetes objects. 

#### Exam Tip:

--dry-run: By default as soon as the command is run, the resource will be created. If you simply want to test your command, use the --dry-run=client option. This will not create the resource, instead, tell you whether the resource can be created and if your command is right.

-o yaml: This will output the resource definition in YAML format on the screen.

##### POD

Create an NGINX Pod
```
kubectl run nginx --image=nginx
```

Generate POD Manifest YAML file (-o yaml). Don't create it(--dry-run)
```
kubectl run nginx --image=nginx  --dry-run=client -o yaml
```

##### Deployment

Create a deployment
```
kubectl create deployment --image=nginx nginx
```

Generate Deployment YAML file (-o yaml). Don't create it(--dry-run)
```
kubectl create deployment --image=nginx nginx --dry-run=client -o yaml
```

IMPORTANT:

kubectl create deployment does not have a --replicas option. You could first create it and then scale it using the kubectl scale command.


Save it to a file - (If you need to modify or add some other details)

kubectl create deployment --image=nginx nginx --dry-run=client -o yaml > nginx-deployment.yaml

You can then update the YAML file with the replicas or any other field before creating the deployment.


##### Service

Create a Service named redis-service of type ClusterIP to expose pod redis on port 6379
```
kubectl expose pod redis --port=6379 --name redis-service --dry-run=client -o yaml

(This will automatically use the pod's labels as selectors)

Or
kubectl create service clusterip redis --tcp=6379:6379 --dry-run=client -o yaml 
```
 (This will not use the pods labels as selectors, instead it will assume selectors as app=redis. You cannot pass in selectors as an option. So it does not work very well if your pod has a different label set. So generate the file and modify the selectors before creating the service)


Create a Service named nginx of type NodePort to expose pod nginx's port 80 on port 30080 on the nodes:

```
kubectl expose pod nginx --port=80 --name nginx-service --type=NodePort --dry-run=client -o yaml

(This will automatically use the pod's labels as selectors, but you cannot specify the node port. You have to generate a definition file and then add the node port in manually before creating the service with the pod.)

Or

kubectl create service nodeport nginx --tcp=80:80 --node-port=30080 --dry-run=client -o yaml
```
(This will not use the pods labels as selectors)

Both the above commands have their own challenges. While one of it cannot accept a selector the other cannot accept a node port. I would recommend going with the `kubectl expose` command. If you need to specify a node port, generate a definition file using the same command and manually input the nodeport before creating the service.


[Reference:] (https://kubernetes.io/docs/reference/kubectl/conventions/)