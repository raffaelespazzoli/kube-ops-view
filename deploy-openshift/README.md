# Deploy on OpenShift

## kube-ops-view without Authentication

```
oc new-project ocp-ops-view
oc create sa kube-ops-view
oc adm policy add-scc-to-user anyuid -z kube-ops-view
oc adm policy add-cluster-role-to-user cluster-admin system:serviceaccount:ocp-ops-view:kube-ops-view
oc apply -f https://raw.githubusercontent.com/raffaelespazzoli/kube-ops-view/ocp/deploy-openshift/kube-ops-view.yaml
oc expose svc kube-ops-view
oc get route | grep kube-ops-view | awk '{print $2}'
```


## kube-ops-view with Authentication

Replace the `FIXME` entries to provide the correct parameters. Use the following command to display all available parameters and their description:
```
oc process -f https://raw.githubusercontent.com/appuio/kube-ops-view/ocp/deploy-openshift/kube-ops-view-oauth.yaml --parameters
```

Deploy the application with:
```
oc new-project ocp-ops-view
oc create sa kube-ops-view
oc adm policy add-scc-to-user anyuid -z kube-ops-view
oc adm policy add-cluster-role-to-user cluster-admin system:serviceaccount:ocp-ops-view:kube-ops-view
oc process -f https://raw.githubusercontent.com/appuio/kube-ops-view/ocp/deploy-openshift/kube-ops-view-oauth.yaml --param=KUBE_OPS_VIEW_APP_DOMAIN=FIXME --param=KUBE_OPS_VIEW_AUTHORIZE_URL=FIXME --param=KUBE_OPS_VIEW_ACCESS_TOKEN_URL=FIXME --param=KUBE_OPS_VIEW_CLIENT_ID=FIXME --param=KUBE_OPS_VIEW_CLIENT_SECRET=FIXME | oc create -f -
```

