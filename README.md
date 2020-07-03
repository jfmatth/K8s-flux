# K8s-flux
Using Flux CD and Flux Helm operators, I can maintain my clusters
- Local (laptop)
- Development (cluster on DO)
- Production (cluster on DO)

## Local (laptop) cluster

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
    
- Install Flux
    Using https://docs.fluxcd.io/en/1.19.0/tutorials/get-started/ as a baseline.

    ```
    export GHUSER="jfmatth"
    fluxctl install \
    --git-user=${GHUSER} \
    --git-email=${GHUSER}@users.noreply.github.com \
    --git-url=git@github.com:${GHUSER}/flux-get-started \
    --git-path=cluster-local \
    --namespace=flux | kubectl apply -f -
    ```

After the above steps are completed, Flux will begin rolling out the HelmRelease file from the cluster-local folder in this repo.  You can check the pods are being created.



