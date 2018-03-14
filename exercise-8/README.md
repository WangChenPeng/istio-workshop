# Exercise 8 - Telemetry

First we need to configure Istio to automatically gather telemetry data for services running in the mesh.

1. Create a rule to collect telemetry data.

    ```sh
    istioctl create -f guestbook/guestbook-telemetry.yaml
    ```
2. Generate a small load to the application.

    ```sh
    while sleep 0.5; do curl http://$INGRESS_IP:$PORT/hello/world; done
    ```

## View guestbook telemetry data

### Grafana
Establish port forwarding from local port 3000 to the Grafana instance:
```sh
kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=grafana \
  -o jsonpath='{.items[0].metadata.name}') 3000:3000
```

Browse to http://localhost:3000 and navigate to the Istio Dashboard.

### Jaeger
Establish port forwarding from local port 16686 to the Jaeger instance:
```sh
kubectl port-forward -n istio-system \
$(kubectl get pod -n istio-system -l app=jaeger -o \
jsonpath='{.items[0].metadata.name}') 16686:16686 &
```

Browse to http://localhost:16686.

### Prometheus
Establish port forwarding from local port 9090 to the Prometheus instance:
```sh
kubectl -n istio-system port-forward \
  $(kubectl -n istio-system get pod -l app=prometheus -o jsonpath='{.items[0].metadata.name}') \
  9090:9090
```

Browse to http://localhost:9090/graph, and in the “Expression” input box, enter: `request_count`. Click **Execute**.


### Service Graph
Establish port forwarding from local port 8088 to the Service Graph instance:
```sh
kubectl -n istio-system port-forward \
  $(kubectl -n istio-system get pod -l app=servicegraph -o jsonpath='{.items[0].metadata.name}') \
  8088:8088
```

Browse to http://localhost:8088/dotviz.

#### Mixer Log Stream

```sh
kubectl -n istio-system logs $(kubectl -n istio-system get pods -l istio=mixer -o jsonpath='{.items[0].metadata.name}') mixer | grep \"instance\":\"newlog.logentry.istio-system\"
```

Congratulations! You have finished the lab. If you want to find out more about Istio, try out more advanced features, or follow more examples and guides, you can find all this and more at https://istio.io/docs/.
