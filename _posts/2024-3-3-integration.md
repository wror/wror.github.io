---
layout: post
title: Programming across Teams
---
This post describes two different approaches to managing software releases across multiple teams.

It's important to understand which teams are downstream of others,
i.e. which teams can bring their code live before the others without risk.
For example, if Team1 manages a GUI that accepts orders, and Team2 manages a backend system that executes orders,
then Team2 is downstream of Team1. It can add the feature to execute orders without breaking anything,
though the feature might not actually be used yet. Team1 cannot safely add the feature before Team2, because users will see it and try it,
and it won't actually work.

If teams agree to change an existing integration which is already being used in production,
then the downstream system should support both the old and the new integrations,
and only decommission the old integration after its upstreams have stopped using it.
This avoids coupling releases across teams, and is especially good if one of the teams needs to rollback their release.
### Frequent Integration
For frequent integration, there should be a person or process for managing the larger inter-team release.
Each sprint, they should determine which teams are participating, and schedule the participating downstream releases first.

This approach is best when most sprints involve more than one team.

### Infrequent Integration
When one team needs changes from other teams, but not frequently, extra care is required.
Often those other teams might be slow to release, or might not communicate well regarding their releases.

The simple and robust thing to do, is to be paranoid, and have the downstream system work both with and without the changes in the upstream system. Rely on automated tests to prove that the new code will continue to work against the old integration.

<details>
<summary>I previously suggested a more complicated approach, but now regret it.</summary>

<br/>
The trick here is for the motivated team to get task IDs from other teams whenever those other teams make changes for them.
For example, when the GUI Team1 wants to support a new order type, and needs the backend Team2 to support it,
they need to get an ID from Team2, say MYJIRA123.
Then, before Team1 schedules a release, they can ask Team2, "did you release MYJIRA123 yet to production?"
<br/>
<br/>
This is even more important than having an ID beforehand, for tracking when the other team will do the work.
Programmers should remember (and managers should remind them), that _any time_ they ask another team to do something,
to do anything other than "please bring up our test environment", they should get a task ID.
<br/>
<br/>
Teams should understand that it's important that these tasks be "agile",
that they represent everything that's needed for the feature by the requiring team.
If the implementing team wants to split the task up into ten subtasks, that's fine,
but it's important that the critical one have a stable identifier.
<br/>
<br/>
Note that it's not necessarily the most upstream team which needs to be most proactive here;
it's the most motivated team, the team that owns the overall feature.
That team needs to make sure that downstream teams release their changes first, and that upstream teams release their changes afterwards.
</details>
<br/>

### Regarding Testing
In both approaches, teams test all their changes together in a test environment before releasing them to production.
This doc describes how to transition the production environment to have the same configuration as that tested configuration.
Generally intermediate configuations are not manually tested in any pre-production environment.
For example, no company manually tests downstream Team2's changes in isolation, before Team1's have been made.
Instead this is handled by automated regression tests, and production checkouts after release.
