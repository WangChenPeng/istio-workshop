# Exercise 11 - Security

#### Overview of Istio Security

Istio Auth’s aim is to enhance the security of microservices and their communication without requiring service code changes. It is responsible for:

* Providing each service with a strong identity that represents its role to enable interoperability across clusters and clouds.

* Securing service to service communication and end-user to service communication.

* Providing a key management system to automate key and certificate generation, distribution, rotation, and revocation.

### Istio Mutual TLS

Service-to-service communication is tunneled through the client side Envoy and the server side Envoy. End-to-end communication is secured by:

* Local TCP connections between the service and Envoy.

* Mutual TLS connections between proxies.

* Secure Naming: during the handshake process, the client side Envoy checks that the service account provided by the server side certificate is allowed to run the target service.

### Steps
#### Verifying Istio’s mutual TLS authentication setup
Verify the cluster-level CA is running:   
```sh
kubectl get deploy -l istio=istio-ca -n istio-system
```
Istio CA is up if the “AVAILABLE” column is 1. For example:
```sh
NAME       DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
istio-ca   1         1         1            1           1m
```
#### Verify AuthPolicy setting in ConfigMap
```sh
kubectl get configmap istio -o yaml -n istio-system | grep authPolicy | head -1
```
Istio mutual TLS authentication is enabled if the line `authPolicy: MUTUAL_TLS` is uncommented (doesn’t have a `#`).

#### Trying out the authentication setup

One way to try out the mutual TLS authentication communication, is to use curl in one service’s envoy proxy to send request to other services. For example, after starting the Helloworld application you can ssh into the envoy container of Helloworld service, and send request to guestbook service by curl.

1. get the Helloworld pod name
```sh
kubectl get pods -l app=guestbook-ui
```
```sh
NAME                            READY     STATUS    RESTARTS   AGE
guestbook-ui-596d68d88f-qxhzk   2/2       Running   1          1h
```
Make sure the pod is “Running”.

2. ssh into the envoy container
```sh
kubectl exec -it guestbook-ui-xxxxxxxx -c istio-proxy /bin/bash
```
Make sure to change the pod name into the corresponding one on your system. This command will ssh into istio-proxy container(sidecar) of the pod.

3. check out the keys
```sh
ls /etc/certs/ 
```
You should see
```sh
cert-chain.pem   key.pem   root-cert.pem
```
Note that cert-chain.pem is envoy’s cert that needs to present to the other side. key.pem is envoy’s private key paired with cert-chain.pem. root-cert.pem is the root cert to verify the other side’s cert. Currently we only have one CA, so all envoys have the same root-cert.pem.   

4. send request to the guestbook-ui service
```sh
curl https://guestbook-service:8080 -v --key /etc/certs/key.pem --cert /etc/certs/cert-chain.pem --cacert /etc/certs/root-cert.pem -k
```
From the output there will be some error message `error fetching CN from cert:The requested data were not available`. This is expected. 
Go to the bottom and there you will see the success message.

```sh
< HTTP/1.1 200 OK
< x-application-context: helloworld-service
< content-type: application/json;charset=UTF-8
< date: Thu, 01 Feb 2018 06:12:46 GMT
...
```

Congratulations! You have finished the lab. If you want to find out more about Istio, try out more advanced features, or follow more examples and guides, you can find all this and more at https://istio.io/docs/.

