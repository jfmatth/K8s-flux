# K8s-flux

# FLUX IS NO LONGER USED FOR THIS, but the Repo is being used for ArgoCD for now.

# Misc.

## Backup
pgo create schedule sotb-v1-qa  \
--schedule="0 1 * * *"  \
--schedule-type=pgbackrest  \
--pgbackrest-backup-type=full \
--schedule-opts="--repo1-retention-full=14"


Using Flux CD and Flux Helm operators, I can maintain my clusters
- Local cluster (laptop)
- Production cluster (cluster on Digital Ocean)

**IMPORTANT**  
All HELM values should be kept in the helm values.yaml file, and not overriden in the flux YAML files, unless absolutely necessary.  This allows you to run helm charts directly w/o flux  


## Folders
Charts - Holds all HELM charts to be used for AIM  
- aim-dev - development on DO cluster  
- aim-k8sprod - production copy on DO cluster, mimics current VM code-base  
- aim-laptop - laptop chart for proper ingress name, etc  

cluster-local - FLUX local cluster definitions (not used much)  

cluster-production - FLUX production Digital Ocean charts / etc.  





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

### Home cluster

You can develop locally with just HELM.

```
helm upgrade -i dev aim-dev --render-subchart-notes
```

if you want to use FLUX locally.. see below  

```
export GHUSER="jfmatth"
helm upgrade -i flux fluxcd/flux \
--set git.user=${GHUSER} \
--set git.email=${GHUSER}@users.noreply.github.com \
--set git.url=git@github.com:${GHUSER}/K8s-flux \
--set git.path=cluster-home \
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

