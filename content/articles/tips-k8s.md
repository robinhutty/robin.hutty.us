---
title: "Tips: Kubernetes"
date: 2025-09-08
modified: 2026-02-11T16:38:49
categories: IaaS
tags: k8s
type: docs
draft: true
---

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
