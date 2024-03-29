

## Questions:

There is an application deployed on Kubernetes cluster. Recently, the Nautilus application development team developed a new version of the application that needs to be deployed now. As per new updates some new changes need to be made in this existing setup. So update the deployment and service as per details mentioned below:


We already have a deployment named `nginx-deployment` and service named `nginx-service`. Some changes need to be made in this deployment and service, make sure not to delete the deployment and service.

1. Change the service nodeport from `30008` to `32165`

2. Change the replicas count from `1` to `5`

3. Change the image from `nginx:1.18` to `nginx:latest`

Note: The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.


## Solution: 

**1. At first  run below commands to check  the existing setup**    

```
thor@jump_host ~$ kubectl get deploy
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   1/1     1            1           2m22s

thor@jump_host ~$ kubectl get service
NAME            TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
kubernetes      ClusterIP   10.96.0.1      <none>        443/TCP        4m35s
nginx-service   NodePort    10.96.230.21   <none>        80:30008/TCP   2m45s

thor@jump_host ~$ kubectl get pods
NAME                                READY   STATUS    RESTARTS   AGE
nginx-deployment-58cf54c7f6-rj8km   1/1     Running   0          3m12s
```

**2. Edit the services as per the task and change the port no**

```
thor@jump_host ~$ kubectl edit service nginx-service
service/nginx-service edited

(Change the service nodeport from 30008 to 32165)

# Please edit the object below. Lines beginning with a '#' will be ignored,
# and an empty file will abort the edit. If an error occurs while saving this file will be
# reopened with the relevant failures.
#
apiVersion: v1
kind: Service
metadata:
  annotations:
    kubectl.kubernetes.io/last-applied-configuration: |
      {"apiVersion":"v1","kind":"Service","metadata":{"annotations":{},"name":"nginx-service","namespace":"default"},"spec":{"ports":[{"nodePort":30008,"port":80,"targetPort":80}],"selector":{"app":"nginx-app"},"type":"NodePort"}}
  creationTimestamp: "2023-08-22T12:08:24Z"
  name: nginx-service
  namespace: default
  resourceVersion: "604"
  uid: 27c6de91-4e31-4ec4-8ae4-84e877e6391d
spec:
  clusterIP: 10.96.230.21
  clusterIPs:
  - 10.96.230.21
  externalTrafficPolicy: Cluster
  internalTrafficPolicy: Cluster
  ipFamilies:
  - IPv4
  ipFamilyPolicy: SingleStack
  ports:
  - nodePort: 30008 -> nodePort: 32165
    port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: nginx-app
  sessionAffinity: None
  type: NodePort
status:
  loadBalancer: {}

 

thor@jump_host ~$ kubectl get service
NAME            TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
kubernetes      ClusterIP   10.96.0.1      <none>        443/TCP        11m
nginx-service   NodePort    10.96.230.21   <none>        80:32165/TCP   9m18s
```

**3. Edit the deployment and do the changes as per the task**

```
thor@jump_host ~$ kubectl edit deploy nginx-deployment
deployment.apps/nginx-deployment edited

(Change the replicas count from 1 to 5
Change the image from nginx:1.18 to nginx:latest)

# Please edit the object below. Lines beginning with a '#' will be ignored,
# and an empty file will abort the edit. If an error occurs while saving this file will be
# reopened with the relevant failures.
#
apiVersion: apps/v1
kind: Deployment
metadata:
  annotations:
    deployment.kubernetes.io/revision: "1"
    kubectl.kubernetes.io/last-applied-configuration: |
      {"apiVersion":"apps/v1","kind":"Deployment","metadata":{"annotations":{},"labels":{"app":"nginx-app","type":"front-end"},"name":"nginx-deployment","namespace":"default"},"spec":{"replicas":1,"selector":{"matchLabels":{"app":"nginx-app"}},"template":{"metadata":{"labels":{"app":"nginx-app"},"name":"nginx-replica"},"spec":{"containers":[{"image":"nginx:1.18","name":"nginx-container"}]}}}}
  creationTimestamp: "2023-08-22T12:08:24Z"
  generation: 1
  labels:
    app: nginx-app
    type: front-end
  name: nginx-deployment
  namespace: default
  resourceVersion: "639"
  uid: bcc43c89-3bbb-48d5-86bd-cfd99198703a
spec:
  progressDeadlineSeconds: 600
  replicas: 1 -> replicas: 5
  revisionHistoryLimit: 10
  selector:
    matchLabels:
      app: nginx-app
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: nginx-app
      name: nginx-replica
    spec:
      containers:
      - image: nginx:1.18 -> image: nginx:latest
        imagePullPolicy: IfNotPresent
        name: nginx-container
        resources: {}
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
      dnsPolicy: ClusterFirst
      restartPolicy: Always
      schedulerName: default-scheduler
      securityContext: {}
      terminationGracePeriodSeconds: 30
status:
  availableReplicas: 1
  conditions:
  - lastTransitionTime: "2023-08-22T12:08:34Z"
    lastUpdateTime: "2023-08-22T12:08:34Z"
    message: Deployment has minimum availability.
    reason: MinimumReplicasAvailable
    status: "True"
    type: Available
  - lastTransitionTime: "2023-08-22T12:08:24Z"
    lastUpdateTime: "2023-08-22T12:08:34Z"
    message: ReplicaSet "nginx-deployment-58cf54c7f6" has successfully progressed.
    reason: NewReplicaSetAvailable
    status: "True"
    type: Progressing
  observedGeneration: 1
  readyReplicas: 1
  replicas: 1
  updatedReplicas: 1

thor@jump_host ~$ kubectl get deploy
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   4/5     4            4           14m

thor@jump_host ~$ kubectl get pods
NAME                                READY   STATUS    RESTARTS   AGE
nginx-deployment-854ff588b7-97ls7   1/1     Running   0          27s
nginx-deployment-854ff588b7-c7p98   1/1     Running   0          64s
nginx-deployment-854ff588b7-mmtvb   1/1     Running   0          30s
nginx-deployment-854ff588b7-wd8vv   1/1     Running   0          64s
nginx-deployment-854ff588b7-xrdlm   1/1     Running   0          64s
```

**4.  Wait for  deployment & pods to get ready & running status**

```
thor@jump_host ~$ kubectl get deploy
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   5/5     5            5           15m

thor@jump_host ~$ kubectl get pods
NAME                                READY   STATUS    RESTARTS   AGE
nginx-deployment-854ff588b7-97ls7   1/1     Running   0          96s
nginx-deployment-854ff588b7-c7p98   1/1     Running   0          2m13s
nginx-deployment-854ff588b7-mmtvb   1/1     Running   0          99s
nginx-deployment-854ff588b7-wd8vv   1/1     Running   0          2m13s
nginx-deployment-854ff588b7-xrdlm   1/1     Running   0          2m13s
```

**5.  Click on `Finish` & `Confirm` to complete the task successful**

