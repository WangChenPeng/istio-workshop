# Exercise 8 - IBM Front Door

IBM Front Door is IBM container service Ingress controller which is an enhancement of the Kubernetes Ingress Controller. The Front Door provides IBM Cloud users with a secure, reliable and scalable high performance network stack to distribute incoming traffic to their applications running on the IBM Cloud Platform. The additional features are simply configured as additional annotations in the yaml file and are deployed when configured. Some features supported by the Front Door Ingress Controller are:
* Unique subdomain with a free certificate
* DNS resolution for the Subdomain
* SSL (TLS) termination
* Layer 7 routing
* Load Balance to an application running on IBM container service
* Load Balance to an external application (specifying external endpoints)
* Highly Available Ingress Controller

In this exercise we'll create an Front Door Ingress and connect it to Istio ingress. And add the domain names to some services.

### Copy secret over
The Front Door ingress will be under the `istio-system` namespace. And the istio ingress created in Lab 6 is under the `default` namespace. To communicate across the namespaces, the secret has to be copied over.
```sh
kubectl get secret -n default
```

```sh
NAME                                   TYPE                                  DATA      AGE
bluemix-default-secret                 kubernetes.io/dockercfg               1         4d
bluemix-default-secret-international   kubernetes.io/dockercfg               1         4d
bluemix-default-secret-regional        kubernetes.io/dockercfg               1         4d
default-token-r2bv7                    kubernetes.io/service-account-token   3         4d
guestbook-242887                       Opaque                                2         4d
istio.default                          istio.io/key-and-cert                 3         2d
```
Pick the secret name showing `Opaque` in `Type`.
Now copy:
```sh
kubectl get secret [secret] -o yaml | sed 's/default/istio-system/g' | kubectl -n istio-system create -f -
```
To verify the secret being copied:
```sh
kubectl get secret -n guestbook
```
```sh
NAME                                        TYPE                                  DATA      AGE
...
guestbook-242887                            Opaque                                2         23s
...
```
### Deploy the Front Door Ingress
In our workshop, we are using `us-east` region. If you have a cluster from another region, please modify the `guestbook/frontdoor-ingress.yaml` accordingly.

Let's check the IBM Ingress secret and subdomain information.
```sh
bx cs cluster-get guestbook

...
Ingress subdomain:	guestbook-242887.us-east.containers.mybluemix.net
Ingress secret:		guestbook-242887
```
For this cluster, the subdomain name and secret name are the same `guestbook-242887`. But that is not always the case.
Change the template file with the secret name and subdomain name. Then create the Ingress.
```sh
cat guestbook/frontdoor-ingress.yaml| sed 's/xxxx/${secret}/g' | sed 's/ssss/${subdomain}/g' | kubectl -n istio-system create -f -
```
To examine the Ingress, run
```sh
kubectl get ingress istio-fd  -o yaml -n istio-system
```
```sh
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: istio-fd
  namespace: istio-system
spec:
  tls:
  - hosts:
    - xxxx.us-east.containers.mybluemix.net
    secretName: ssss
  rules:
  - host: xxxx.us-east.containers.mybluemix.net
    http:
      paths:
      - path: /
        backend:
          serviceName: istio-ingress
          servicePort: 80
  - host: jaeger.xxxx.us-east.containers.mybluemix.net
    http:
      paths:
      - path: /
        backend:
          serviceName: jaeger
          servicePort: 16686
  - host: grafana.xxxx.us-east.containers.mybluemix.net
    http:
      paths:
      - path: /
        backend:
          serviceName: grafana
          servicePort: 3000
status:
  loadBalancer: {}
```

Note the part where it connects to istio ingress:
```sh
backend:
   serviceName: istio-ingress
   servicePort: 80
```
Which corresponds to the `guestbook-ui` in istio ingress.   
Now let's access the guestbook service. Try `http://[subdomain].us-east.containers.mybluemix.net` and you'll see the guestbook gui.   
And go on with `http://jaeger.[subdomain].us-east.containers.mybluemix.net` and `http://grafana.[subdomain].us-east.containers.mybluemix.net` to access the zipkin and grafana services.  

#### [Continue to Exercise 9 - Telemetry](../exercise-9/README.md)
