
## Questions:

The Nautilus DevOps team want to create a time check pod in a particular Kubernetes namespace and record the logs. This might be initially used only for testing purposes, but later can be implemented in an existing cluster. Please find more details below about the task and perform it.



Create a pod called time-check in the devops namespace. This pod should run a container called time-check, container should use the busybox image with latest tag only and remember to mention tag i.e busybox:latest.

Create a config map called time-config with the data TIME_FREQ=12 in the same namespace.

The time-check container should run the command: while true; do date; sleep $TIME_FREQ;done and should write the result to the location /opt/itadmin/time/time-check.log. Remember you will also need to add an environmental variable TIME_FREQ in the container, which should pick value from the config map TIME_FREQ key.

Create a volume log-volume and mount the same on /opt/itadmin/time within the container.

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.


## Solution:  

**1. At first  kubectl  utility configure and working from jump server, run below commands**

```
thor@jump_host ~$ kubectl get namespace
NAME                 STATUS   AGE
default              Active   31m
kube-node-lease      Active   31m
kube-public          Active   31m
kube-system          Active   31m
local-path-storage   Active   31mce

thor@jump_host ~$ kubectl get pods
No resources found in default namespace.
```

**2. create a new namespace  as per the given in the task** 
  
```  
thor@jump_host ~$ kubectl create namespace devops
namespace/devops created
```

**3.  Create a YAML  file with all the parameters, you can copy from GitLab**

```
thor@jump_host ~$ vi /tmp/time.yaml

open terminal paste:
apiVersion: v1

kind: ConfigMap

metadata:

  name: time-config

  namespace: devops

data:

  TIME_FREQ: "12"

---

apiVersion: v1

kind: Pod

metadata:

  name: time-check

  namespace: devops

  labels:

    app: time-check

spec:

  volumes:

    - name: log-volume

      emptyDir: {}

  containers:

    - name: time-check

      image: busybox:latest

      volumeMounts:

        - mountPath: /opt/itadmin/time

          name: log-volume

      envFrom:

        - configMapRef:

            name: time-config

      command: ["/bin/sh", "-c"]

      args:

        [

          "while true; do date; sleep $TIME_FREQ;done > /opt/itadmin/time/time-check.log",

        ]

exit :wq

thor@jump_host /$ cat /tmp/time.yaml
```

**4.  Run the below command to create a pod**

```
thor@jump_host ~$ kubectl create -f /tmp/time.yaml
configmap/time-config created
pod/time-check created
```

**5.  Wait for  pods to get running status**
      
```
thor@jump_host ~$ kubectl get pods -n devops
NAME         READY   STATUS    RESTARTS   AGE
time-check   1/1     Running   0          36s
```

**6.  Click on `Finish` & `Confirm` to complete the task successfully**