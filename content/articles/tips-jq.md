---
title: "Tips: jq"
date: 2025-09-22
modified: 2026-02-11T16:38:30
categories: productivity
tags: cli, jq, tips
type: docs
draft: true
sidebar:
  open: true
---

Here I'm just collecting some things I seem to need help remembering and do not have a better place.

## jq

```json
[
  { name = "foo", size = "large" },
  { name = "bar", size = "medium" },
  { name = "baz", size = "medium" }
]
```

Filtering by the Value of a Field:

```shell
jq -r '.[] | select(.size=="medium").name'
```

Including multiple fields in the output:

```shell
jq -r '.[] | "\(.name) \(.size)"'
```

This allows things like the following which, assuming that my AWS NAT Gateway Resources are tagged with 'env_name', will give me the set of public IPs used (by my VPCs) in each AWS Region I use. (This is a real example from when someone wanted to write NACL Rules to allow traffic from my VPCs)

```shell
for R in ap-south-1 ap-southeast-2 ca-central-1 eu-west-1 me-central-1 us-west-2;
do
  aws ec2 describe-nat-gateways --region=$R --filter Name=tag-key,Values=env_name | \
  jq -r '.NatGateways[] | "\(.Tags[]|select(.Key =="env_name").Value) \(.NatGatewayAddresses[].PublicIp)"' \
  | sort;
done
```
