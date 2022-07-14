
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

In the ocp folder, you can find the files needed to deploy the application. 

You only have to apply the ApplicationSet:

```zsh
❯ oc apply -f gitops/golang-applicationset.yaml
applicationset.argoproj.io/kustomize-bootstrap created
```

## Test the application

We are going to test our application. Firstly we will see the state in ArgoCD dashboard:

// TODO: añadir imagen

Once we are sure that the application is running, we will execute an endpoint, for example the ```greetings`` endpoint:

```zsh
❯ SERVICE_ENDPOINT=$(oc get route -n dev-monitoring | grep golang | awk '{print $2}')
❯ curl $SERVICE_ENDPOINT/api/v1/greetings
{"msg":"Hello world"}
```

Finally, we can explore the ```metrics``` endpoint that we will use with Grafana and Prometheus in the next step:

```zsh
❯ curl $SERVICE_ENDPOINT/metrics 
# HELP go_gc_duration_seconds A summary of the pause duration of garbage collection cycles.
# TYPE go_gc_duration_seconds summary
go_gc_duration_seconds{quantile="0"} 0
go_gc_duration_seconds{quantile="0.25"} 0
go_gc_duration_seconds{quantile="0.5"} 0
go_gc_duration_seconds{quantile="0.75"} 0
go_gc_duration_seconds{quantile="1"} 0
go_gc_duration_seconds_sum 0
go_gc_duration_seconds_count 0
```

> NOTE: It's an application's responsibility to expose the ```/metrics``` endpoint, like Prometheus format awaits.

# Metrics

The OpenShift monitoring architecture is the following:

![OCP Monitoring overview](images/ocp-monitoring.svg)

By default, the OpenShift installation process installs and configures all the pieces. We can override or customize most parameters.

The main components that we are going to explore are ```Grafana``` and ```Prometheus```

## Prometheus

Prometheus has the responsibility to get the information about the metrics along the pods.

By default, Prometheus calls the ```/metrics``` endpoint, finding for a specific format. Most language programming has the corresponding library to expose these metrics.

At this point, we will find our application metrics in Prometheus. Our first step is to find the endpoint and enter the UI. 

```zsh
❯ oc get route -A | grep prometheus | awk '{print $3}'
```



# References
- https://docs.openshift.com/container-platform/4.9/monitoring/monitoring-overview.html