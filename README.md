
# GitOps

GitOps isn't mandatory to run this example. I use it to facilitate this example and the agility.

If you prefer to avoid it, you can go directly to the following point.

## Install GitOps operator

Install the operator:

```zsh
❯ oc apply -f gitops/gitops-operator.yaml
subscription.operators.coreos.com/openshift-gitops created
```

Get the route:

```zsh
❯ oc get route -n openshift-gitops | grep server | awk '{print $2}'
openshift-gitops-server.{your-domain}.com
```

Go to the route and use your own credentials.

## Configure the application


# Install the application

# References
- https://docs.openshift.com/container-platform/4.9/monitoring/monitoring-overview.html