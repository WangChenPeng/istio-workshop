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
   kubectl apply -f install/kubernetes/istio.yaml
   ```

### Install Add-ons for Grafana, Prometheus and Jaeger

```sh
kubectl apply -f install/kubernetes/addons/grafana.yaml
kubectl apply -f install/kubernetes/addons/prometheus.yaml
kubectl apply -f install/kubernetes/addons/servicegraph.yaml
kubectl apply -n istio-system -f https://raw.githubusercontent.com/jaegertracing/jaeger-kubernetes/master/all-in-one/jaeger-all-in-one-template.yml
```

### View the Istio deployments

Istio is deployed in a separate Kubernetes namespace `istio-system`  You can watch the state of Istio and other services and pods using the watch flag (`-w`) when listing Kubernetes resources. For example, in two separate terminal windows run:

```sh
kubectl get pods -w --all-namespaces
```
```sh
kubectl get services -w --all-namespaces
```

#### [Continue to Exercise 6 - Creating a service mesh with Istio Proxy](../exercise-3/README.md)
