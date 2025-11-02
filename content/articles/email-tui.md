---
title: "Email: TUI"
date: 2025-09-27
modified: 2025-10-26T23:03:04
categories: productivity
tags: email, tui
type: docs
draft: true
---

# One approach for How to Email

This approach downloads an (offline) copy of the entire IMAP mailbox[^not-backup].

* [offlineimap][7] can download email (via IMAP from the server) to a maildir archive
* which can then be indexed by [notmuch][6]
* which makes it available to various TUI/CLI MUAs such as:

* [meli][5]
* [aerc][1]
* [himalaya][2]/[himalaya-vim][3]
* [alot][4]
* [notmuch-vim][8]

Without comprehensively evaluating[^jwz] the vastness of the field, I'm looking into the following modern software for my email TUIs[^tb].

## meli

If the above notmuch-based usage works out well, perhaps I want to use [meli][5] as a "first-class" MUA? If nothing else because it seems that JMAP (as offered/promoted by my mail provider, Fastmail) is supported out of the box with a configuration stanza in `~/.config/meli/config.toml` something like:

```toml
[accounts."fastmail-jmap"]
root_mailbox = "INBOX"
format = "jmap"
server_url="https://api.fastmail.com/jmap/session"
server_username="user@fastmail.com"
server_password="xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
use_token=true
identity = "My Name <user@fastmail.com>"
send_mail = "server_submission"
```

[1]: https://aerc-mail.org/
[2]: https://github.com/pimalaya/himalaya
[3]: https://github.com/pimalaya/himalaya-vim
[4]: https://github.com/pazz/alot
[5]: https://meli-email.org/
[6]: https://notmuchmail.org/
[7]: https://www.offlineimap.org/
[8]: https://github.com/felipec/notmuch-vim

[^jwz]: See [Zawinski's Law](https://www.laws-of-software.com/laws/zawinski/): "Every program attempts to expand until it can read mail."
[^tb]: I also use [Mozilla Thunderbird](https://www.thunderbird.net) and MS Outlook when necessary.
[^not-backup]: This is _not_ a backup solution for my mailbox; see https://www.offlineimap.org/documentation.html#make-backups-of-a-mailbox

## See Also

* https://entropicthoughts.com/current-email-solution-gpg-agent-offlineimap-notmuch-alot-msmtp
* https://wilw.dev/notes/aerc
* https://bence.ferdinandy.com/2023/07/20/email-in-the-terminal-a-complete-guide-to-the-unix-way-of-email/
