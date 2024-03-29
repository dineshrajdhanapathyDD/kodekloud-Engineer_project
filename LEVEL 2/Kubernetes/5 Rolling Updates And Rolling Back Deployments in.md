

## Questions:

There is a production deployment planned for next week. The Nautilus DevOps team wants to test the deployment update and rollback on Dev environment first so that they can identify the risks in advance. Below you can find more details about the plan they want to execute.



1. Create a namespace `devops`. Create a deployment called `httpd-deploy` under this new namespace, It should have one container called `httpd`, use `httpd:2.4.27` image and `3` replicas. The deployment should use RollingUpdate strategy with `maxSurge=1`, and `maxUnavailable=2`. Also create a `NodePort` type service named `httpd-service` and expose the deployment on `nodePort: 30008`.


2. Now upgrade the deployment to version `httpd:2.4.43` using a rolling update.


Finally, once all pods are updated undo the recent update and roll back to the previous/original version.


`Note`:

a. The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.


b. Please make sure you only use the specified image(s) for this deployment and as per the sequence mentioned in the task description. If you mistakenly use a wrong image and fix it later, that will also distort the revision history which can eventually fail this task.


## Solution:  

**1. At first  kubectl  utility configure and working from jump server, rub below commands**

```

thor@jump_host ~$ kubectl get namespace
NAME                 STATUS   AGE
default              Active   12m
kube-node-lease      Active   12m
kube-public          Active   12m
kube-system          Active   12m
local-path-storage   Active   11m
```

**2.  Create namespace as per the task request.**

```

thor@jump_host ~$ kubectl create namespace  devops
namespace/devops created
```

**3.  Create yaml  file with all the parameters**

```

thor@jump_host ~$ vi /tmp/httpd.yaml
thor@jump_host ~$ cat /tmp/httpd.yaml
apiVersion: v1
kind: Namespace
metadata:
  name: devops
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: httpd-deploy
  namespace: devops
spec:
  strategy:
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 2
  replicas: 3
  selector:
    matchLabels:
      app: httpd
  template:
    metadata:
      name: httpd
      labels:
        app: httpd
    spec:
      containers:
        - name: httpd
          image: httpd:2.4.27
          imagePullPolicy: IfNotPresent
      restartPolicy: Always
---
apiVersion: v1
kind: Service
metadata:
  name: httpd-service
  namespace: devops
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
      nodePort: 30008
  type: NodePort
```

**4.  Run below command to create pod**

```

thor@jump_host ~$ kubectl apply  -f /tmp/httpd.yaml --namespace=devops
Warning: resource namespaces/devops is missing the kubectl.kubernetes.io/last-applied-configuration annotation which is required by kubectl apply. kubectl apply should only be used on resources created declaratively by either kubectl create --save-config or kubectl apply. The missing annotation will be patched automatically.
namespace/devops configured
deployment.apps/httpd-deploy created
service/httpd-service created
```

**5.  Wait for  pods to get running status**

```

thor@jump_host ~$ kubectl get pods -n devops
NAME                           READY   STATUS    RESTARTS   AGE
httpd-deploy-69cd9ccdd-8wfjk   1/1     Running   0          76s
httpd-deploy-69cd9ccdd-gwjbg   1/1     Running   0          76s
httpd-deploy-69cd9ccdd-qlwfd   1/1     Running   0          76s


thor@jump_host ~$ kubectl get deployment -n devops
NAME           READY   UP-TO-DATE   AVAILABLE   AGE
httpd-deploy   3/3     3            3           103s
```

**6.  Perform  rolling update by running below command**

```

thor@jump_host ~$ kubectl set image deploy/httpd-deploy httpd=httpd:2.4.43 -n devops
deployment.apps/httpd-deploy image updated

thor@jump_host ~$ kubectl rollout history deploy/httpd-deploy -n devops
deployment.apps/httpd-deploy 
REVISION  CHANGE-CAUSE
1         <none>
2         <none>
```

**7.  Rollback the deployment as per task**

```

thor@jump_host ~$ kubectl rollout undo deploy/httpd-deploy -n devops
deployment.apps/httpd-deploy rolled back
```

**8.  validate roll out status by command**

```

thor@jump_host ~$ kubectl rollout status deploy/httpd-deploy -n devops
deployment "httpd-deploy" successfully rolled out
```

**9.  Click on `Finish` & `Confirm` to complete the task successful**