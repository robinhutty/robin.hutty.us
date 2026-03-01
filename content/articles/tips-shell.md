---
title: "Tips: Shell"
date: 2025-09-19
modified: 2026-02-11T16:39:26
categories: productivity
tags: programming, tips, shell
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

## Timing

###

This is just for quick'n'dirty; probably don't. There are various bugs concerning subsecond precision, changing system clocks, datetime math complexity, and more.

```bash
start=$(date +%s); <command you want to time>; end=$(date +%s); runtime=$((end-start));
# echo "Command runtime was: $runtime"
10
```

### SECONDS

Bash has a builtin variable, `SECONDS`, which records the number of seconds since the shell started. But a script can reset it, so it's easy to use as a time marker.


### Definitions

Real
: wall clock time - time from start to finish of the call. This is probably what you want for hand-waving - unless you are investigating performance issues/profiling when you probably want 'user+sys'.

User
: CPU time spent in user-mode (i.e. not kernel) by the process. This excludes CPU time spent by other processes during this process's execution.

Sys
: CPU time spent in the kernel-mode by the process, syscalls and the like. As with 'user', this is only CPU time used by the process.


## Coping with unreliable execution via retry attempts

* recur
* attempt-cli

[^not-bash]: There are lots of other ways to achieve parallelization with various trade-offs for complex/flexible/portable vs. simple/portable/convenient, etc. See [xargs(1)](https://pubs.opengroup.org/onlinepubs/9799919799/utilities/xargs.html), [GNU Parallel](), and [comparison](https://www.gnu.org/software/parallel/man.html#differences_between_xargs_and_gnu_parallel) of `xargs` and `parallel`.
