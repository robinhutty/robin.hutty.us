---
title: SWE on MS Windows
type: docs
draft: true
sidebar:
  open: true
---

And if so, does IT allow you to have admin rights on that machine?
(If they do, that will make things easier. If they don't, it'll be fine, we might just have to modify the approach somewhat: https://docs.chocolatey.org/en-us/choco/setup/#non-administrative-install)

If you don't already have a process for managing what software is installed, you'll probably want to use software called Chocolatey because you'll need to install a bunch of software for this adventure and chocolatey helps make it all a bit more manageable.

So to get started, read/follow https://docs.chocolatey.org/en-us/choco/setup/#installing-chocolatey-cli

And then prove it works by using it to install Visual Studio Code (which is probably the most popular software development software and probably what you want to use) by opening a command shell and running

```powershell
choco install visualstudiocode
```



I have rarely managed machines running any variant of Microsoft's Windows Operating System[^last], so I've never known much - and it's been a while so (a) I've probably forgotten most of it and (b) things have changed anyway. Here's to trying to collect a little useful info that might serve me as a starting point/reminder for next time.

[^last]: The last time I worked in professional capacity on a machine running Windows (_other_ than a company-provided/required laptop) was more than a decade ago.

## Configuration Management

* Chocolatey, WinGet
* Ansible

## PowerShell
