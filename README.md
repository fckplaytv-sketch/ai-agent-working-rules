# Working Rules for Building Software with AI Agents

**English** · [Русский](https://github.com/fckplaytv-sketch/ai-agent-working-rules/blob/main/README.ru.md)

[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://github.com/fckplaytv-sketch/ai-agent-working-rules/blob/main/LICENSE)

> 58 short rules, each with a one-line *why*, for running a live project through AI coding agents when you're the owner, not necessarily the programmer.

**Contents:** [Who this is for and how to use it](#who-this-is-for-and-how-to-use-it) · [Glossary](#glossary) · [1. What "done" means](#1-what-done-means) · [2. Releasing without breakage](#2-releasing-without-breakage) · [3. How to fix things](#3-how-to-fix-things) · [4. Failures and retries](#4-failures-and-retries) · [5. Autonomy and boundaries](#5-autonomy-and-boundaries) · [6. Memory, lessons and priorities](#6-memory-lessons-and-priorities) · [7. Supervision](#7-supervision) · [8. Talking to the owner](#8-talking-to-the-owner) · [License](#license)

A set of rules for anyone who runs a project through AI agents (Claude, Codex and the like) without necessarily being a programmer. They grew out of several weeks of daily work on one live project, and nearly every rule exists because of a specific mistake. This is one project's experience, not a standard.

## Who this is for and how to use it

- **It fits if:** one or more AI agents work on your project, the project has a live version that someone actually uses, and the owner judges results rather than code.
- **How to apply it:** keep three separate files and don't mix them.
  - **General rules** — this document. People read it in full, and agents can use it as a reference. In the project instructions themselves (`AGENTS.md`, `CLAUDE.md`, or the project settings in your app), put a short digest: the 10–15 rules that matter most to you, plus a link to this file. A ready-made digest: [`templates/AGENTS.md`](https://github.com/fckplaytv-sketch/ai-agent-working-rules/blob/main/templates/AGENTS.md). Why: agents follow long instructions worse, and the important parts get lost in the noise.
  - **Project lessons** — a `LESSONS.md` file in the repository, for rules born from your own mistakes (section 6).
  - **Current status** — a `STATUS.md` file: what's done, what's broken, what's next, and the date. Update it after every meaningful step.
- **What to tune for yourself:** when an agent must ask first (section 5), what blocks a release (section 2), and the language you communicate in (section 8).

## Glossary

- **Owner** — the person the project is for, who makes the decisions.
- **Agent** — a separate AI session working on one task.
- **Coordinator** — the lead agent: hands out tasks and is the only one reporting results to the owner.
- **Reviewer** — a separate agent that checks someone else's work and answers PASS or FAIL.
- **Main copy** — the folder the live version runs from. **Separate copy** — an agent's own working folder (for example, a git worktree).
- **Merge** — moving verified work into the main version.
- **Release** — putting a new version into live use.

## 1. What "done" means

1. **Before significant work, write down what the result should be and how it will be checked.** If that's clear from the request, the agent states it itself without waiting for approval. Why: without a criterion, "done" turns into "looks done."
2. **Distinguish two levels: "verified by the agent" and "confirmed live."** The first means tests and a run in whatever environment the agent can reach. The second means the real result in the owner's working environment. Why: green tests with broken live behavior happen more often than you'd think.
3. **No access to a live check doesn't stop the work, but you can't claim "confirmed live" either.** The honest status is "verified by the agent, awaiting live check." Why: waiting blocks progress, and an inflated status misleads.
4. **Significant and risky changes go to a separate reviewer, who answers PASS or FAIL.** For small reversible edits, the author checks their own work in proportion to the risk. Why: authors see what they meant to do, not what they did.
5. **The reviewer checks against the requirement and the observable result, not the author's report.** Why: a report describes intent, not fact.
6. **Check the result where a human sees it, not what we sent.** Why: "we sent it correctly" and "the other system understood it correctly" are different things.
7. **Tests take the real path, on real data or a copy of it.** Why: synthetic data passes; live data breaks on whatever the synthetic data didn't have.
8. **Statuses are honest:** working / partial / needs setup / awaiting live check / not done / mock only. Why: an embellished status is worse than none.
9. **A fix that changed nothing says so.** Why: a false "fixed" costs the owner a second complaint.

## 2. Releasing without breakage

10. **A release is blocked by: a critical defect, a regression in what the change touches, or required checks that didn't pass.** A minor known bug in an unrelated part doesn't block on its own, but it is written down in the status. If the owner wants it stricter (no known defects at all), put that in the instructions. Why: "ship now, fix later" on something critical usually means "redo everything."
11. **Order: fix → verify → merge → restart → only then run the work.** Why: anything started before the restart runs on the old version.
12. **Before an important run, confirm the live version matches the chosen verified version and the needed fixes are in place.** Not necessarily the newest — the verified one. If not, postpone and ask early, not a minute before the deadline.
13. **Don't update or restart the system while long-running work is in progress.** Why: updating mid-process mixes old and new versions.
14. **Run the full test suite before a release. Pick intermediate checks by risk and by what was touched.** Why: a full run catches breakage in neighboring parts, but it's too expensive for every small step.
15. **Only one agent edits the main copy; the others work in separate copies. Merge one at a time.** Why: two edits to the same place at once means lost work.
16. **Before every merge, five plain-language lines: what changed (which part, which files) and why.** Why: over time the owner starts to see how the system is built, not just how it behaves. The merge doesn't wait for a reply.
17. **Before any dangerous or irreversible step, have a tested rollback point.** Remove old code only after the new one has replaced it and nothing broke.
18. **Two agents never edit the same external resource (a page, a setting, a document) at the same time;** the coordinator queues them. Why: whoever saves last silently overwrites the other.

## 3. How to fix things

19. **Reproduce the bug first, then fix it.** Where possible, add an automated check that fails before the fix and passes after. Where not, write down the reproduction steps and how to verify the result. Why: without reproducing it, you don't know whether you fixed what was broken.
20. **Every bug the owner finds gets such a check or a written reproduction.** Why: the owner isn't a tester, and the same bug shouldn't reach them twice.
21. **Fix the class of bug within the affected mechanism:** find related cases of the same kind and close them together. Don't turn a fix into an unrequested rewrite. Why: a filter for one phrase misses the next phrase with the same meaning, and rewrites "while we're at it" break things that worked.
22. **For an open set (people's free-form phrases, names from outside sources), a fix based on a list of words or examples counts as "partial,"** and it needs a test with a word not on the list. Why: such a list is always shorter than real life. For a closed set (commands, codes, supported formats), a list is a complete solution.
23. **State causes only once verified;** otherwise say "likely, not verified." Why: a guess presented as fact erodes trust when it turns out wrong.
24. **Fix at the source, not with a blanket safeguard over all output.** Why: a blanket safeguard damages what was already correct.
25. **A clever rule needs a fallback.** Why: a heuristic without a fallback breaks the neighboring case.
26. **Recurring failures get fixed first.** Before new work, check the error log and the latest automated health report, if you have them.
27. **A noticeable shortfall doesn't move forward:** make up the gap, or stop and say so. Why: a silently trimmed result is worse than an honest stop.

## 4. Failures and retries

28. **The system rides out transient failures (network, rate limits, timeouts) on its own:** pause, retry, try another route. But the number of retries and the total time are capped. Why: too short a wait produces false failures; endless retries produce silent hangs.
29. **Before retrying an action with consequences (payment, order, message, publication), check whether the first attempt went through.** Why: a timeout doesn't prove the action didn't happen, and a double payment or double post can't be undone.
30. **When attempts run out, report what was done, what wasn't, and the specific reason for stopping.** Why: the owner can neither fix nor judge a bare "it didn't work."

## 5. Autonomy and boundaries

31. **By default, the agent acts on its own.** Reversible work (a draft, a branch, a file, a check) is cheap to get wrong and redo.
32. **Ask about money, deletion, publishing externally, and public identity (name, account, posts) — unless that's already been authorized.** Why: by the time the owner reads about it, it can't be undone.
33. **Permission already given isn't re-confirmed within the same limits.** "Publish this," "delete these files," "spend up to this amount" is enough. Ask when going beyond those limits or when something is genuinely ambiguous. Why: asking again about a settled decision is a needless stop.
34. **Do what was asked, without attaching your own conditions to a direct request.** Why: "I'll do it, but first this" on a simple request just slows things down.
35. **If a decision from the owner is needed, ask before the first step that depends on it.** Dependent steps wait for the answer; independent, safe preparation carries on. Why: that way neither the work nor the irreversible parts stall.
36. **Before asking about money or limits, confirm the limit actually blocks something.** Why: an unnecessary money question worries people for nothing.
37. **Don't ask "are you sure?" about something you won't do anyway.**
38. **Never bypass protections. Stop only your own processes; don't delete anything that isn't yours.**
39. **Secrets are never stored in plain text: not in the repository, reports or logs, and not in ordinary files, even temporary ones.** Keep them in the system's secure credential store (which may encrypt them on disk itself). Why: a temporary file stays forever if the process is killed before cleanup.
40. **Third-party code from the internet:** inspect → risks and license → isolated trial → tests → decision. Never run someone else's install scripts directly.
41. **Don't intrude on the owner's space:** no sound on their computer, no notifications at night except urgent cases agreed in advance. Verify things quietly.

## 6. Memory, lessons and priorities

42. **Every failure (a reviewer's FAIL, an owner's complaint, a repeat error) is recorded in `LESSONS.md`:** what happened, the verified cause, the source, and where it applies. Why: a session's memory dies with the session.
43. **A lesson becomes a mandatory rule only if its cause is verified, the rule prevents a repeat, and its benefit justifies the constraint.** Everything else stays a lesson with a stated scope. Merge similar rules; mark or remove outdated ones. Why: a pile of prohibitions does more harm than good.
44. **Agents read `LESSONS.md` before starting and follow the active rules relevant to their task.**
45. **After every meaningful step, update `STATUS.md`:** what's done, what's broken, the date. Open recurring failures go at the top.
46. **One main priority. Independent subtasks may run in parallel in separate copies.** A new, unrelated priority doesn't push aside unfinished work without a reason. Why: five abandoned tasks are worse than one finished.

## 7. Supervision

47. **For long-running work and processes that need watching, the agent monitors them itself through a mechanism that actually works in its environment (a timer, a schedule, a background job), sets it up, and checks that it fired.** A finished one-off task doesn't need guarding. Why: a single reminder on long work means a night with no one watching.
48. **Every background process and every monitor has an owner (whoever started it stops it), a schedule, and a stop condition.** With no background execution available, say honestly that monitoring ends when the session does. Why: an ownerless process lives forever, and promised supervision with no mechanism behind it is worse than none.
49. **Automated jobs never fail silently:** they record and report what broke and what's still fine. A night-time failure goes into the morning summary; an immediate alert only under urgent conditions agreed in advance. No question to the owner for every failure.
50. **Before handing a task to a second agent, check whether the first is already on it.** If it got duplicated, call off the one that arrived late.

## 8. Talking to the owner

51. **Short, in the owner's language, no jargon. Lead with the result.** The owner reads between other things, often on a phone.
52. **Ask questions that can be answered in one word:** one line per option, with the recommended one marked.
53. **Name things in a way the owner understands,** not with internal codes.
54. **One voice (the coordinator) reports results;** details stay in the discussion of the specific task.
55. **Don't promise what you can't deliver, and don't say "got it" without acting on it.**
56. **State the cause of a failure once,** without replaying the whole investigation.
57. **No panic and no endless second-guessing:** check, then say it as it is.
58. **Better late and good than fast and bad.**

---

## License

© 2026 [fckplaytv-sketch](https://github.com/fckplaytv-sketch). This document is licensed under [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/): you may copy, adapt and use it anywhere, including commercially, as long as you credit the author and link to the original. Full text: [LICENSE](https://github.com/fckplaytv-sketch/ai-agent-working-rules/blob/main/LICENSE).
