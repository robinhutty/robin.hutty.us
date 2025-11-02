---
title: About
date: 2025-01-25
type: about
---

## About Me

In my professional life I wear/have worn many "hats" (Engineer/Developer, Architect, Consultant/SME) and work in various different subdisciplines, of which the most common are:

* Platform/Infrastructure Engineering
* CI/CD Pipeline Engineering
* Security Engineering
* Test Automation
* Metrics/Monitoring/Alerting/Observability
* (Systems) Architecture

But I've done and am interested in many areas of tech work that do not fit easily in these sorts of "Role" categories; the most accurate description I can give is necessarily rather imprecise:
* My "customers" are typically (internal) Software Engineering teams.
* I find ways to use my experience/knowledge/skills to help them build/run the software that they build.

Now I admit that's a bit vague, but I hope it gives some useful context to those who are techie (or techie-adjacent) enough to be interested in reading the sort of material that I'm likely to publish here.

## About this site's content

My main[^extra] purpose for creating this content is "Writing to Learn"; writing helps me to think [more] clearly, to better understand the material and my own opinions/thought process. Therefore, the principal target audiences are: _me_ and _future me_. Of course, I'd like to think that others might get benefit from this effort as well. But I try to write for my audience.

[^extra]: Additional benefits (to me) include: a single process for publishing my writing and a canonical destination/location so I can more easily rediscover information/thoughts I've sort of forgotten about. So "oh I know I have written about _foo_ before, where did I put that?" has exactly one answer.

## Other notes

1. In most of the (tech) material here, I generally presume:

    - Some exposure to networking protocols and concepts, especially HTTP and DNS
    - Some exposure to linux/unix-like systems
    - Basic terminal/cli skills
    - Basic text editor skills
    - Basic familiarity with Version Control Systems(VCS) especially `git` and "code forge" software such as github, gitlab, forgejo, etc.

2. When you see capitalized forms such as "SHOULD", "SHOULD NOT", etc., I'm using the approach of [RFC2219][1] which establishes language conventions to indicate levels of "required-ness".

[1]: https://datatracker.ietf.org/doc/html/rfc2119

3. In sections that represent cli input/output or code snippets:

    - The `<carets>` indicate tokens to be substituted by actual values.
    - The `[brackets]` indicate optional parameters.

## Other Resources I like

I mention some as options to increase background/context, some as reference/tutorial.

* [Beej's Guide to Networking Concepts](https://beej.us/guide/bgnet0/)[^bgnet]
* [Mozilla's overview of HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview)

<!-- TODO: list some intro to linux/cli tutorials on the web -->

[^bgnet]: Not to be confused with the venerable but also still awesome [Beej's Guide to Network Programming](https://beej.us/guide/bgnet/), which aims to be a practical tutorial for programming using (TCP/IP) network sockets in C.

## About this site

Here is a list of some software, written by not-me, that I've used to make/publish this website:

* [Hugo, static site generator](https://gohugo.io/)
* [Hextra(Hugo Theme)](https://imfing.github.io/hextra)
* [GitHub Pages](https://docs.github.com/en/pages)
* [GitHub Actions](https://docs.github.com/en/actions)

More details available at [How this Site was Made](../articles/this-site)

### Appearance

As someone aware of my weak understanding of how to design beautifully (despite the common "I know it when I see it"[^stewart]), I hereby offer apology if this site does not Look Good; but only mildly because it Looks Good Enough for me.

My fontstack choices are simple:

* Serif: [Vollkorn][V], [Linux Libertine][LL]. While both are free/libre, using the [SIL Open Font License][OFL], I generally prefer the feel of Vollkorn, but Linux Libertine is more comprehensive as well as more prevalent. The CSS falls back to other serif typefaces.
* Sans-Serif:
* Header/Decorative: TBD?

[V]: http://vollkorn-typeface.com
[LL]: https://github.com/libertine-fonts/libertine
[OFL]: https://openfontlicense.org

[^font]: Typeface (~= "Font Family"): "A set of typographic/design features creating a unified style for text". Font: "a particular variant of a typeface - typically specifying size, weight, but also other variations such as italic, condensed, etc.".


[^stewart]: As United States Supreme Court Justice Potter Stewart describing his threshold test for obscenity: https://en.wikipedia.org/wiki/I_know_it_when_I_see_it
