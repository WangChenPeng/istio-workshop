# Exercise 5 - Installing Istio

### Download Istio

1. Either download Istio directly from https://github.com/istio/istio/releases or get the latest version using curl:

   ```
   curl -L https://git.io/getLatestIstio | sh -
   ```

2. Extract the installation files.
   
3. Add the `istioctl` client to your PATH. For example, run the following command on a MacOS or Linux system:   
   
   #### Remember to change the [version] to the current value

   ```
   export PATH=$PWD/istio-[version]/bin:$PATH
   ```

### Install Istio on the Kubernetes cluster

1. Change the directory to the Istio file location.

   ```
   cd [path_to_istio-version]
   ```
   
2. Install Istio on the Kubernetes cluster.

   ```
   kubectl apply -f install/kubernetes/istio-demo-auth.yaml
   ```


### View the Istio deployments

Istio is deployed in a separate Kubernetes namespace `istio-system`  You can watch the state of Istio and other services and pods using the watch flag (`-w`) when listing Kubernetes resources. For example, in two separate terminal windows run:

```sh
kubectl get pods -w --all-namespaces
```
```sh
kubectl get services -w --all-namespaces
```
## What just happened

Congratulations! You have installed Istio into the Kubernetes cluster. A lot has been installed:

- Istio Controllers and related RBAC rules
- Istio Custom Resource Definitions 
- Prometheus and Grafana for Monitoring
- Jeager for Distributed Tracing
- Istio Sidecar Injector (we'll take a look next next section)

#### [Continue to Exercise 6 - Creating a service mesh with Istio Proxy](../exercise-6/README.md)
