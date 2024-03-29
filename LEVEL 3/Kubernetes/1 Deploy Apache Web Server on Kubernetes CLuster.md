

## Questions:
There is an application that needs to be deployed on Kubernetes cluster under Apache web server. The Nautilus application development team has asked the DevOps team to deploy it. We need to develop a template as per requirements mentioned below:



Create a namespace named as httpd-namespace-devops.

Create a deployment named as httpd-deployment-devops under newly created namespace. For the deployment use httpd image with latest tag only and remember to mention the tag i.e httpd:latest, and make sure replica counts are 2.

Create a service named as httpd-service-devops under same namespace to expose the deployment, nodePort should be 30004.

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.


## Solution:

**1. At first  create a namespace as per the task**

```

thor@jump_host ~$ kubectl create namespace httpd-namespace-devops
namespace/httpd-namespace-devops created
thor@jump_host ~$ kubectl get namespace
NAME                     STATUS   AGE
default                  Active   50m
httpd-namespace-devops   Active   18s
kube-node-lease          Active   50m
kube-public              Active   50m
kube-system              Active   50m
local-path-storage       Active   50m
```

**2. Create yaml  file with all the parameters , you can copy form gitlab**

```

thor@jump_host ~$ vi /tmp/http.yaml

open new terminal:

apiVersion: v1
kind: Service
metadata:
  name: httpd-service-devops
  namespace: httpd-namespace-devops
spec:
  type: NodePort
  selector:
    app: httpd_app_devops
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30004
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: httpd-deployment-devops
  namespace: httpd-namespace-devops
  labels:
    app: httpd_app_devops
spec:
  replicas: 2
  selector:
    matchLabels:
      app: httpd_app_devops
  template:
    metadata:
      labels:
        app: httpd_app_devops
    spec:
      containers:
        - name: httpd-container-devops
          image: httpd:latest
          ports:
            - containerPort: 80

control+c , exit :wq

thor@jump_host ~$ cat /tmp/http.yaml
```

**3.  Run below command to create pod**

```

thor@jump_host ~$ kubectl create -f /tmp/http.yaml
service/httpd-service-devops created
deployment.apps/httpd-deployment-devops created
```

**4.  Wait for  pods to get running status**

```

thor@jump_host ~$ kubectl get pods -n httpd-namespace-devops
NAME                                      READY   STATUS    RESTARTS   AGE
httpd-deployment-devops-867b499f4-8jjrb   1/1     Running   0          98s
httpd-deployment-devops-867b499f4-pbwm8   1/1     Running   0          98s
```

**5.  Click on `Finish` & `Confirm` to complete the task successful.**

