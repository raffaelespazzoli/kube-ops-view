# kube-ops-view for OpenShift

Because of the standardization of the metrics endpoints we cna now install kube-ops-view directly

## Install using the helm chart (tillerless approach)

```shell
export OCP_OPS_VIEW_ROUTE=<add your route fqdn here>
oc new-project ocp-ops-view
helm fetch stable/kube-ops-view
oc adm policy add-scc-to-user anyuid -z default -n ocp-ops-view
helm template kube-ops-view-1.0.0.tgz --name=kube-ops-view-stable --namespace ocp-ops-view --set redis.enabled=true --set rbac.create=true --set ingress.enabled=true --set ingress.hostname=$OCP_OPS_VIEW_ROUTE | oc apply -f -
```

## Install using static manifest

In case you can 't use helm, a static manifest approach is provided

```shell
oc new-project ocp-ops-view
oc adm policy add-scc-to-user anyuid -z default -n ocp-ops-view
oc apply -f ocp-ops-view.yaml
```
