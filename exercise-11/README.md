# Exercise 11 - Service Isolation

## Service Isolation Using Mixer

We'll block access to the Guestbook Service by adding the `deny-guestbook-service.yaml` rule shown below.

```sh
kubectly apply -f istio/deny-guestbook-service.yaml
```

Visit Guestbook UI and see that Guestbook Service can no longer be accessed.

To remove denial, you can delete the rule.

```sh
kubectly delete -f istio/deny-guestbook-service.yaml
```

Congratulations! You have finished the lab. If you want to find out more about Istio, try out more advanced features, or follow more examples and guides, you can find all this and more at https://istio.io/docs/.

