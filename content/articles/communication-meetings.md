---
title: "Team Communication: Meetings"
date: 2025-09-22
modified: 2025-09-22T14:02:08
categories: engineering
tags: communication, meetings
type: docs
draft: true
---

For our environment/team, I see up to five possible kinds of purpose for team meetings:

1.	Making decision(s) as a group: This kind of meeting has a mandatory deliverable – either an update to the ticket(s) that specifies the work to be done or an ADR[^ADR].
2.	Brainstorming: identifying problems, ideating solutions, strategizing, etc. The deliverable should be "meeting minutes" – a summary of what was discussed.
3.	Expand and clarify knowledge/understanding. This kind of meeting should also have a deliverable – that the relevant document is created/updated so that the next time the topic needs to be referenced (or to be understood by someone new), there is less (no?) need for another meeting. Typically, the responsibility for the deliverable should lie with the person who learned something, and they should be assisted by the person(s) who explained.
4.	Standup/status: For us, usually, there is not much benefit to having this as a meeting instead of merely posting into a Teams channel. The obvious counterexample is: if/when someone has a situation in which they are blocked or struggling – but usually that situation would be better served by a request to one other team member not by bringing in the whole team. And since our timezone situation means that synchronous meeting time is at such a premium, I recommend that this type of meeting should not be scheduled too frequently. If all of us feel empowered to ask for the three kinds of meetings listed above, we shouldn't need frequent standup-style meetings.
5.	Incident management: synchronous effort to work together on remediating an acute problem. This kind of meeting is a bit different from the rest because it will occur on-demand, not scheduled.

Non-goals for meetings:
* a meeting should not primarily be about spreading knowledge; mostly knowledge should be spread in written form – either an update to the description of a ticket, a comment on a ticket/MR or an update to our written documentation.
* if a meeting's value can be achieved in writing only, then we should not have the meeting.


[^ADR]: See [Architecture/Any Decision Record](../architecture-decision-record.md)
