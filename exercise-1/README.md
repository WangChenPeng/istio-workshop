# Exercise 1 - Accessing a Kubernetes cluster 

This is the section to configure your kubectl CLI to point to your Kubernetes cluster. For the convienence we will use Minikube here.

### Install Minikube on Mac

The following are the quick steps to install Minikube on Mac if you have homebrew installed:

```
brew cask install virtualbox

brew cask install minikube
```

Make sure the minikube has at least 4G of memory(more is better). Otherwise it will not be sufficient to run Istio.

```
minikube start 
    --extra-config=controller-manager.cluster-signing-cert-file="/var/lib/localkube/certs/ca.crt" 
    --extra-config=controller-manager.cluster-signing-key-file="/var/lib/localkube/certs/ca.key" 
    --extra-config=apiserver.admission-control="NamespaceLifecycle,LimitRanger,ServiceAccount,PersistentVolumeLabel,DefaultStorageClass,DefaultTolerationSeconds,MutatingAdmissionWebhook,ValidatingAdmissionWebhook,ResourceQuota" 
    --kubernetes-version=v1.10.0
 ```

### Else

In case the first approach doesn't work, here are the full references to set up Minikube:
- [Install Minikube](https://kubernetes.io/docs/tasks/tools/install-minikube/)
- [Minikube release](https://github.com/kubernetes/minikube/releases)
- [Vbox download](https://www.virtualbox.org/wiki/Downloads)
- [Set up Minikube](https://kubernetes.io/docs/setup/minikube/)


### Clone the lab repo

From your command line, run:
   
```    
git clone https://github.com/szihai/istio-workshop

cd istio-workshop
```

This is the working directory for the lab.

### Next step
directly go into Istio topics:
#### [Continue to Exercise 5 - Installing Istio](../exercise-5/README.md)
OR go through the Kubenetes basics
#### [Continue to Exercise 2 - Deploying a microservice to Kubernetes](../exercise-2/README.md)

