

## Questions:
The Nautilus DevOps team has started practicing some pods and services deployment on Kubernetes platform as they are planning to migrate most of their applications on Kubernetes platform. Recently one of the team members has been assigned a task to create a pod as per details mentioned below:


1. Create a pod named `pod-nginx` using `nginx` image with `latest` tag only and remember to mention the tag i.e `nginx:latest`.

2. Labels `app` should be set to `nginx_app`, also container should be named as `nginx-container`.

Note: The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.


## Solution:  

**1. At first  kubectl  utility configure and working from jump server, run below commands**  

```
thor@jump_host ~$ kubectl get namespace
NAME                 STATUS   AGE
default              Active   20m
kube-node-lease      Active   20m
kube-public          Active   20m
kube-system          Active   20m
local-path-storage   Active   19m

thor@jump_host ~$ kubectl get pods
No resources found in default namespace.
```

**2.  Create yaml  file with all the parameters , you can copy form gitlab**

    
```
thor@jump_host ~$ vi /tmp/pod.yaml
thor@jump_host ~$ cat /tmp/pod.yaml
apiVersion: v1

kind: Pod

metadata:

    name: pod-nginx

    labels:

      app: nginx_app

spec:

    containers:

    - name: nginx-container

      image: nginx:latest
```

**3.  Run below command to create pod**   

```
thor@jump_host ~$ kubectl create -f /tmp/pod.yaml
pod/pod-nginx created
```

**4.  Wait for  pods to get running status**

```
thor@jump_host ~$ kubectl get pods
NAME        READY   STATUS    RESTARTS   AGE
pod-nginx   1/1     Running   0          37s

thor@jump_host ~$ kubectl get pods -o wide
NAME        READY   STATUS    RESTARTS   AGE   IP           NODE                      NOMINATED NODE   READINESS GATES
pod-nginx   1/1     Running   0          59s   10.244.0.5   kodekloud-control-plane   <none>           <none>
```

**5.  Click on `Finish` & `Confirm` to complete the task successful**
