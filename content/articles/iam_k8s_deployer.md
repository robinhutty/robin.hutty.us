---
title: Provide limited access to Kubernetes to an AWS IAM Role
date: 2025-08-20
modified: 2025-09-08T07:59:51
type: docs
categories: IaaS
tags: kubernetes,aws
draft: false
---

I want to grant an AWS IAM Role the ability to "bounce" a workload, i.e. to run the equivalent of `kubectl -n foo rollout restart deployment/foo`.

<!-- Cite: https://aws.plainenglish.io/kubernetes-auto-restart-deployments-when-k8s-configmap-or-secret-is-updated-ad2c042a745f -->

## In AWS

TODO:

## In Kubernetes

### An example workload

The last line of this manifest `serviceAccountName: ...` in `spec.template.spec` is what ties _this_ deployment to the Service Account created below; otherwise, it's just the example [run-stateless-application-deployment](https://kubernetes.io/docs/tasks/run-application/run-stateless-application-deployment) in the Kubernetes documentation.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  namespace: foo
  name: nginx
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 2 # tells deployment to run 2 pods matching the template
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
      serviceAccountName: restart-sa
```

### A (Kubernetes)  Role

This Role's permissions allows _editing_ deployments which is broader than just rolling out restarts, but it's a good place to start.

```yaml
kind: Role
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  namespace: foo
  name: restarter
rules:
  - apiGroups: ["apps"]
    resources: ["deployments","replicasets", "pods"]
    resourceNames: ["nginx-deployment"] # or ["*"]
    verbs: ["get", "patch"]
```

### A ServiceAccount that can be used by an AWS IAM Role

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: restart-sa
  annotations:
    eks.amazonaws.com/role-arn: <IAM_ROLE_ARN>
```

### Binding the ServiceAccount to the (Kubernetes) Role

```yaml
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: rb-restarter
  namespace: foo
subjects:
  - kind: ServiceAccount
    name: restart-sa
    namespace: foo
roleRef:
  kind: Role
  name: restarter
  apiGroup: rbac.authorization.k8s.io
```
