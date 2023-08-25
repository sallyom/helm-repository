## Deploy Apache server

```bash
oc new-project <securesign-helm, securesign-helm-v2>
# or 'oc project securesign-helm,securesign-helm-v2'
# with oc, you don't need to specify the ns in the following cmds,
# if using 'kubectl', add a '-n ns-name' to the cmds.

oc create configmap charts-charts --from-file ../charts/
oc create configmap charts-index --from-file ../index.yaml
oc create -f pvc.yaml
oc create -f service.yaml
oc create -f deployment.yaml
oc create -f route.yaml
