# Exercise 3 - Creating a Kubernetes service

Each pod has a unique IP address, but the address is ephemeral. The pod IP addresses are not stable and can change when pods start or restart. A service provides a single access point to a set of pods matching some constraints. A service IP address is stable.

In Kubernetes, you can instruct the underlying infrastructure to create an external load balancer by specifying the service type as a LoadBalancer. If you open up [helloworldservice-service.yaml](../guestbook/helloworldservice-service.yaml), you will see that it has `type: NodePort`.

### Create a Kubernetes service

1. Create a Kubernetes service for the Hello World service.

    ```
    kubectl apply -f kubernetes/helloworldservice-service.yaml --record
    ```

2. Verify that service was created. Note the second port in the `PORT(S)` field for the service.

    ```bash
    kubectl get services
    
    NAME                 CLUSTER-IP      EXTERNAL-IP      PORT(S)          AGE
    helloworld-service   172.21.48.166   <none>           8080:32678/TCP   1m
    ```

3. Obtain the Kubernetes worker VM's public IP.  Note the `Public IP` for the worker VM.

    ```bash
    $ bx cs workers my_cluster
    OK
    ID                                                 Public IP         Private IP      Machine Type   State    Status   Zone    Version
    kube-hou02-pafb5ac29a060e4bc3861d1e774503f682-w1   184.172.233.171   10.76.196.127   free           normal   Ready    hou02   1.9.3_1502
    ```


### Test the Hello World service

Curl the external IP address to test the Hello World service.

    ```
    curl [WORKER-PUBLIC-IP]:32678/hello/world
    ```

#### Optional - Curl the service using a DNS name

If you log in to another container, you can access the Hello World service via the DNS name. For example, start `tutum/curl` to get a shell and curl the service using the service name:

```
kubectl run curl --image=tutum/curl -i --tty

root@busybox:/data# curl http://helloworld-service:8080/hello/Batman
{"greeting":"Hello Batman from helloworld-service-... with 1.0","hostname":"helloworld-service-...","version":"1.0"}

root@busybox:/data# exit
```

## Explanation
#### By Ray Tsang [@saturnism](https://twitter.com/saturnism)

Open the [helloworldservice-service.yaml](helloworldservice-service.yaml) to examine the service descriptor. The important part about this file is the selector section. This is how a service knows which pod to route the traffic to, by matching the selector labels with the labels of the pods.

The other important part to notice in this file is the type of service is a Node Port.  This tells Bluemix that an externally facing service and it is accessible from the outside via one of the Kubernetes's worker VM.

#### [Continue to Exercise 4 - Scaling in and out](../exercise-4/README.md)
