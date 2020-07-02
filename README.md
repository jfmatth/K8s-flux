# K8s-helm
A repository to hold my Kubernetes infrastructure helm charts

Install Flux CD and Helm-operator before doing anything.


## Development branch / cluster

development-env.yaml - This is a HelmRelease

```
kubectl apply -f development-env.yaml
```

This will bring up the aim development Kubernetes namespace aim-dev and all the necessary parts to push development


