---
title: An initial setup to use local LLMs
date: 2025-07-21
modified: 2025-09-25T08:48:08
categories: productivity
tags: llm, self-hosted
type: docs
draft: true
---

While I am quite [skeptical][5] about many of the claims made by those who are pushing adoption of the [current wave][^ai-wave] of "AI" technologies, I also recognize that not only are these technologies are broadly influential but also that they can be powerful additions to the toolkit of professional techies and the general public alike, bringing real benefit.

So I decided to get some experience with a setup that can make some of this potential benefit available to me (as well as to get some experience with prompt engineering, evaluating the responses, etc.). Therefore, no matter however much I decide to incorporate LLM-based tooling into my daily professional life, I know something about what the hype is about.

The ecosystem is changing very quickly, so this article might soon be completely obsolete (and not just "behind the state of the art" as is likely at the time of writing), but it will serve me as a record of one approach.

## What do I want/not want?

I want local-only[^local], free-as-in-beer[^free], CLI/TUI that I can use to interact with LLMs. For the current scope, I will not prioritize functionality for images/vision, audio, multiple (natural) language support, although I think at least some of that will be part of the baseline.

## Tools

- [Rancher Desktop][1] - which "provides container management and Kubernetes on the desktop". For this (Ollama) purpose, I [did not need Kubernetes][7], so I disabled it[^no-k8s]. I wanted to use the docker CLI, so I set[^engine] the Container Engine to be `dockerd`/`moby` instead of `containerd`.
- [ollama][4] - provides a server to run the LLM(s). Ollama runs locally, and conversation data does not leave the machine on which it runs.
- The [Open WebUI][2] [extension][3] for Rancher - which provides integration between Rancher and Ollama.

[^no-k8s]: Under 'Preferences > Kubernetes', see https://docs.rancherdesktop.io/faq#q-i-dont-need-the-kubernetes-cluster-deployed-by-rancher-desktop-how-do-i-disable-it-to-save-resources

[^engine]: Under 'Preferences > Container Engine'. I'm not sure that I really care about using the docker CLI, but this was familiar and worked; I'll look into using the `containerd` and the `nerdctl` CLI at some point.

### Install

1. [Rancher CLI](https://github.com/rancher/cli)
1. [Rancher Desktop](https://github.com/rancher-sandbox/rancher-desktop)
1. [Ollama][7]

```console
$ brew install rancher-cli rancher-desktop ollama-app
```

```console
$ rdctl extension install ghcr.io/rancher-sandbox/rancher-desktop-rdx-open-webui:0.0.9
```


#### Update Open WebUI

New [Open WebUI Releases](https://github.com/rancher-sandbox/rancher-desktop-rdx-open-webui/releases) have to be explicitly uninstalled/installed[^rd-update], e.g.:

```console
$ rdctl extension uninstall ghcr.io/rancher-sandbox/rancher-desktop-rdx-open-webui:0.0.8
$ rdctl extension install ghcr.io/rancher-sandbox/rancher-desktop-rdx-open-webui:0.0.9
```

[^rd-update]: Unless done as part of a new release of [Rancher Desktop](https://github.com/rancher-sandbox/rancher-desktop/releases) itself.

## Models

```console
$ ollama pull "tinyllama:latest"
$ ollama pull "qwen3:8b"
```

## Usage

### CLI

```console
$ ollama run "tinyllama:latest" "tell me whether this works"
Yes, the sentence "This works" does work as it is a valid English sentence made up of words and clauses. The correct syntax would be "This works."
```

### Vim

[vim-ollama][6] is a plugin for Vim which adds "code completion support to Vim". Since it uses Ollama as a backend, it fits in nicely with the above tooling without sending your LLM conversations anywhere and can configured for entirely airgapped/offline use.

[^ai-wave]: Which I'm defining as starting with the release of [GPT-3](https://en.wikipedia.org/wiki/GPT-3) in 2020 and [ChatGPT](https://en.wikipedia.org/wiki/ChatGPT) in 2022.
[^local]: _Not_ making network requests to The Cloud or a SaaS provider; so either "strong" local (to the individual computer that I'm typing on), or a self-hosted service that I provide on my LAN/my infrastructure to my users.
[^free]: I would also like free-as-in-freedom, but that's complicated; I suspect it's more difficult or even not available.

## Next Steps

I have one other big use case I'd like to support for myself:

Open a bunch of buffers in vim, send their content to a local AI service with another buffer that is the chat session with that AI in which I can make requests like: "port this codebase to another language/framework/cloud provider"

[1]: https://docs.rancherdesktop.io
[2]: https://github.com/open-webui/open-webui
[3]: https://github.com/rancher-sandbox/rancher-desktop-rdx-open-webui
[4]: https://github.com/ollama/ollama/tree/main/docs
[5]: ../../blog/2025-07-21-ai-hype.md
[6]: https://github.com/gergap/vim-ollama
[7]: https://ollama.ai
