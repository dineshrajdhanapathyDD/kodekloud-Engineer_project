

## Questions:

One of the junior DevOps team members was working on to deploy a stack on Kubernetes cluster. Somehow the pod is not coming up and its failing with some errors. We need to fix this as soon as possible. Please look into it.


There is a pod named `webserver` and the container under it is named as `nginx-container`. It is using image `nginx:latest`

There is a sidecar container as well named `sidecar-container` which is using `ubuntu:latest` image.

Look into the issue and fix it, make sure pod is in `running` state and you are able to access the app.

Note: The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.


## Solution: 

**1. At first  kubectl  utility configure and working from jump server, run below commands**

```
thor@jump_host ~$ kubectl get pods
NAME        READY   STATUS             RESTARTS   AGE
webserver   1/2     ImagePullBackOff   0          3m2s
```

**2. To identify the error run below command** 

```
thor@jump_host ~$ kubectl describe pod webserver
Name:             webserver
Namespace:        default
Priority:         0
Service Account:  default
Node:             kodekloud-control-plane/172.17.0.2
Start Time:       Mon, 21 Aug 2023 10:04:37 +0000
Labels:           app=web-app
Annotations:      <none>
Status:           Pending
IP:               10.244.0.5
IPs:
  IP:  10.244.0.5
Containers:
  nginx-container:
    Container ID:   
    Image:          nginx:latests
    Image ID:       
    Port:           <none>
    Host Port:      <none>
    State:          Waiting
      Reason:       ImagePullBackOff
    Ready:          False
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/log/nginx from shared-logs (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-lzbmf (ro)
  sidecar-container:
    Container ID:  containerd://6dd08d8f463bafed4bd8a9ed52afb4757c81ff1ad3b7b430064756220fcfa883
    Image:         ubuntu:latest
    Image ID:      docker.io/library/ubuntu@sha256:ec050c32e4a6085b423d36ecd025c0d3ff00c38ab93a3d71a460ff1c44fa6d77
    Port:          <none>
    Host Port:     <none>
    Command:
      sh
      -c
      while true; do cat /var/log/nginx/access.log /var/log/nginx/error.log; sleep 30; done
    State:          Running
      Started:      Mon, 21 Aug 2023 10:04:43 +0000
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/log/nginx from shared-logs (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-lzbmf (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             False 
  ContainersReady   False 
  PodScheduled      True 
Volumes:
  shared-logs:
    Type:       EmptyDir (a temporary directory that shares a pod's lifetime)
    Medium:     
    SizeLimit:  <unset>
  kube-api-access-lzbmf:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type     Reason     Age                    From               Message
  ----     ------     ----                   ----               -------
  Normal   Scheduled  3m54s                  default-scheduler  Successfully assigned default/webserver to kodekloud-control-plane
  Normal   Pulling    3m52s                  kubelet            Pulling image "ubuntu:latest"
  Normal   Pulled     3m48s                  kubelet            Successfully pulled image "ubuntu:latest" in 4.280753847s (4.28076727s including waiting)
  Normal   Created    3m48s                  kubelet            Created container sidecar-container
  Normal   Started    3m48s                  kubelet            Started container sidecar-container
  Normal   Pulling    3m8s (x3 over 3m53s)   kubelet            Pulling image "nginx:latests"
  Warning  Failed     3m8s (x3 over 3m52s)   kubelet            Failed to pull image "nginx:latests": rpc error: code = NotFound desc = failed to pull and unpack image "docker.io/library/nginx:latests": failed to resolve reference "docker.io/library/nginx:latests": docker.io/library/nginx:latests: not found
  Warning  Failed     3m8s (x3 over 3m52s)   kubelet            Error: ErrImagePull
  Normal   BackOff    2m29s (x6 over 3m47s)  kubelet            Back-off pulling image "nginx:latests"
  Warning  Failed     2m29s (x6 over 3m47s)  kubelet            Error: ImagePullBackOff
```

**3.  To resolve the images issue edit the pod and do the changes**

```
thor@jump_host ~$ kubectl edit pod webserver
pod/webserver edited

note:  Image:  nginx:latests   ->  Image: nginx:latest
```

**4.  Wait for  pods to get running status**

```
thor@jump_host ~$ kubectl get pods
NAME        READY   STATUS    RESTARTS   AGE
webserver   2/2     Running   0          11m
```

**5.  Click on `Finish` & `Confirm` to complete the task successful.**
