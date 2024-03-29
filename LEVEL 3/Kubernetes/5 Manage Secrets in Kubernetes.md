

## Questions:

The Nautilus DevOps team is working to deploy some tools in Kubernetes cluster. Some of the tools are licence based so that licence information needs to be stored securely within Kubernetes cluster. Therefore, the team wants to utilize Kubernetes secrets to store those secrets. Below you can find more details about the requirements:


1. We already have a secret key file `news.txt` under `/opt` location on `jump host`. Create a `generic secret` named `news`, it should contain the password/license-number present in `news.txt` file.

2. Also create a `pod` named `secret-nautilus`.

3. Configure pod's `spec` as container name should be `secret-container-nautilus`, image should be `centos` preferably with `latest` tag `remember to mention the tag with image`. Use `sleep` command for container so that it remains in running state. Consume the created secret and mount it under `/opt/apps` within the container.

4. To verify you can exec into the container `secret-container-nautilus`, to check the secret key under the mounted path `/opt/apps`. Before hitting the `Check` button please make sure pod/pods are in running state, also validation can take some time to complete so keep patience.

`Note`: The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.


## Solution:  

**1. At first create a secret named as beta as per the given path**

```

thor@jump_host ~$ cat /opt/news.txt
5ecur3
thor@jump_host ~$ kubectl create secret generic news --from-file=/opt/news.txt
secret/news created
thor@jump_host ~$ ls /opt
news.txt
thor@jump_host ~$ 
```

**2.  Create  a YAML  file with all the parameters**

```

thor@jump_host ~$ vi /tmp/secret.yml
thor@jump_host ~$ cat /tmp/secret.yml
apiVersion: v1

kind: Pod

metadata:

  name: secret-nautilus

  labels:

    name: myapp

spec:

  volumes:

    - name: secret-volume-nautilus

      secret:

        secretName: news

  containers:

    - name: secret-container-nautilus

      image: centos:latest

      command: ["/bin/bash", "-c", "sleep 10000"]

      volumeMounts:

        - name: secret-volume-nautilus

          mountPath: /opt/apps

          readOnly: true
thor@jump_host ~$ 
```

**3. Run the below command to create a pod**

```

thor@jump_host ~$ kubectl create -f /tmp/secret.yml
pod/secret-nautilus created
thor@jump_host ~$ 
```

**4.  Wait for pods to get running status** 

```

thor@jump_host ~$ kubectl get pods
NAME              READY   STATUS    RESTARTS   AGE
secret-nautilus   1/1     Running   0          41s
thor@jump_host ~$ 
```

**5.  Validate the task by running the below command** 

```
thor@jump_host ~$ kubectl exec secret-nautilus -- cat /opt/apps/news.txt
5ecur3
```

**6.  Click on `Finish` & `Confirm` to complete the task successfully**


