

## Questions:

The Nautilus DevOps team is planning to deploy some micro services on Kubernetes platform. The team has already set up a Kubernetes cluster and now they want set up some namespaces, deployments etc. Based on the current requirements, the team has shared some details as below:

Create a namespace named dev and create a POD under it; name the pod dev-nginx-pod and use nginx image with latest tag only and remember to mention tag i.e nginx:latest.

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.

## Solution:  

**1. At first  kubectl  utility configure and working from jump server, run below commands**

```
thor@jump_host ~$ kubectl get namespace
NAME                 STATUS   AGE
default              Active   55m
kube-node-lease      Active   55m
kube-public          Active   55m
kube-system          Active   55m
local-path-storage   Active   55m
thor@jump_host ~$ kubectl get pods
No resources found in default namespace.
```

**2. Create a new namespace as per the task**  

```
thor@jump_host ~$ kubectl get pods
No resources found in default namespace.
thor@jump_host ~$ kubectl create namespace dev
namespace/dev created
thor@jump_host ~$ kubectl get namespace
NAME                 STATUS   AGE
default              Active   56m
dev                  Active   13s
kube-node-lease      Active   56m
kube-public          Active   56m
kube-system          Active   56m
local-path-storage   Active   56m
```

**3.  Run the below command to create a pod**

```
thor@jump_host ~$ kubectl run dev-nginx-pod --image=nginx:latest -n dev
pod/dev-nginx-pod created
```

**4.  Wait for  pods to get running status** 

```
thor@jump_host ~$ kubectl get pods -n dev
NAME            READY   STATUS    RESTARTS   AGE
dev-nginx-pod   1/1     Running   0          42s
```

**5.  Click on `Finish` & `Confirm` to complete the task successfully**