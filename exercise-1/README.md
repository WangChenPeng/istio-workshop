# Exercise 1 - Accessing a Kubernetes cluster with IBM Cloud Container Service

Your IBM Cloud paid account and your Kubernetes cluster have been pre-provisioned for you. Here are the steps to access your cluster:

### Install IBM Cloud Container Service command line utilities

1. Log in to the IBM Cloud CLI with your user id and password:   
   `bx login -u {userid} -p {password}`      


### Access your cluster

1. Set the context for your cluster in your CLI. Every time you log in to the IBM Bluemix Container Service CLI to work with the cluster, you must run these commands to set the path to the cluster's configuration file as a session variable. The Kubernetes CLI uses this variable to find a local configuration file and certificates that are necessary to connect with the cluster in IBM Cloud.

    a. List the available clusters. You should see the cluster `guestbook`.
    
    ```bash
    bx cs clusters
    ```
    
    b. Download the configuration file and certificates for your `guestbook` cluster using the `cluster-config` command.
    
    ```bash
    bx cs cluster-config guestbook
    ```
    
    c. Copy and paste the output command from the previous step to set the `KUBECONFIG` environment variable and configure your CLI to run `kubectl` commands against your cluster.

2. Obtain your kubernetes cluster token.

    ```
    kubectl config view -o jsonpath='{.users[0].user.auth-provider.config.id-token}'
    ```

3. Create a proxy to your Kubernetes API server.

    ```
    kubectl proxy
    ```
    
4. In a browser, go to http://localhost:8001/ui to access the API server dashboard.   Choose the `Token` option and paste in the token obtained earlier from step 2 into the token field and click `SIGN IN`.

4. View details of your cluster.
    ```
    bx cs cluster-get guestbook
    ```

5. Verify the worker nodes in the cluster.   
    ```
    bx cs workers guestbook
    bx cs worker-get [worker name]
    ```
### Clone the lab repo

From your command line, run:
   
```    
git clone -b openlab https://github.com/szihai/istio-workshop

cd istio-workshop
```

This is the working directory for the lab.

### Next step

If you are not familar with Kubernetes, we recommend you to learn Kubernetes first.

#### [Continue to Exercise 2 - Deploying a microservice to Kubernetes](../exercise-2/README.md)

If you are already familar with Kubernetes, feel free to proceed to exercise 5 to learn Istio.

#### [Continue to Exercise 5 - Installing Istio](../exercise-5/README.md)
