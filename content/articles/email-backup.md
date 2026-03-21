---
title: Email with offlineimap
date: 2025-09-02
modified: 2026-02-11T18:07:48
categories: productivity
tags: email, backup
type: docs
draft: false
---

This approach downloads an (offline) copy of the entire IMAP mailbox[^not-backup].

## offlineimap

Using the following configuration for [offlineimap][1] (`~/.offlineimaprc` is the default, but I'm locating it with my backup mailbox), I can copy the entire contents of the IMAP account.

```
[general]
accounts = my-example-acct
metadata = /mnt/backups/offlineimap/metadata

[Account my-example-acct]
localrepository = my-example-Local
remoterepository = my-example-Remote

[Repository my-example-Local]
type = Maildir
localfolders = /mnt/backups/offlineimap/my-example-acct/robin
sync_deletes = no

[Repository my-example-Remote]
type = IMAP
sslcacertfile = OS-DEFAULT
remotehost = imap.example.com
remoteuser = my-username
```

* `sync_deletes = no` in the `Local` section is saying "even if the remote mailbox deletes messages, don't delete from my copy".
* `sslcacertfile = OS-DEFAULT` tells offlineimap to use the OS provided CA certs for verifying the IMAP server's certificate(s).

Then I can run offlineimap as a one-off with:

```
offlineimap -c /mnt/backups/offlineimap/offlineimaprc --ignore-keyring -o
```

And then we're done, right? If I just do this regularly (by hand, as a cron job, or with something like systemd) then I'm good?*_NO_*

What if another client (i.e. a "regular" email client such as Thunderbird) modifies my messages[^sync_deletes] in the source mailbox on the server, perhaps because of a bug, but more likely because of user error(PEBKAC)? Offlineimap will happily sync those _bad_ changes to my local copy and my "backup" provides no value over my regular client.

## Actual Backup

Now that we have a one-off copy of the data, let's add some functionality so that we have actual backup capability.

See https://www.offlineimap.org/doc/backups.html

[^sync_deletes]: `sync_deletes = no` protects us from _deletes_ but what about other changes such as message corruption? Not very likely you might think, but for me at least, my email store/history is _really_ important to me - damage/loss could cause me barely imaginable costs.

[1]: https://www.offlineimap.org
