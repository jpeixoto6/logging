# grafana

This directory contains an example configuration for Grafana 9 to be used with a LokiStack instance.

There is currently only one variant "ocp-oauth" which configures Grafana to use the OpenShift OAuth service for authentication.

## ocp-oauth Overlay

There are two configuration values that need to be set based on the cluster and LokiStack configuration:

- `GATEWAY_ADDRESS` in `patch_deployment_env.yaml`, based on the name and namespace of the LokiStack
- `CLUSTER_ROUTES_BASE` in `patch_deployment_oauth.yaml`, based on the DNS name of the cluster

For example, if your LokiStack is called `logging-loki` in the namespace `openshift-logging` and your cluster domain (the part after `.apps.` in routes) is `my-cluster.devcluster.openshift.com` then the following configuration values would be correct:

```plain
GATEWAY_ADDRESS=logging-loki-gateway-http.openshift-logging.svc:8080
CLUSTER_ROUTES_BASE=my-cluster.devcluster.openshift.com
```

Once these two configuration values have been set in the files, the configuration can be applied using:

```bash
oc apply -k overlays/ocp-oauth/
```

The configuration creates a `grafana` deployment and route in the `openshift-logging` namespace. Your Grafana instance will be reachable using `https://grafana-openshift-logging.apps.${CLUSTER_ROUTES_BASE}/`.
# logging
