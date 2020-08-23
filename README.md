# K8s-flux
Using Flux CD and Flux Helm operators, I can maintain my clusters
- Local cluster (laptop)
- Production cluster (cluster on Digital Ocean)

**IMPORTANT**  
All HELM values should be kept in the helm values.yaml file, and not overriden in the flux YAML files, unless absolutely necessary.  This allows you to run helm charts directly w/o flux  


## Install Helm Operator CRD (all environments)
Using https://docs.fluxcd.io/projects/helm-operator/en/stable/get-started/quickstart/

```
export FLUX_FORWARD_NAMESPACE=flux
```
Namespace first for everything
```
kubectl create ns flux
```
Add the Flux helm-operator via HELM
```
kubectl apply -f https://raw.githubusercontent.com/fluxcd/helm-operator/1.2.0/deploy/crds.yaml
helm repo add fluxcd https://charts.fluxcd.io
helm upgrade -i helm-operator fluxcd/helm-operator \
--namespace flux \
--set helm.versions=v3
```

**Note: If you get errors, run the following and try helm-operator again**
```
kubectl delete ClusterRole/flux ClusterRole/helm-operator ClusterRoleBinding/flux ClusterRoleBinding/helm-operator
```

## Install Flux via Helm

### Local (laptop) cluster
```
export GHUSER="jfmatth"
helm upgrade -i flux fluxcd/flux \
--set git.user=${GHUSER} \
--set git.email=${GHUSER}@users.noreply.github.com \
--set git.url=git@github.com:${GHUSER}/K8s-flux \
--set git.path=cluster-local \
--set syncGarbageCollection.enabled=true \
--namespace flux
```

### Staging / Production (DO) cluster

**CONNECT TO YOUR PRODUCTION CLUSTER**

```
export GHUSER="jfmatth"
helm upgrade -i flux fluxcd/flux \
--set git.user=${GHUSER} \
--set git.email=${GHUSER}@users.noreply.github.com \
--set git.url=git@github.com:${GHUSER}/K8s-flux \
--set git.path=cluster-production \
--set syncGarbageCollection.enabled=true \
--namespace flux
```

Follow directions for the SSH key Identity
```
fluxctl identity
```

