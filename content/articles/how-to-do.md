---
title: 'How To Do, Knowing What We Want to Achieve, and "Definition of Done"'
date: 2025-09-08
modified: 2025-09-26T14:24:21
categories: engineering
tags: specification, planning
type: docs
draft: true
---

(each layer could also discuss the decision making process, e.g. alternatives that were considered)

Problem statement, goals, non-goals, identifies stakeholders (roles and individuals/teams), and requirements, both functional and non-functional (TODO: define)

## Goals

Goals
: What are we trying to achieve? How does this effort expect to ameliorate the Problem?

## Non-Goals

Non-Goals
: What are we excluding from the scope of this effort?

## Functional Requirements

Functional Requirements
:

### Examples


## Non-Functional Requirements

Non-Functional Requirements
:

### Examples

----

The next layer is the functional _specification_, which describes precisely how the system will work from an external perspective.

The third and final layer is the technical specification, which describes the internals. This layer (especially for Infra/Platform projects?) is often very dynamic because ...; which makes it even more important to keep the record of it close to the code (as opposed to in email/wiki/random docx).

The whole becomes the Definition of Done, subject to any requirements marked "optional".

Not all projects need all components, this article presents a framework to be used as a guide and implemented _with sound judgement_. Hopefully it will provide some shared context, jargon, and expectations to help my team projects work better.

----

## How to Approach "Doing" (vs. "Deciding/Planning")

> If I had six hours to chop down a tree, I would spend the first four sharpening the axe.
Abraham Lincoln

(only relevant once you've decided which tree to chop!)

TODO: Discuss "Make it work, make it correct, make it good, make it fast, make it cheap – mostly in that order"

TODO: Discuss "Risk Analysis/Management": Risk = the probability of [a] problem * the cost of that problem. (Low _p_, High _c_ => fire extinguishers, etc.)

TODO: Discuss Sequencing. What should we work on and in what order? Necessary pre-requisites vs. 'better (=='faster'?) if we do X before Y'

TODO: Consider "Leverage Points", Theory/Dynamics of Complex Systems, Donella Meadows, etc.
  - https://systemsthinkingalliance.org/transforming-systems-with-leverage-points-insights-and-critiques-and-future-directions/, https://donellameadows.org/archives/leverage-points-places-to-intervene-in-a-system/
