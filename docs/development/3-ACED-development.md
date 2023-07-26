# Instructions for ACED Gen3 Deployment

## Install Rancher

Install Rancher Desktop from [Github Releases page](https://github.com/rancher-sandbox/rancher-desktop/releases/latest)

Note: after restarting rancher desktop, you will need to ensure this value is still set to 262144:

```sh
echo 'sudo sysctl -p' | rdctl shell
# vm.max_map_count = 262144
```

### Increase Elasticsearch Memory

As referenced in the [Gen3 developer docs](gen3_developer_environments.md#elasticsearch-error), Elasticsearch may output an error regarding too low of `max virtual memory` —

```
ERROR: [1] bootstrap checks failed
[1]: max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]
```

To fix this we'll open a shell into the Rancher Desktop node and update the required memory fields —

```sh
rdctl shell

sudo sysctl -w vm.max_map_count=262144
sudo sh -c 'echo "vm.max_map_count=262144" >> /etc/sysctl.conf'

sysctl vm.max_map_count
# vm.max_map_count = 262144
```

## Install Kubernetes

```sh
brew install kubectl
```

## Install Helm

```sh
brew install helm
helm repo add gen3 https://helm.gen3.org
helm repo update
```

## Switch to AWS k8s Context

```sh
export AWS_DEFAULT_PROFILE=staging

aws eks update-kubeconfig --region us-west-2 --name aced-commons-staging
# Updated context arn:aws:eks:us-west-2:119548034047:cluster/aced-commons-staging in /Users/beckmanl/.kube/config

kubectl config current-context
# arn:aws:eks:us-west-2:119548034047:cluster/aced-commons-staging
```

## Switch to Local (Rancher Desktop) k8s Context
```sh
# To switch to the local k8s context
kubectl config use-context rancher-desktop
# Switched to context "rancher-desktop".
```

## Deploy the ACED Commons

```sh
# Clone gen3-helm 
git clone https://github.com/ACED-IDP/gen3-helm
git checkout feature/etl

# uninstall previous version
helm uninstall local
# update dependencies
helm dependency update ./helm/gen3

# Start deployment 
helm upgrade --install local ./helm/gen3 -f values.yaml -f user.yaml -f fence-config.yaml -f Secrets/TLS/gen3-certs.yaml
```

## Add ETL Pod

> Login to browser first, download credentials.json to Secrets/credentials.json

```sh
kubectl delete configmap credentials
kubectl create configmap credentials --from-file Secrets
kubectl delete pod etl
kubectl apply -f etl.yaml
sleep 10
# kubectl describe pod etl
kubectl exec --stdin --tty etl -- /bin/bash
```

## ACED Specific Files

* gitops.json - Controls windmill UI configuration - see values.yaml
  Gitops values are encoded as a json string under portal.gitops.json
  
    ```yaml
    portal:
    ...  
    # -- (map) GitOps configuration for portal
        gitops:
        # -- (string) multiline string - gitops.json
        json: |
    ```

* fence-config.yaml - Authentication config. Same as legacy compose services file except addition of header

    ```yaml
    fence:
    FENCE_CONFIG:
        APP_NAME:
        ...
    ```

* user.yaml - Authorization config. Same as legacy compose services file except addition of header, note contents are a yaml string

    ```yaml
    fence:
    USER_YAML: |
        ...
    ```

* certs
    *See [OneDrive](https://ohsuitg-my.sharepoint.com/:f:/r/personal/walsbr_ohsu_edu/Documents/compbio-tls?csf=1&web=1&e=7oFdxd)*

    Copy the keys into the `gen3-certs.yaml` file.

    ```sh
    cp Secrets/TLS/gen3-certs-example.yaml Secrets/TLS/gen3-certs.yaml
    cat service.* >> Secrets/TLS/gen3-certs.yaml
    # Then match the key-key value formats in the file
    ```

## Helpful Commands

### Listing Secrets

```sh
kubectl get pods -o json | jq '.items[].spec.containers[].env[]?.valueFrom.secretKeyRef.name' | grep -v null | sort | uniq
```

### Counting pods neither Running or Completed

```sh
kubectl get pods --all-namespaces | grep -v Running | grep -v Completed  | grep -v NAMES | wc -l
```

### Manually change SSL certificate

The SSL certificate and key file are automatically handled by the `Secrets/TLS/gen3-certs.yaml` and invoked in the `helm upgrade` command. However if you wish change the certificate or key for any reason simply delete the `gen3-certs` secret and recreate it with the `crt` and `key` file you wish to use:

```sh
kubectl delete secrets gen3-certs
kubectl create secret tls gen3-certs --cert=Secrets/TLS/new-service.crt --key=Secrets/TLS/new-service.key
```
