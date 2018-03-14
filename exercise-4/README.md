# Exercise 4 - Scaling in and out

### Scale the number of Hello World service pods

1. Scale the number of replicas of your Hello World service by running the following commands:

    ```sh
    kubectl get deployment

    NAME                    DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
    helloworld-service-v1   1         1         1            1           1m
    ```

    ```sh
    kubectl scale deployment helloworld-service-v1 --replicas=4
    ```

    ```sh
    kubectl get deployment

    NAME                    DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
    helloworld-service-v1   4         4         4            4           1m
    ```

    ```sh
    kubectl get pods

    NAME                          READY     STATUS    RESTARTS   AGE
    helloworld-service-v1-...    1/1       Running   0          1m
    helloworld-service-v1-...    1/1       Running   0          1m
    helloworld-service-v1-...    1/1       Running   0          1m
    helloworld-service-v1-...    1/1       Running   0          2m
    ```

2. Try scaling out further.

    ```
    kubectl scale deployment helloworld-service-v1 --replicas=8
    ```
        
3. Verify replicates are scheduled and running.     

    ```
    kubectl get nodes
    kubectl get pods -o wide
    ```


#### [Continue to Exercise 5 - Installing Istio](../exercise-5/README.md)
