# K8s-flux
Using Flux CD and Flux Helm operators, I can maintain my clusters
- Local cluster (laptop)
- Production cluster (cluster on DO)


```
export FLUX_FORWARD_NAMESPACE=flux
```

## Local (laptop) cluster

- Install Helm Operator CRD 
    Using https://docs.fluxcd.io/projects/helm-operator/en/stable/get-started/quickstart/

    Namespace first for everything
    ```
    kubectl create ns flux
    ```
    
    Then add the Flux helm-operator via HELM
    ```
    kubectl apply -f https://raw.githubusercontent.com/fluxcd/helm-operator/1.2.0/deploy/crds.yaml
    helm repo add fluxcd https://charts.fluxcd.io
    helm upgrade -i helm-operator fluxcd/helm-operator \
    --namespace flux \
    --set helm.versions=v3
    ```

    Note: If you get errors, check the following:
    ```
        kubectl get ClusterRole,ClusterRoleBinding
    ```
    Delete all roles for flux and helm-operator

    
- Install Flux
    Using https://docs.fluxcd.io/en/1.19.0/tutorials/get-started/ as a baseline.

    ```
    export GHUSER="jfmatth"
    fluxctl install \
    --git-user=${GHUSER} \
    --git-email=${GHUSER}@users.noreply.github.com \
    --git-url=git@github.com:${GHUSER}/K8s-flux \
    --git-path=cluster-local \
    --namespace=flux | kubectl apply -f -
    ```

    Follow the instructions on putting the SSH identity key into your Repo

## Staging / Production (DO) cluster

**CONNECT TO YOUR PRODUCTION CLUSTER**

- Install Helm Operator CRD 
    Using https://docs.fluxcd.io/projects/helm-operator/en/stable/get-started/quickstart/

    Namespace first for everything
    ```
    kubectl create ns flux
    ```
    
    Then add the Flux helm-operator via HELM
    ```
    kubectl apply -f https://raw.githubusercontent.com/fluxcd/helm-operator/1.1.0/deploy/crds.yaml
    helm repo add fluxcd https://charts.fluxcd.io
    helm upgrade -i helm-operator fluxcd/helm-operator \
    --namespace flux \
    --set helm.versions=v3
    ```

    Note: If you get errors, check the following:
    * ClusterRole,ClusterRoleBinding for flux and helm-operator

    
- Install Flux
    Using https://docs.fluxcd.io/en/1.19.0/tutorials/get-started/ as a baseline.

    ```
    export GHUSER="jfmatth"
    fluxctl install \
    --git-user=${GHUSER} \
    --git-email=${GHUSER}@users.noreply.github.com \
    --git-url=git@github.com:${GHUSER}/K8s-flux \
    --git-path=cluster-production \
    --namespace=flux | kubectl apply -f -
    ```

    Follow the instructions on putting the SSH identity key into your Repo



