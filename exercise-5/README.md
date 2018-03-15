# Exercise 5 - Installing Istio

### Install istioctl

If you are not at the `istio-workshop` directory, change to that directory.  Istio-0.6.0 should be in the istio-workshop directory.

1. Add the `istioctl` client to your PATH. Since this terminal is an emulator:  
   
   #### Copy the istioctl over

   ```
   sudo cp istio-0.6.0/bin/istioctl /usr/local/bin
   ```
   When promted with `[sudo] password for student:`, enter `passw0rd`.
   
### Install Istio on the Kubernetes cluster

1. Change the directory to the Istio file location.

   ```
   cd istio-0.6.0
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

#### [Continue to Exercise 6 - Creating a service mesh with Istio Proxy](../exercise-6/README.md)
