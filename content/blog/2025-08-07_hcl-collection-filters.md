---
title: Filtering Collections in Terrform/HCL
date: 2025-08-07
type: blog
categories:
tags: hcl,tf,code
draft: false
---

HCL provides functions for some set operations (`intersection`, `product`, `subtract`, `union`), but they are only for variables of type `set` (although there is also the `contains` function which works with variables of type `list` as well, which is an ordered version of a `set`).

## Lists

```hcl
locals {
    first_list = ["foo", "bar", "baz"]
    second_list = ["foo", "a", "b"]
}
```

* Filter the `first_list` to only include elements that are _also_ members of `second_list`: `intersection_list = [for X in first_list if contains(second_list, X)]`
* Filter the `first_list` to exclude members of `second_list`: `smaller_list = [for X in first_list if !contains(second_list, X)]`

## Maps


```hcl
locals {
    first_map = {
        "foo" = "foo_val",
        "bar" = "bar_val",
        "baz" = "baz_val"
    }
    second_map = {
        "foo" = "different_foo_value",
        "quux" = "quux_val",
    }
}
```

* (Shallow) merge: `merge(first_map, second_map)`
* Second-level merge: `{ for k, v in first_map : k => merge(v, try(second_map[k], {})) }`
* A recursive/deep merge is left as an exercise for the reader :)
* For each key/value pair in the first map, include it in the output map if that key is _not_ present in the second map
    ```hcl
    smaller_map = { for k,v in first_map : k => v if !contains(keys(second_map), k) }
    ```
