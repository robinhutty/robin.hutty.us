---
title: How this Site was Made
type: docs
prev: /articles
next: articles/first-page
sidebar:
  open: true
---

This file documents exactly how I made the repository/website that is the purpose of this repository/project.

For the impatient if you already know how this works (yes, I am talking to future-me here), you can just edit the following and execute it. You should end up with a new GitHub Project/repository for static website and a development environment that is configured with software to develop a website for root of the domain specified.

This assumes you have:
* a github.com account and your shell's ssh-agent has the private key for the public key that you will use to access that account
* the [github cli](https://cli.github.com/) installed and configured (i.e. such that `gh auth status` gives a useful response)

## Create the Repository

```shell {linenos=table,linenostart=1}
export DEST_DOMAIN=<YOUR_DOMAIN_HERE>
brew install mise
cd ${DEST_DOMAIN}
mise use hugo[@<some version>]

gh repo create ${DEST_DOMAIN} \
  --public \
  --template imfing/hextra-starter-template \
  --description "Sources for ${DEST_DOMAIN}"
```

## Create the website



## Build/Serve locally

```shell
hugo server --buildDrafts --disableFastRender --bind 0.0.0.0 --port 8088
```

## Publishing

* [GitLab Pages](https://gohugo.io/hosting-and-deployment/hosting-on-gitlab/)
* [GitHub Pages](https://gohugo.io/hosting-and-deployment/hosting-on-github/)
* [Cloudflare Pages](https://developers.cloudflare.com/pages/framework-guides/deploy-a-hugo-site/)

## Next?

* Web Analytics via [Plausible Analytics](plausible.io), a simple, open-source, lightweight and privacy-friendly web analytics, using the [plausible-hugo](https://github.com/divinerites/plausible-hugo) integration
* Fediverse publication?


## Testing

### Pre-commit

#### Linting

(Code) syntax checker, grammar/style checker, etc.

#### Link checking

* Check for broken (internal) links with [hyperlink](https://github.com/untitaker/hyperlink).
* Consider `--check-anchors` and `--sources`.
* [dump a list of external hyperlinks](https://github.com/untitaker/hyperlink?tab=readme-ov-file#external-links).
