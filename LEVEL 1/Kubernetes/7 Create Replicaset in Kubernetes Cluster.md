
## Questions:

The Nautilus DevOps team is going to deploy some applications on kubernetes cluster as they are planning to migrate some of their existing applications there. Recently one of the team members has been assigned a task to write a template as per details mentioned below:

1. Create a ReplicaSet using `nginx` image with `latest` tag only and remember to mention tag i.e `nginx:latest` and name it as `nginx-replicaset`.

2. Labels `app` should be `nginx_app`, labels `type` should be `front-end`.

3. The container should be named as `nginx-container`; also make sure replicas counts are 4.


Note: The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.


## Solution: 

**1. At first  kubectl  utility configure and working from jump server, run below commands** 

```
thor@jump_host ~$ kubectl get deploy
No resources found in default namespace.

thor@jump_host ~$ kubectl get services
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   8m28s
```

**2.  Create  httpd.yaml  file with all the parameters given in task**

```
thor@jump_host ~$ vi /tmp/nginx.yaml
thor@jump_host ~$ cat /tmp/nginx.yaml
apiVersion: apps/v1

kind: ReplicaSet

metadata:

  name: nginx-replicaset

  labels:
 
    app: nginx_app

    type: front-end

spec:

  replicas: 4

  selector:

    matchLabels:

      app: nginx_app

  template:

    metadata:

      labels:

        app: nginx_app

        type: front-end

    spec:

      containers:

        - name: nginx-container

          image: nginx:latest
```

**3.  Run below command to create pod**

```
thor@jump_host ~$ kubectl create -f /tmp/nginx.yaml
replicaset.apps/nginx-replicaset created
```

**4.  Wait for  pods to get running status**

```
thor@jump_host ~$ kubectl get pods
NAME                     READY   STATUS    RESTARTS   AGE
nginx-replicaset-5nrhg   1/1     Running   0          37s
nginx-replicaset-7vtn9   1/1     Running   0          37s
nginx-replicaset-9mmvs   1/1     Running   0          37s
nginx-replicaset-rmn8n   1/1     Running   0          37s
```

**5.  Click on `Finish` & `Confirm` to complete the task successful**






