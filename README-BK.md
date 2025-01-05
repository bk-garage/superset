# Bulent's Notes

## Using Helm

See:
* `helm/superset/README.md`
* https://superset.apache.org/docs/installation/kubernetes/


Custom values are in `helm/superset/values-custom.yaml`.

```zsh
cd helm/superset
helm install -f values-custom.yaml -f values-secret.yaml superset superset/superset -n superset --create-namespace
```

## Deploying to EKS

TODO: Try Superset helm to deploy to an EKS cluster.

The following has worked:
* https://github.com/bk-garage/data-on-eks/tree/main/analytics/terraform/superset-on-eks
* https://awslabs.github.io/data-on-eks/docs/blueprints/data-analytics/superset-on-eks


### Installing EKS

https://github.com/aws-samples/eks-blueprints-actions-workflow
https://docs.aws.amazon.com/eks/latest/userguide/getting-started-console.html


## Deploying to OpenShift Local

Much faster than Rancher Desktop.

* Added `oc-rb-custom-scc.yaml` and `oc-role-custom-scc.yaml`.
* Changed `values-custom.yaml` to add Service Accounts for Postgresql and Redis.

## Deploying to Rancher Desktop

Select the K8s context:
Right-click on the RD icon -> Kubernetes Contexts -> rancher-desktop

Deployment succeeded with the two values files:
```zsh
cd helm/superset
helm install -f values-custom.yaml -f values-secret.yaml superset superset/superset -n superset --create-namespace
helm uninstall superset -n superset
```

Use RD's **Port Forwarding feature**. With the `kubectl port-forward` command the connection breaks.

The application was too slow. Connected to the database, but could not load the schema. OpenShift Local was much faster.

### Delete the app
```zsh
helm uninstall superset -n superset
```
Then delete the namespace for a clean start.


## Using Superset

Get database IP for setting up the connection:
```
k describe po superset-postgresql-0 -n superset
```
