# Overnight / unattended contract

Fill this in **before** you walk away. The loop obeys it every tick.

1. **Cadence** — When does a tick fire?
   - [ ] Fixed interval: every ___
   - [ ] Self-paced (as soon as the last step finishes)

2. **Guardrails** — What can never happen, no matter what?
   -

3. **Scope** — Where is it allowed to make changes? (branch, folder, files — named explicitly)
   - Branch:
   - Paths:

4. **Action** — What does one tick actually do, and how is it verified?
   - One tick:
   - Verify with:

5. **Budget** — Ceiling (helper agents, tokens, retries)
   - Max helper agents per tick:
   - Stop retrying a step after:

6. **Stop conditions** — When to park and wait for a human
   - [ ] All boxes in `GOAL.md` checked
   - [ ] Three failed attempts at the same step
   - [ ] Needs approval (restart, secrets, judgment call):

7. **Report** — What every tick must write down
   - What it did
   - How it verified that
   - What's next
