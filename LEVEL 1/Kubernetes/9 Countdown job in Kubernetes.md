

## Questions:

The Nautilus DevOps team is working on to create few jobs in Kubernetes cluster. They might come up with some real scripts/commands to use, but for now they are preparing the templates and testing the jobs with dummy commands. Please create a job template as per details given below:


1.Create a job `countdown-devops`.

2.The spec template should be named as `countdown-devops` (under metadata), and the container should be named as `container-countdown-devops)`

3.Use image `debian` with `latest` tag only and remember to mention tag i.e `debian:latest`, and restart policy should be `Never`.

4. Use command `sleep 5`

Note: The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.


## Solution:

**1. At first  kubectl  utility configure and working from jump server, run below commands**

```
thor@jump_host ~$ kubectl get services
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   29m

thor@jump_host ~$ kubectl get pods
No resources found in default namespace.
```

**2.  Create yaml  file with all the parameters , you can copy form gitlab**
    
```
thor@jump_host ~$ vi /tmp/countdown.yaml
thor@jump_host ~$ cat /tmp/countdown.yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: countdown-devops
spec:
  template:
    metadata:
      name: countdown-devops
    spec:
      containers:
        - name: container-countdown-devops
          image: debian:latest
          command: ["/bin/sh", "-c", "sleep 5" ]
          args:
      restartPolicy: Never
```

**3.  Run below command to create pod**

```
thor@jump_host ~$ kubectl create -f /tmp/countdown.yaml
job.batch/countdown-devops created
```

**4.  Wait for  pods to get running status**

```
thor@jump_host ~$ kubectl get pods
NAME                     READY   STATUS      RESTARTS   AGE
countdown-devops-jbpvl   0/1     Completed   0          50s
```

**5.  Click on `Finish` & `Confirm` to complete the task successful**