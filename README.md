## Helm repository for securesign/sigstore-ocp

This is how I update the helm repository served at
[https://repo-securesign-helm.apps.open-svc-sts.k1wl.p1.openshiftapps.com/helm-charts](https://repo-securesign-helm.apps.open-svc-sts.k1wl.p1.openshiftapps.com/helm-charts)

### Update the helm packages

Create helm packages for the `scaffolding` chart from [securesign/sigstore-ocp](https://github.com/securesign/sigstore-ocp),
the upstream `scaffold` chart, and/or the individual charts at [sigstore/helm-charts](https://github.com/sigstore/helm-charts).

#### Update securesign/scaffolding chart

```bash
cd path/to/securesign/sigstore-ocp/
git checkout main or v2-charts, depending on which scaffolding chart you are updating
helm package .
mv scaffold-*.tgz path/to/index/charts/.
cd back/to/index/dir
```

Now follow the [update helm repository server](#update-helm-repository-server) to push the updated repository to the OpenShift cluster
where an Apache server is running.

#### Update sigstore/helm-charts individual charts and/or scaffold chart

To update the V2 charts:

This is to update any of the upstream charts you want to include in the `scaffolding 2.0.0` chart in the `v2-charts` branch of
[securesign/sigstore-ocp](https://github.com/securesign/sigstore-ocp/tree/v2-charts)

```bash
cd path/to/sigstore/helm-charts
git remote add sallyom git@github.com:sallyom/helm-charts
git fetch sallyom && git checkout -b v2-charts sallyom/v2-sigstore-charts
helm package charts/[chart-you-want-to-update]
cp *.tgz charts/scaffold/charts/.
# we'll move the files also to the index/charts/. below
helm package charts/scaffold
cd back/to/index/dir
mv path/to/sigstore/helm-charts/*.tgz charts/.
```

Now follow the [update helm repository server](#update-helm-repository-server) to push the updated repository to the OpenShift cluster
where an Apache server is running.

### Update Helm repository Apache server

It is assumed you are working from local checkout of this repository.
You'll need to login to the OpenShift cluster that is serving the repository, you can get token
[here](https://oauth-openshift.apps.open-svc-sts.k1wl.p1.openshiftapps.com/oauth/token/display?code=sha256~UkZgpVvTCZshyAuvOgJEykzv_OjelSpEztyOQp8yshc&state=)

sigstore-ocp/main scaffolding chart is served at namespace `securesign-helm`
sigstore-ocp/v2-charts scaffolding chart is served at namespace `securesign-helm-v2`

#### Update scaffolding chart from securesign/sigstore-ocp main branch

Update the securesign/sigstore-ocp `main branch` scaffolding chart that wraps sigstore/helm-charts `main branch` scaffold chart:

```bash
cd ../
helm repo index index/ --url https://repo-securesign-helm.apps.open-svc-sts.k1wl.p1.openshiftapps.com/helm-charts
oc delete -n securesign-helm configmap charts-charts
oc delete -n securesign-helm configmap charts-index
oc create configmap -n securesign-helm charts-charts --from-file ./charts/
oc create configmap -n securesign-helm charts-index --from-file ./index.yaml
oc delete pods --all -n securesign-helm
```

go to [repository](https://repo-securesign-helm-v2.apps.open-svc-sts.k1wl.p1.openshiftapps.com/helm-charts),
confirm that it's been updated.

#### Update scaffolding chart from securesign/sigstore-ocp v2-charts branch

Update the securesign/sigstore-ocp `v2-charts branch` scaffolding chart that wraps sallyom/helm-charts `v2-sigstore-charts branch` scaffold chart:

```bash
cd ../
helm repo index index/ --url https://repo-securesign-helm-v2.apps.open-svc-sts.k1wl.p1.openshiftapps.com/helm-charts
oc delete -n securesign-helm-v2 configmap charts-charts
oc delete -n securesign-helm-v2 configmap charts-index
oc create configmap -n securesign-helm-v2 charts-charts --from-file ./charts/
oc create configmap -n securesign-helm-v2 charts-index --from-file ./index.yaml
oc delete pods --all -n securesign-helm-v2
```

go to [repository](https://repo-securesign-helm-v2.apps.open-svc-sts.k1wl.p1.openshiftapps.com/helm-charts),
confirm that it's been updated.

#### Update scaffolding chart from securesign/sigstore-ocp v2-charts branch

Update the securesign/sigstore-ocp `v2-charts branch` scaffolding chart that wraps sallyom/helm-charts `v2-sigstore-charts branch` scaffold chart:

```bash
cd ../
helm repo index index/ --url https://repo-securesign-helm-v2.apps.open-svc-sts.k1wl.p1.openshiftapps.com/helm-charts
oc delete -n securesign-helm-v2 configmap charts-charts
oc delete -n securesign-helm-v2 configmap charts-index
oc create configmap -n securesign-helm-v2 charts-charts --from-file ./charts/
oc create configmap -n securesign-helm-v2 charts-index --from-file ./index.yaml
oc delete pods --all -n securesign-helm-v2
```

go to [repository](https://repo-securesign-helm-v2.apps.open-svc-sts.k1wl.p1.openshiftapps.com/helm-charts),
confirm that it's been updated.
