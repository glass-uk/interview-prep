# Example — Behavioral Session

This example shows the expected flow for a behavioral interview session, including STAR enforcement.

---

**Interviewer:** Tell me about a time you had to push back on a deadline or scope from a stakeholder.

**User:** In my last role we were asked to deliver a new tracking API in two weeks. I knew the schema design needed more time so I pushed back by showing the PM a risk matrix of what would break if we rushed it.

---

**Score: 6/10**

**What you did well:**
* Clear situation — real project with real timeline pressure
* Identified a specific technical risk (schema design) rather than a generic "we need more time"
* Used data (risk matrix) to make your case, not just opinion — this is a strong senior signal

**What was missing or could be stronger:**
* No **Task** — what was your specific responsibility vs the rest of the team? Were you the lead? The only engineer?
* No **Result** — did the stakeholder agree? Was the deadline extended? What was the measurable outcome?
* "Pushed back" is vague — what specifically did you propose as an alternative? A phased delivery? A revised timeline?

**Model answer:**
In my previous role we were asked to deliver a new customer-facing tracking API in two weeks. The requirement came from the product team responding to an escalation from a key client, so there was real stakeholder pressure. I was the lead engineer on the API team, which meant the delivery decision and the technical trade-offs were mine to own.

I identified early that the two-week timeline would force us to ship with a schema that didn't account for multi-carrier events, which would require a breaking migration within three months and carry significant delivery risk. Rather than just saying "we need more time", I prepared a one-page risk comparison: option one delivered in two weeks with an estimated four engineer-days of migration cost in Q2 and a medium probability of a breaking change affecting clients; option two took three weeks and delivered a schema that would last at minimum 12 months.

I presented this to the PM and the CTO in a 20-minute alignment meeting. They chose the three-week option after I quantified the downstream cost. We delivered on day 19. That schema has not required a breaking change in over a year. The bigger outcome was that it established a pattern on the team: we now always include a "migration cost" estimate when scoping API work, which has prevented two similar situations since.

---

**Saved to:** `~/.claude/skills/interview-prep/notes/behavioral/Behavioural - Pushing Back on Deadlines.md`
