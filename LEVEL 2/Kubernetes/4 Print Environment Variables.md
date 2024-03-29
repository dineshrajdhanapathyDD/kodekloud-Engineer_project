

## Questions:

The Nautilus DevOps team is working on to setup some pre-requisites for an application that will send the greetings to different users. There is a sample deployment, that needs to be tested. Below is a scenario which needs to be configured on Kubernetes cluster. Please find below more details about it.


1. Create a `pod` named `print-envars-greeting`.

2. Configure spec as, the container name should be `print-env-container` and use `bash` image.

3. Create three environment variables:

- a. `GREETING` and its value should be `Welcome to`

- b. `COMPANY` and its value should be `xFusionCorp`

- c. `GROUP` and its value should be `Datacenter`

- Use command to echo `["$(GREETING) $(COMPANY) $(GROUP)"]` message.

-  You can check the output using `kubectl logs -f print-envars-greeting` command.


`Note`: The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.


## Solution:  

**1. At first  check existing deployment and  pods running status**

```

thor@jump_host ~$ kubectl get pods
No resources found in default namespace.
```

**2.  Create yaml file with all the parameters**

```

thor@jump_host ~$ vi /tmp/env.yml
thor@jump_host ~$ cat /tmp/env.yml
apiVersion: v1
kind: Pod
metadata:
  name: print-envars-greeting
spec:
  containers:
    - name: print-env-container
      image: bash
      imagePullPolicy: IfNotPresent
      env:
        - name: GREETING
          value: Welcome to
        - name: COMPANY
          value: xFusionCorp
        - name: GROUP
          value: Datacenter
      command:
        - echo
      args: ["$(GREETING) $(COMPANY) $(GROUP)"]
  restartPolicy: Always
```

**3. Run below command to create pod**

```

thor@jump_host ~$ kubectl create -f /tmp/env.yml
pod/print-envars-greeting created
```

**4.  Wait for pods to get running status**

```

thor@jump_host ~$ kubectl get pods
NAME                    READY   STATUS      RESTARTS     AGE
print-envars-greeting   0/1     Completed   1 (8s ago)   10s
```

**5.  Validate the task by running below command** 

```

thor@jump_host ~$ kubectl logs -f print-envars-greeting
Welcome to xFusionCorp Datacenter
```

**6.  Click on `Finish` & `Confirm` to complete the task successful**


