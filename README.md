## Workshop Setup
- [Exercise 1 - Accessing a Kubernetes cluster with IBM Cloud Container Service](exercise-1/README.md)

## Creating a Service Mesh with Istio

- [Exercise 2 - Installing Istio](exercise-2/README.md)
- [Exercise 3 - Creating a service mesh with Istio Proxy](exercise-3/README.md)
- [Exercise 4 - Istio Ingress controller](exercise-4/README.md)
- [Exercise 5 - Telemetry](exercise-5/README.md)

## Credits
These workshop exercises are built with the help from a number of amazing Kubernetes and Istio experts from Google and Grand Cloud.

#### Ray Tsang  [@saturnism](https://twitter.com/saturnism)
The Kubernetes and Istio exercises are derived from the work of Ray Tsang  [@saturnism](https://twitter.com/saturnism) and these repositories:

[https://github.com/saturnism/spring-boot-docker](https://github.com/saturnism/spring-boot-docker)

[https://github.com/saturnism/istio-by-example-java](https://github.com/saturnism/istio-by-example-java)

#### Zach Butcher [@ZachButcher](https://twitter.com/ZackButcher)
Zach was instrumental in helping write the Istio tutorials.

####  Kelsey Hightower [@kelseyhightower](https://twitter.com/kelseyhightower)
The Istio Ingress Tutorial is largely based on the work of Kelsey and this repository:

[https://github.com/kelseyhightower/istio-ingress-tutorial](https://github.com/kelseyhightower/istio-ingress-tutorial)

Kelsey's tutorial uses more advance features of Kubernetes to taint some of the nodes so that the Ingress controller runs on dedicated nodes. The Ingress controller is then deployed as a daemonset.
