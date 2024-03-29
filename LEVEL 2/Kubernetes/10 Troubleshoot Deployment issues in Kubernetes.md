

## Questions:

Last week, the Nautilus DevOps team deployed a redis app on Kubernetes cluster, which was working fine so far. This morning one of the team members was making some changes in this existing setup, but he made some mistakes and the app went down. We need to fix this as soon as possible. Please take a look.

The deployment name is `redis-deployment`. The pods are not in running state right now, so please look into the issue and fix the same.

`Note`: The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.


## Solution:  

**1. Check existing running deployment, pods**

```

thor@jump_host ~$ kubectl get deployment
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
redis-deployment   0/1     1            0           100s
thor@jump_host ~$ kubectl get pods
NAME                                READY   STATUS              RESTARTS   AGE
redis-deployment-54cdf4f76d-dmst5   0/1     ContainerCreating   0          111s
thor@jump_host ~$ kubectl get configmap
NAME               DATA   AGE
kube-root-ca.crt   1      18m
redis-config       2      3m10s
```

**2. Describe the configuration of the deployment**

```

thor@jump_host ~$ kubectl describe deployment
Name:                   redis-deployment
Namespace:              default
CreationTimestamp:      Mon, 18 Sep 2023 10:18:17 +0000
Labels:                 app=redis
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               app=redis
Replicas:               1 desired | 1 updated | 1 total | 0 available | 1 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=redis
  Containers:
   redis-container:
    Image:      redis:alpin
    Port:       6379/TCP
    Host Port:  0/TCP
    Requests:
      cpu:        300m
    Environment:  <none>
    Mounts:
      /redis-master from config (rw)
      /redis-master-data from data (rw)
  Volumes:
   data:
    Type:       EmptyDir (a temporary directory that shares a pod's lifetime)
    Medium:     
    SizeLimit:  <unset>
   config:
    Type:      ConfigMap (a volume populated by a ConfigMap)
    Name:      redis-conig
    Optional:  false
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      False   MinimumReplicasUnavailable
  Progressing    True    ReplicaSetUpdated
OldReplicaSets:  <none>
NewReplicaSet:   redis-deployment-54cdf4f76d (1/1 replicas created)
Events:
  Type    Reason             Age    From                   Message
  ----    ------             ----   ----                   -------
  Normal  ScalingReplicaSet  3m48s  deployment-controller  Scaled up replica set redis-deployment-54cdf4f76d to 1
```

```

thor@jump_host ~$ kubectl describe configmap
Name:         kube-root-ca.crt
Namespace:    default
Labels:       <none>
Annotations:  kubernetes.io/description:
                Contains a CA bundle that can be used to verify the kube-apiserver when using internal endpoints such as the internal service IP or kubern...

Data
====
ca.crt:
----
-----BEGIN CERTIFICATE-----
MIIDBTCCAe2gAwIBAgIIGAbwSS1tIBkwDQYJKoZIhvcNAQELBQAwFTETMBEGA1UE
AxMKa3ViZXJuZXRlczAeFw0yMzA5MTgxMDAxNTVaFw0zMzA5MTUxMDAxNTVaMBUx
EzARBgNVBAMTCmt1YmVybmV0ZXMwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEK
AoIBAQC8hcnzYvlGAExhkBlfZwjcvuiBNVCJeGNNwOFaxUXvntiq1eQI2gIRYRCs
ohPyh9JTyZCMRP5QqpdaQBu84F7TMokFpsG0huD3y87bjmwHYLQzz6MJhh2vm+ik
xMnhhqGLUumSN7SJ7mOcM5J4fKIES1SGTytkecl7sY6igbNpMClln1mwDBgEGN/A
EWf7Z5tscjQGnCOoe7cORswABMrHIXhpcQ6KYSWjdig4f5ltTjLWa9Aw6Ib190qg
NfYVujzNpCvtNc+duTJACXRU7NZ3OOCrdTIyCu7MocpxCG3yarJCq0L5uG+AFZCj
RNCy5n4A6Y9z6h9HRAiuW3puqNZpAgMBAAGjWTBXMA4GA1UdDwEB/wQEAwICpDAP
BgNVHRMBAf8EBTADAQH/MB0GA1UdDgQWBBTg0y9HSV8zljQqWAG1UUnnBg92aTAV
BgNVHREEDjAMggprdWJlcm5ldGVzMA0GCSqGSIb3DQEBCwUAA4IBAQCNc+K3dcgz
/rwesBYdIKUgnPcTKUnz1rIzVYlWKvjsdBy9SQbsC4Z2U1ZIRTqvdh0FNTHxSJ1T
xSJT0qXO6++gCfD/3QAk9HxMjR+RSwChN1SHo6xxS4P3vJVFE8YX9FoemLYYrWEP
RmQn/eE6wRJoCCqQpRX7NL8U+FkE2zI6QrOzanFOlm01Z2IHoZENiDRZbOpIeH3I
Oqhmt71+qZG23EJJC7uuo0fUi8QsjxPYcib5yCAvRPi95JO2x6oOMZ1qOcEEjQvw
UKoqZUAQ7bFjuCMIZilJogJACW2lTLpHgulyocluMiN8om3eKKziJ5CBhuTtvXQx
cH+4cCZipIqb
-----END CERTIFICATE-----


BinaryData
====

Events:  <none>


Name:         redis-config
Namespace:    default
Labels:       <none>
Annotations:  <none>

Data
====
maxmemory:
----
2mb
maxmemory-policy:
----
allkeys-lru

BinaryData
====

Events:  <none>
```

```

thor@jump_host ~$ kubectl describe pods
Name:             redis-deployment-54cdf4f76d-dmst5
Namespace:        default
Priority:         0
Service Account:  default
Node:             kodekloud-control-plane/172.17.0.2
Start Time:       Mon, 18 Sep 2023 10:18:17 +0000
Labels:           app=redis
                  pod-template-hash=54cdf4f76d
Annotations:      <none>
Status:           Pending
IP:               
IPs:              <none>
Controlled By:    ReplicaSet/redis-deployment-54cdf4f76d
Containers:
  redis-container:
    Container ID:   
    Image:          redis:alpin
    Image ID:       
    Port:           6379/TCP
    Host Port:      0/TCP
    State:          Waiting
      Reason:       ContainerCreating
    Ready:          False
    Restart Count:  0
    Requests:
      cpu:        300m
    Environment:  <none>
    Mounts:
      /redis-master from config (rw)
      /redis-master-data from data (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-hf5ch (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             False 
  ContainersReady   False 
  PodScheduled      True 
Volumes:
  data:
    Type:       EmptyDir (a temporary directory that shares a pod's lifetime)
    Medium:     
    SizeLimit:  <unset>
  config:
    Type:      ConfigMap (a volume populated by a ConfigMap)
    Name:      redis-conig
    Optional:  false
  kube-api-access-hf5ch:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   Burstable
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type     Reason       Age                   From               Message
  ----     ------       ----                  ----               -------
  Normal   Scheduled    5m44s                 default-scheduler  Successfully assigned default/redis-deployment-54cdf4f76d-dmst5 to kodekloud-control-plane
  Warning  FailedMount  94s (x10 over 5m43s)  kubelet            MountVolume.SetUp failed for volume "config" : configmap "redis-conig" not found
  Warning  FailedMount  84s (x2 over 3m41s)   kubelet            Unable to attach or mount volumes: unmounted volumes=[config], unattached volumes=[], failed to process volumes=[]: timed out waiting for the condition
```

**3. we have troubleshoot the issues with deployment lets fixed by editing**

```
thor@jump_host ~$ kubectl edit deployment
deployment.apps/redis-deployment edited

search word -> 
/conig - change as config
/conig - change as config

again search word ->
/alpin - change as alpine
/alpin - change as alpine
```

**4. Wait for the pod to get completed status**

```

thor@jump_host ~$ kubectl get deploy
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
redis-deployment   1/1     1            1           20m

thor@jump_host ~$ kubectl get pods
NAME                                READY   STATUS    RESTARTS   AGE
redis-deployment-7c8d4f6ddf-mldq8   1/1     Running   0          2m28s
```

**5. Click on `Finish` & `Confirm` to complete the task successful**
