

## Questions:

The Nautilus DevOps team want to deploy a static website on Kubernetes cluster. They are going to use Nginx, phpfpm and MySQL for the database. The team had already gathered the requirements and now they want to make this website live. Below you can find more details:



1. Create some secrets for `MySQL`.

Create a secret named `mysql-root-pass` wih key/value pairs as below:

- name: password
- value: R00t



Create a secret named `mysql-user-pass` with key/value pairs as below:

- name: username
- value: kodekloud_cap

- name: password
- value: Rc5C9EyvbU



Create a secret named `mysql-db-url` with key/value pairs as below:

- name: database
- value: kodekloud_db6



Create a secret named `mysql-host` with key/value pairs as below:

- name: host
- value: mysql-service



2. Create a config map `php-config` for `php.ini` with `variables_order = "EGPCS"` data.

3. Create a deployment named `lemp-wp`.


4. Create two containers under it. First container must be `nginx-php-container` using image `webdevops/php-nginx:alpine-3-php7` and second container must be `mysql-container` from image `mysql:5.6`. Mount `php-config` configmap in nginx container at `/opt/docker/etc/php/php.ini` location.


5) Add some environment variables for both containers:


`MYSQL_ROOT_PASSWORD`, `MYSQL_DATABASE`, `MYSQL_USER`, `MYSQL_PASSWORD` and `MYSQL_HOST`. Take their values from the secrets you created. Please make sure to use env field `do not use envFrom` to define the name-value pair of environment variables.

6) Create a node port type service `lemp-service` to expose the web application, nodePort must be `30008`.


7) Create a service for mysql named `mysql-service` and its port must be `3306`.


We already have a `/tmp/index.php` file on `jump_host` server.


Copy this file into the `nginx` container under document root i.e `/app` and replace the dummy values for mysql related variables with the environment variables you have set for mysql related parameters. Please make sure you do not hard code the mysql related details in this file, you must use the environment variables to fetch those values.


Once done, you must be able to access this `website` using Website button on the top bar, please note that you should see `Connected successfully` message while accessing this page.


`Note`: The `kubectl` on `jump_host` has been configured to work with the kubernetes cluster.


## Solutions:

**1. First, check any running resources.**

```

thor@jump_host ~$ kubectl get all
NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   40m
thor@jump_host ~$ 
```

**2. Create the secrets.**

```

thor@jump_host ~$ kubectl create secret generic mysql-root-pass --from-literal=password=R00t
secret/mysql-root-pass created
thor@jump_host ~$ kubectl create secret generic mysql-user-pass --from-literal=username=kodekloud_cap --from-literal=password=Rc5C9Eyvbu
secret/mysql-user-pass created
thor@jump_host ~$ kubectl create secret generic mysql-db-url --from-literal=database=kodekloud_db6
secret/mysql-db-url created
thor@jump_host ~$ kubectl create secret generic mysql-host --from-literal=host=mysql-service
secret/mysql-host created
thor@jump_host ~$ 
```

**3. Create the deploy.yml.**

```

thor@jump_host ~$ vi /tmp/deploy.yml
thor@jump_host ~$ cat /tmp/deploy.yml
--- 
apiVersion: v1
kind: ConfigMap
metadata:
  name: php-config
data:
  php.ini: |
    variables_order = "EGPCS"
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: lemp-wp
spec:
  replicas: 1
  selector:
    matchLabels:
      app: lemp-wp
  template:
    metadata:
      labels:
        app: lemp-wp
    spec:
      containers:
        - name: nginx-php-container
          image: webdevops/php-nginx:alpine-3-php7
          volumeMounts:
            - name: php-config
              mountPath: /opt/docker/etc/php/php.ini
              subPath: php.ini
          env:
            - name: MYSQL_ROOT_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: mysql-root-pass
                  key: password
            - name: MYSQL_DATABASE
              valueFrom:
                secretKeyRef:
                  name: mysql-db-url
                  key: database
            - name: MYSQL_USER
              valueFrom:
                secretKeyRef:
                  name: mysql-user-pass
                  key: username
            - name: MYSQL_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: mysql-user-pass
                  key: password
            - name: MYSQL_HOST
              valueFrom:
                secretKeyRef:
                  name: mysql-host
                  key: host

        - name: mysql-container
          image: mysql:5.6
          ports:
          - containerPort: 3306
          resources: {}
          env:
            - name: MYSQL_ROOT_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: mysql-root-pass
                  key: password
            - name: MYSQL_DATABASE
              valueFrom:
                secretKeyRef:
                  name: mysql-db-url
                  key: database
            - name: MYSQL_USER
              valueFrom:
                secretKeyRef:
                  name: mysql-user-pass
                  key: username
            - name: MYSQL_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: mysql-user-pass
                  key: password
            - name: MYSQL_HOST
              valueFrom:
                secretKeyRef:
                  name: mysql-host
                  key: host
      volumes:
        - name: php-config
          configMap:
            name: php-config
---
apiVersion: v1
kind: Service
metadata:
  name: lemp-service
spec:
  type: NodePort
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30008
  selector:
    app: lemp-wp
---
apiVersion: v1
kind: Service
metadata:
  name: mysql-service
spec:
  ports:
    - port: 3306
  selector:
    app: lemp-wp  
```

**4. Apply depoly.yml**

```

thor@jump_host ~$ kubectl apply -f deploy.yml
configmap/php-config created
deployment.apps/lemp-wp created
service/lemp-service created
service/mysql-service created
thor@jump_host ~$ 
```

**5. Running status of pods**

```

thor@jump_host ~$ kubectl get pods
NAME                      READY   STATUS    RESTARTS   AGE
lemp-wp-ff9bff9db-hlmt9   2/2     Running   0          2m20s
thor@jump_host ~$ 
```

**6. For easier troubleshooting, we can save the pod name and container names.**

```

thor@jump_host ~$ NGINX=$(kubectl get pod  -o=jsonpath='{.items[*].spec.containers[0].name}') ; echo "NGINX: $NGINX"
NGINX: nginx-php-container
thor@jump_host ~$ 
thor@jump_host ~$ MYSQL=$(kubectl get pod -o=jsonpath='{.items[*].spec.containers[1].name}')  ; echo "MYSQL: $MYSQL"
MYSQL: mysql-container
thor@jump_host ~$ 
thor@jump_host ~$ POD=$(kubectl get pods -o=jsonpath='{.items[*].metadata.name}'); echo "POD: $POD"
POD: lemp-wp-ff9bff9db-x5bjm
thor@jump_host ~$ 
```

**7. Check the resources.**

```

thor@jump_host ~$ k get all
NAME                          READY   STATUS    RESTARTS   AGE
pod/lemp-wp-ff9bff9db-x5bjm   2/2     Running   0          19m

NAME                    TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
service/kubernetes      ClusterIP   10.96.0.1       <none>        443/TCP        65m
service/lemp-service    NodePort    10.96.75.224    <none>        80:30008/TCP   19m
service/mysql-service   ClusterIP   10.96.115.187   <none>        3306/TCP       19m

NAME                      READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/lemp-wp   1/1     1            1           19m

NAME                                DESIRED   CURRENT   READY   AGE
replicaset.apps/lemp-wp-ff9bff9db   1         1         1       19m
```

**8. Next, edit the index.php based on the instructions.**

```

thor@jump_host ~$ vi /tmp/index.php
thor@jump_host ~$ cat /tmp/index.php
<?php
$dbname = $_ENV["MYSQL_DATABASE"];
$dbuser = $_ENV["MYSQL_USER"];
$dbpass = $_ENV["MYSQL_PASSWORD"];
$dbhost = $_ENV["MYSQL_HOST"];:w!
$connect = mysqli_connect($dbhost, $dbuser, $dbpass) or die("Unable to Connect to '$dbhost'");

$test_query = "SHOW TABLES FROM $dbname";
$result = mysqli_query($test_query);

if ($result->connect_error) {
   die("Connection failed: " . $conn->connect_error);
}
  echo "Connected successfully";
```

**9. Copy the file onto the /app directory in the NGINX container.**
 
```
thor@jump_host ~$ kubectl cp /tmp/index.php $POD:/app -c $NGINX 
```

**10. Verify if the file was copied by opening a shell to the container.**

```

thor@jump_host ~$ kubectl exec -it $POD -c $NGINX -- ls -la /app
total 12
drwxr-xr-x    1 applicat applicat      4096 Oct  8 10:42 .
drwxr-xr-x    1 root     root          4096 Oct  8 10:22 ..
-rw-r--r--    1 root     root           435 Oct  8 10:42 index.php
thor@jump_host ~$ kubectl exec -it $POD -c $NGINX -- cat /app/index.php
<?php

$dbname = $_ENV["MYSQL_DATABASE"];
$dbuser = $_ENV["MYSQL_USER"];
$dbpass = $_ENV["MYSQL_PASSWORD"];
$dbhost = $_ENV["MYSQL_HOST"];

$connect = mysqli_connect($dbhost, $dbuser, $dbpass) or die("Unable to Connect to '$dbhost'");

$test_query = "SHOW TABLES FROM $dbname";
$result = mysqli_query($test_query);

if ($result->connect_error) {
   die("Connection failed: " . $conn->connect_error);
}
  echo "Connected successfully";
thor@jump_host ~$ 
```

**11. Finally, click the Website button at the upper right to open the app URL on a new tab.**

-> Connnected successfully


**12. Click on `Finish` & `Confirm` to complete the task successful**






