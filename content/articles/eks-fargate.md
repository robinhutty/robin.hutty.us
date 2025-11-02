---
title: On EKS with Fargate for Compute
date: 2025-06-25
categories: IaaS
tags: aws
type: docs
draft: true
sidebar:
  open: true
---

## Introduction

With rise of Kubernetes as a platform for so many companies' workloads and with the understanding that the operational aspects of running Kubernetes clusters is complex and difficult, it should be clear why

## Limitations/Consequences

* One Pod <=> One Fargate Node; consequences for HPA?
* Graviton (ARM64) not available for EKS Fargate, so workloads must run on AMD64

## References
