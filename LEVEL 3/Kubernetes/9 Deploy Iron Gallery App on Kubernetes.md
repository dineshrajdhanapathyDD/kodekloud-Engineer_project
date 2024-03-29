

## Questions:

There is an iron gallery app that the Nautilus DevOps team was developing. They have recently customized the app and are going to deploy the same on the Kubernetes cluster. Below you can find more details:



1. Create a namespace `iron-namespace-nautilus`

2. Create a deployment `iron-gallery-deployment-nautilus` for `iron gallery` under the same namespace you created.

- Labels `run` should be `iron-gallery`.

- Replicas count should be `1`.

- Selector's matchLabels `run` should be `iron-gallery`.

- Template labels `run` should be `iron-gallery` under metadata.

- The container should be named as `iron-gallery-container-nautilus`, use `kodekloud/irongallery:2.0` image ` use exact image name / tag `.

- Resources limits for memory should be `100Mi` and for CPU should be `50m`.

- First volumeMount name should be `config`, its mountPath should be `/usr/share/nginx/html/data`.

- Second volumeMount name should be `images`, its mountPath should be `/usr/share/nginx/html/uploads`.

- First volume name should be `config` and give it `emptyDir` and second volume name should be `images`, also give it `emptyDir`.

3. Create a deployment `iron-db-deployment-nautilus` for `iron db` under the same namespace.

- Labels `db` should be `mariadb`.

- Replicas count should be `1`.

- Selector's matchLabels (db0 should be `mariadb`).

- Template labels `db` should be `mariadb` under metadata.

- The container name should be `iron-db-container-nautilus`, use `kodekloud/irondb:2.0` image ` use exact image name / tag `.

- Define environment, set `MYSQL_DATABASE` its value should be `database_apache`, set `MYSQL_ROOT_PASSWORD` and `MYSQL_PASSWORD` value should be with some complex passwords for DB connections, and `MYSQL_USER` value should be any custom user ` except root `.

- Volume mount name should be `db` and its mountPath should be `/var/lib/mysql`. Volume name should be `db` and give it an `emptyDir`.

4. Create a service for `iron db` which should be named `iron-db-service-nautilus` under the same namespace. Configure spec as selector's db should be `mariadb`. Protocol should be `TCP`, port and targetPort should be `3306` and its type should be `ClusterIP`.

5. Create a service for `iron gallery` which should be named `iron-gallery-service-nautilus` under the same namespace. Configure spec as selector's run should be `iron-gallery`. Protocol should be `TCP`, port and targetPort should be `80`, nodePort should be `32678` and its type should be `NodePort`.


`Note`:


We don't need to make connection b/w database and front-end now, if the installation page is coming up it should be enough for now.

The `kubectl` on `jump_host` has been configured to work with the kubernetes cluster.




## Solution:  

**1. At first  kubectl  utility configure and working from jump server**

```

thor@jump_host ~$ kubectl get ns,pods,deploy
NAME                           STATUS   AGE
namespace/default              Active   6m6s
namespace/kube-node-lease      Active   6m6s
namespace/kube-public          Active   6m6s
namespace/kube-system          Active   6m6s
namespace/local-path-storage   Active   5m54s
```

**2. Create yaml  file with all the parameters**

```

thor@jump_host ~$ vi deploy.yml
thor@jump_host ~$ cat deploy.yml
--- 
apiVersion: v1
kind: Namespace
metadata:
  name: iron-namespace-devops
  labels:
    name: iron-namespace-devops
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: iron-gallery-deployment-devops
  namespace: iron-namespace-devops
  labels:
    run: iron-gallery
spec:
  replicas: 1
  selector:
    matchLabels:
      run: iron-gallery
  template:
    metadata:
      labels:
        run: iron-gallery
    spec:
      volumes:
        - name: config
          emptyDir: {}
        - name: images
          emptyDir: {}
      containers:
        - name: iron-gallery-container-devops
          image: kodekloud/irongallery:2.0
          volumeMounts:
            - name: config
              mountPath: /usr/share/nginx/html/data
            - name: images
              mountPath: /usr/share/nginx/html/uploads
          resources:
            limits:
              memory: "100Mi"
              cpu: "50m"    
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: iron-db-deployment-devops
  namespace: iron-namespace-devops
  labels:
    db: mariadb
spec:
  replicas: 1
  selector:
    matchLabels:
      db: mariadb
  template:
    metadata:
      labels:
        db: mariadb
    spec:
      volumes:
        - name: db
          emptyDir: {}
      containers:
        - name: iron-db-container-devops
          image: kodekloud/irondb:2.0
          env:
            - name: MYSQL_DATABASE
              value: database_blog
            - name: MYSQL_ROOT_PASSWORD
              value: admin123
            - name: MYSQL_PASSWORD
              value: admin123
            - name: MYSQL_USER
              value: kodekloud
          volumeMounts:
            - name: db
              mountPath: /var/lib/mysql

---
apiVersion: v1
kind: Service
metadata:
  name: iron-db-service-devops
  namespace: iron-namespace-devops
spec:
  type: ClusterIP
  selector:
    db: mariadb
  ports:
    - port: 3306
      targetPort: 3306
      protocol: TCP
---
apiVersion: v1
kind: Service
metadata:
  name: iron-gallery-service-devops
  namespace: iron-namespace-devops
spec:
  type: NodePort
  selector:
    run: iron-gallery
  ports:
    - port: 80
      targetPort: 80
      nodePort: 32678
      protocol: TCP  
thor@jump_host ~$ 
```

**3. created the deploy.yml file**

```

thor@jump_host ~$ kubectl apply -f deploy.yml
namespace/iron-namespace-devops created
deployment.apps/iron-gallery-deployment-devops created
deployment.apps/iron-db-deployment-devops created
service/iron-db-service-devops created
service/iron-gallery-service-devops created
thor@jump_host ~$ 
```

**4. Get namespace iron-namspace-devops**

```

thor@jump_host ~$ k get ns
NAME                    STATUS   AGE
default                 Active   15m
iron-namespace-devops   Active   23s
kube-node-lease         Active   15m
kube-public             Active   15m
kube-system             Active   15m
local-path-storage      Active   15m
thor@jump_host ~$ 
```

**5. Wait for pod, service & deployment running status**

```

thor@jump_host ~$ k get all -n iron-namespace-devops
NAME                                                 READY   STATUS    RESTARTS   AGE
pod/iron-db-deployment-devops-58cbb788f9-555wf       1/1     Running   0          49s
pod/iron-gallery-deployment-devops-9c67c8cb7-r7rsc   1/1     Running   0          49s

NAME                                  TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
service/iron-db-service-devops        ClusterIP   10.96.123.180   <none>        3306/TCP       49s
service/iron-gallery-service-devops   NodePort    10.96.204.115   <none>        80:32678/TCP   49s

NAME                                             READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/iron-db-deployment-devops        1/1     1            1           49s
deployment.apps/iron-gallery-deployment-devops   1/1     1            1           49s

NAME                                                       DESIRED   CURRENT   READY   AGE
replicaset.apps/iron-db-deployment-devops-58cbb788f9       1         1         1       49s
replicaset.apps/iron-gallery-deployment-devops-9c67c8cb7   1         1         1       49s
thor@jump_host ~$ 
```

**6.  Validate the task by login on GUI web console**

![iron gallery](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/92ade9e6-de68-49c3-ac0e-8e54e9899812)


**7.  Click on `Finish` & `Confirm` to complete the task successful**


