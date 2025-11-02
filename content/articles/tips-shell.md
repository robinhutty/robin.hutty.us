---
title: Shell Tips
date: 2025-09-19
modified: 2025-09-19T14:32:36
categories: productivity
tags: programming, tips
type: docs
draft: true
---

## Parallelization

Using bash[^not-bash], process a whole set of files, N files at once:

```bash
N=4
process_single_file {
	local fn="$1"
	echo "Doing stuff to ${fn}"
}

(
for my_file in $(find . -type f -print0);
do
   ((i=i%N)); ((i++==0)) && wait
   process_single_file "$my_file" &
done
)
```

## Coping with unreliable execution via retry attempts

* recur
* attempt-cli

[^not-bash]: There are lots of other ways to achieve parallelization with various trade-offs for complex/flexible/portable vs. simple/portable/convenient, etc. See [xargs(1)](https://pubs.opengroup.org/onlinepubs/9799919799/utilities/xargs.html), [GNU Parallel](), and [comparison](https://www.gnu.org/software/parallel/man.html#differences_between_xargs_and_gnu_parallel) of `xargs` and `parallel`.
