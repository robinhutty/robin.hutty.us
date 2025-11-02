Outage Retrospective - draft
Issue summary
Leadup
Detection
Timeline
Root cause identification: The Five Whys
EXAMPLE:
Root cause
EXAMPLE:
Lessons learned
Corrective actions
Issue summary
There was an outage in the NGRX SalesLab environment, related to Archer Engineering changes, that required several engineers to resolve.

Leadup
Describe the sequence of events that led to the incident, for example, previous changes that introduced bugs that had not yet been detected.

<Add events before>

Incident August 1st - SalesLab unplanned outage

Detection
When did the team detect the issue? How did they know it was happening? How could we improve time-to-detection? Consider: How would we have cut that time by half?

Timeline


<timeline start>

<events>

August 1 - NGRX SalesLab unplanned outage

<events>

<timeline end>

Root cause identification: The Five Whys
The Five Whys is a root cause identification technique. Here’s how you can use it:

Begin with a description of the impact and ask why it occurred.

Note the impact that it had.

Ask why this happened, and why it had the resulting impact.

Then, continue asking “why” until you arrive at a root cause.

List the "whys" in your postmortem documentation.

EXAMPLE:
The application had an outage because the database was locked

The database locked because there were too many writes to the database

Because we pushed a change to the service and didn’t expect the elevated writes

Because we don't have a development process established for load testing changes

Because we never felt load testing was necessary until we reached this level of scale.

Root cause
Note the final root cause of the incident, the thing identified that needs to change in order to prevent this class of incident from happening again.

EXAMPLE:
A bug in connection pool handling led to leaked connections under failure conditions, combined with lack of visibility into connection state.

Lessons learned
Discuss what went well in the issue response, what could have been improved, and where there are opportunities for improvement.

Corrective actions
Describe the corrective action ordered to prevent this class of issue in the future. Note who is responsible and when they have to complete the work and where that work is being tracked.





Source: Incident Postmortem Templates: Improve Response Process | Atlassian
