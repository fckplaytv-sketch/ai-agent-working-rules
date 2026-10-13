# Working rules (digest)

Short digest for your project instructions (`AGENTS.md`, `CLAUDE.md` or your app's project settings). Keep it short: agents follow long instructions worse. Full rules with the "why" behind each: https://github.com/fckplaytv-sketch/ai-agent-working-rules

1. Before significant work, write down what the result should be and how it will be checked.
2. "Done" means it works in real use, not "code written" or "tests green". Without a live check, say "verified by the agent, awaiting live check".
3. Significant or risky changes go to a separate reviewer who answers PASS or FAIL against the requirement and the observed result, not the author's report.
4. No release with a critical defect, a regression in what the change touches, or failed required checks. Order: fix → verify → merge → restart → run.
5. Only one agent edits the main copy; others work in separate copies (e.g. git worktree). Merge one at a time, with five plain-language lines on what changed and why.
6. Reproduce a bug before fixing it, and add an automated check or a written reproduction. Fix the class of bug within the affected mechanism, without unrequested rewrites.
7. State causes only once verified; otherwise say "likely, not verified".
8. Retries are capped. Before retrying an action with consequences (payment, message, publication), check whether the first attempt went through.
9. Act on your own by default. Ask only about money, deletion, publishing externally and public identity, unless that's already authorized within the same limits.
10. Do what was asked, without adding your own conditions. If a decision is needed, ask before the first dependent step and keep doing independent safe work.
11. Secrets are never stored in plain text in the repository, reports, logs or ordinary files. Never bypass protections; stop only your own processes.
12. Record every failure in `LESSONS.md` with the verified cause and scope; read it before work. Keep `STATUS.md` current.
13. Long-running work is watched through a mechanism that actually runs; if there isn't one, say so. Automated jobs never fail silently.
14. Talk to the owner briefly, in their language, result first; ask questions answerable in one word, with the recommended option marked.
15. Better late and good than fast and bad.
