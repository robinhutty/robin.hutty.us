---
title: How this Site was Made
type: docs
draft: false
prev: /articles
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

There are several options to create basic scaffolding for a Hugo website, such as cloning or otherwise copy from a Hugo Theme repository, such as the template repository of the [Hextra Theme]() or using the Hugo CLI.

### Hugo CLI

```shell {linenos=table,linenostart=1}
hugo new site SITENAME --format=yaml
cd SITENAME
hugo mod init github.com/USERNAME/SITENAME
```

#### Add a theme



* For Beautiful Hugo: use `halogenica/beautifulhugo` as the repository
* For Hextra: use `imfing/hextra` as the repository

```shell
hugo mod get github.com/imfing/hextra
```


```yaml {filename="hugo.yaml",linenos=table,linenostart=1}
module:
  imports:
    - path: github.com/imfing/hextra
```

### Writing New Posts

To create a new blog post:

1. Create a new file in the `blog` directory
2. Name it using the format: `YYYY-MM-DD-title.md`
3. Add front matter at the top:

```yaml
---
# type:
title: "Your Post Title"
date: YYYY-MM-DD
type: blog
draft: false
---
```

### Writing Pages

For non-blog content, create a new file in the `articles` directory:

```yaml
---
title: "About"
type: docs
# prev:
# next:
sidebar:
  open: true
---
```

### Markdown Tips

This site supports CommonMark as a "standard" Markdown:

- **Bold** text uses `**asterisks**`
- *Italic* text uses `*asterisks*`
- Links use `[text](url)`
- Images use `![alt text](image-url)`
- See some [markdown docs](https://commonmark.org/help/) for more help
- Footnotes
  ```markdown
  Here is some text with a footnote.[^1]

  [^1]: And here's the footnote text.
  ```

#### Deeper details

By default, and for this site, Hugo uses [Goldmark](https://github.com/yuin/goldmark/) to render Markdown. This means:

* I can use anything in the [CommonMark](https://commonmark.org/) Spec.
* Various [built-in extensions](https://github.com/yuin/goldmark/?tab=readme-ov-file#built-in-extensions) are available by default. As you can see across this site, I use the extensions 'Definition Lists' and 'Footnotes' extensively.
* Various [optional extensions](https://github.com/yuin/goldmark/?tab=readme-ov-file#list-of-extensions) can easily be made available.

### Hugo Tips

* Learn more about [draft, future, and expired content](https://gohugo.io/getting-started/usage/#draft-future-and-expired-content)

## Build/Serve locally

```shell
hugo server --buildDrafts --disableFastRender --bind 0.0.0.0 --port 8088
```

## Publish/Deploy

Given that Hugo is a Static Site Generator, it outputs plain text files (HTML, CSS, JavaScript) that can be served by any webserver. So there are many simple publishing/deployment options.

* [GitLab Pages](https://gohugo.io/hosting-and-deployment/hosting-on-gitlab/)
* [GitHub Pages](https://gohugo.io/hosting-and-deployment/hosting-on-github/)
* [Cloudflare Pages](https://developers.cloudflare.com/pages/framework-guides/deploy-a-hugo-site/)
* or your own tiny server such as a Raspberry Pi, smartphone or other low power computing device.

## Next?

* Additional Goldmark extensions
  - https://github.com/abhinav/goldmark-frontmatter
  * diagrams (via [D2](https://github.com/FurqanSoftware/goldmark-d2) or [Mermaid](https://github.com/abhinav/goldmark-mermaid))
  - [Table of Contents](https://github.com/abhinav/goldmark-toc)
* Web Analytics via [Plausible Analytics](plausible.io), a simple, open-source, lightweight and privacy-friendly web analytics, using the [plausible-hugo](https://github.com/divinerites/plausible-hugo) integration
* Fediverse/IPFS/etc. publication?
* Comments? Perhaps via [giscus](giscus.app)?


## Testing

### Pre-commit

#### Linting

(Code) syntax checker, grammar/style checker (vale et al.), etc.

#### Link checking

* Check for broken (internal) links with [hyperlink](https://github.com/untitaker/hyperlink).
* Consider `--check-anchors` and `--sources`.
* [dump a list of external hyperlinks](https://github.com/untitaker/hyperlink?tab=readme-ov-file#external-links).
