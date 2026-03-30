---
title: "Tips: Kubernetes"
date: 2025-09-08
modified: 2026-03-30T15:44:48
categories: cloud-native,IaaS,PaaS
tags: k8s
type: docs
draft: false
---

## Conveniences

### Contexts

I often need to work with multiple Kubernetes clusters; for example, one set of environments that I work with has a "one production cluster per geo-region" architecture, plus there are various clusters for development, staging/UAT, etc.
Mostly the use case is investigative activity where I want to verify assumptions/hopes such as "the pipeline (or one of the controllers/operators/autoscaling/etc.) should have done X, did it?".
This kind of activity is common when I'm troubleshooting (workloads) and when I'm attempting to test various kinds of automation, but also when I'm tinkering: exploring new functionality, local clusters (such as minikube/kind/etc.).
And so I want to see what Resources are present and their configuration/state, possibly comparing different clusters, etc.

Kubernetes clients support a [kubeconfig file][1] to configure access to cluster(s).

Sometimes I will have [.env][2] functionality available or a `kubeconfig` in the working directory.
Otherwise [kubectx][2] and [kubecm][3] are helpful tools.

[1]: https://kubernetes.io/docs/tasks/access-application-cluster/configure-access-multiple-clusters/
[2]: https://www.dotenv.org/
[3]: https://github.com/ahmetb/kubectx
[4]: https://github.com/sunny0826/kubecm

I tend to use this little function most often (for my AWS EKS clusters):

```bash
function k8s {
    local STAGE="$1"
    local REGION="$2"
    export REGION=$REGION; export STAGE=$STAGE;
    export INFRA=${STAGE}-${REGION}; export AWS_REGION=$(expand_short_region $REGION)
    [[ $commands[aws] ]] && aws eks update-kubeconfig --name $INFRA
}
```

(which relies on another function to expand the abbreviations that we use for AWS Regions)

### Restarting a deployment

```bash
function krr {
  local WORKLOAD="$1"
  local NAMESPACE="$2"
  kubectl -n ${NAMESPACE:-default} rollout restart "deployment/${WORKLOAD:-DUMMY}"
}

```

### Helm

Empty a specified (or default) list of K8s namespaces of their helm-installed workloads:

```bash
function helmnsdelete {
    local nsen=($@)
    if [[ ${#nsen[@]} == 0 ]]; then nsen=(external-workloads internal-workloads extra-workloads); fi
    for NS in ${nsen[@]};
    do
        echo $NS
        for SVC in $(helm -n $NS ls -o json | jq -r '.[].name');
        do
            echo $SVC
            helm -n $NS uninstall $SVC;
        done;
    done
}
```

## Try It and See

From a quick peek at the K8s docs[^k8s-docs], it was not immediately clear to me whether, in a Kubernetes manifest, I could declare both `env` and `envFrom` to provide Environment Variables to a container (spoiler: "yes"). And if I can, which one "wins" in the case where both approaches attempt to set the same Environment Variable?

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: test-dummy
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: test-dummy-configmap
  namespace: test-dummy
data:
  TEST_ENV_VAR: dummy-value-via-envfrom-configmap
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: test-dummy-busybox
  namespace: test-dummy
spec:
  selector:
    matchLabels:
      app: busybox
  template:
    metadata:
      labels:
        app: busybox
    spec:
      containers:
      - name: busybox
        image: busybox
        command: [ "/bin/bash", "-c", "--" ]
        args: [ "while true; do printenv; sleep 10; done;" ]
        envFrom:
        - configMapRef:
            name: test-dummy-configmap
        env:
        - name: TEST_ENV_VAR
          value: dummy-value-via-env
```

This will spam the container logs[^logs] every 10 seconds with the container's Environment, but we can look at it directly with:

```console
$ kubectl -n test-dummy \
    exec $(kubectl get pods -n test-dummy -o jsonpath='{.items[0].metadata.name}') \
    -- sh -c "printenv"
```

[^logs]: `kubectl -n test-dummy logs --follow $(kubectl get pods -n test-dummy -o jsonpath='{.items[0].metadata.name}')`
[^k8s-docs]: I looked in the 'Tasks' and 'Concepts' sections first (in order to avoid digging), but I should have just checked the [PodSpec Reference](https://kubernetes.io/docs/reference/kubernetes-api/workload-resources/pod-v1/#PodSpec). Kubernetes has _lots_ of documentation, and much of it is excellent, but the examples tend to be minimal demonstration of the concept in question. And for good reason; but that doesn't help me when I'm considering real-world interactions.

## See Also

* GitOps for K8s: [Flux](https://github.com/fluxcd/flux2) or [ArgoCD](https://argo-cd.readthedocs.io/)
* [Collabnix's list of tools](https://collabnix.github.io/kubetools/)
