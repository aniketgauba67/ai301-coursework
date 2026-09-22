# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72

**Verdict output**

Scope check: `codepath/pathreview-ai301-fa26-s3` is the scoped Path Review source per `scope.md` — candidate is in scope.

- maintainer-alive: pass — collaborator Aburke225 authored commits `2f4e82f`/`ddb377d`/`f011a77` to `main` on 2026-09-16, 6 days before capture. A human-authored default-branch commit within 30 days satisfies this check on its own.
- repo-in-use: pass — the repo is not archived, and that same push to `main` on 2026-09-16 is well within 90 days of capture.
- bounded-scope: pass — the issue names one defect (`verify_password` raising `UnknownHashError` instead of returning `False` on a malformed stored hash), the exact files to change (`core/security.py`, `tests/unit/test_security.py`), the exact xfail marker to remove (manifest id H-05), and a 1-2 hour estimate. No history of abandoned attempts.
- unclaimed: pass — no assignee, no linked or open PR. One classmate (`sseid4`) left a comment describing a plan to reproduce it; per `scope.md`'s Path Review house rule, classmates' claim comments do not block claiming.
- ai-contribution-allowed: pass — `docs/CONTRIBUTING.md` sets out a workflow (branch naming, conventional commits, green CI, the xfail-removal contract) but states no restriction on AI-generated or AI-assisted contributions; no `AGENTS.md` or `AI_POLICY.md` is present in the repo. Silence passes.

Verdict: accept

```json
{
  "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72",
  "checks": [
    {"name": "maintainer-alive", "grade": "pass", "evidence": "Collaborator Aburke225 authored commits 2f4e82f/ddb377d/f011a77 to main on 2026-09-16, 6 days before capture"},
    {"name": "repo-in-use", "grade": "pass", "evidence": "Not archived; last push to main 2026-09-16, 6 days before capture (within 90 days)"},
    {"name": "bounded-scope", "grade": "pass", "evidence": "Issue names one defect, the exact files (core/security.py, tests/unit/test_security.py), the exact xfail marker (manifest id H-05) to remove, and a 1-2 hour estimate"},
    {"name": "unclaimed", "grade": "pass", "evidence": "No assignee, no linked/open PR; one classmate claim comment does not block per scope.md's Path Review house rule"},
    {"name": "ai-contribution-allowed", "grade": "pass", "evidence": "docs/CONTRIBUTING.md states no AI-contribution restriction; no AGENTS.md/AI_POLICY.md present"}
  ],
  "verdict": "accept"
}
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

16/20 (first full run, against the original bounded-scope wording) -> 20/20 (confirming full run, after rewriting bounded-scope) -> 19/20 (final run, saved to `eval-run.txt`; direct quote from that file: "agreement: 19/20 scored items  (bar: 18/20: PASS)"). issue-20 flipped between the 20/20 and 19/20 runs — that is grading non-determinism in the model's read of that one bundle, not a rubric change; the rubric itself did not move between those two runs.

**Issue analysis**

issue-19. Gold label: accept. My rubric's verdict: accept (agree). From the bundle:

"There are two potential causes which should be fixed:
1. The matchers are slow for certain rewrites (quadratic instead of linear)
2. UI update is waiting for the matching thread to finish

Additional suggestions:
1. We should use multi-processing to use all the cores to match rewrites in parallel
2. Only match rewrites in the categories that are not collapsed. If the category is expanded, we should run matchers for those rewrites
3. Applying the rewrite should also happen in a separate thread"

Under the original, narrower bounded-scope wording this read like an umbrella issue — one symptom, two possible causes, three more suggestions on top — and graded reject. It is actually one bounded symptom ("Selecting large subgraphs in proof mode freezes the UI"), opened by a COLLABORATOR and labeled Priority: High, where the "causes" and "suggestions" are the reporter's own diagnostic notes, not a request to do all five things. The rubric's current bounded-scope wording — "...or a bug report naming one symptom even if it lists multiple possible causes or optional follow-up suggestions, as long as the core fix is a single identifiable target" — was written to fix exactly this misread, and grades issue-19 accept.

**Check rationale**

Quoted as currently written in `rubric.md`:

"bounded-scope | Issue body and comment thread | Pass if the issue describes one cohesive piece of work with a concrete, decided target outcome — this includes: a fully-specified multi-file task where the reporter already laid out exactly what changes are needed; a terse ask (even one line) if it names a specific, concrete target, especially when opened by a maintainer/collaborator or labeled "good first issue"; or a bug report naming one symptom even if it lists multiple possible causes or optional follow-up suggestions, as long as the core fix is a single identifiable target. Fail if: it is explicitly an umbrella/tracking issue meant to be split into separate issues; the thread shows an unresolved design debate with no maintainer decision; a maintainer states it touches core internals; it is a pure usage/support question ("how do I...") rather than a contribution; or the thread shows a history of multiple abandoned attempts (two or more closed, unmerged linked/mentioned PRs, or repeated claim-then-go-silent cycles) — that history means the issue is harder than it looks, whatever its apparent scope | required"

Reasoning behind the current form: the first draft only accepted a bug report if it named a single cause. That rejected several genuinely good first issues (issue-01, issue-04, issue-15, issue-19) that were bounded in practice but written by reporters who listed every plausible cause or nice-to-have alongside the real fix. The rewrite keeps "harder than it looks" as the actual thing to fail on — a documented history of abandoned attempts — instead of penalizing a thorough bug report for being thorough.

**Trade-offs**

Canary re-run with `--only issue-01,issue-04,issue-15,issue-19` after the rewrite:

"issue-01  accept  accept   yes
issue-04  accept  accept   yes
issue-15  reject  reject   yes
issue-19  accept  accept   yes

agreement: 4/4 scored items"

issue-15 is what shows the edge the looser wording still catches: it is a bug report with multiple listed causes, like issue-19, but it also carries a documented history of abandoned attempts, so it still fails bounded-scope and grades reject correctly. What the check gives up is precision on an issue that lists many causes or suggestions but has no abandoned-attempt history yet: the rubric now trusts "one named symptom" as the real target even when the write-up is long, so a newcomer could still pick something that turns out to touch several subsystems, as long as nobody has failed at it before. That is a risk I'm accepting, because the narrower wording was rejecting real good-first issues for the crime of being well-documented.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. Fit and time available: this is a backend/Python issue in `core/security.py` (password-hash verification with `passlib`) — exactly the backend/API work I said I wanted more reps in, not a front-end task. The scope is one function's error-handling behavior plus one test file, estimated at 1-2 hours by the reporter, which fits comfortably inside this unit without requiring me to first learn PathReview's RAG or agent internals.

2. What the verdict got right vs. what I weighed myself: the rubric correctly found the issue unclaimed under its own rules — no assignee, no open PR — but it has no way to weigh that a classmate (`sseid4`) already posted a detailed reproduction plan in the thread. The Path Review house rule says that doesn't block me, and I agree with treating it that way, but deciding whether to jump in alongside a classmate who is already partway through is a judgment call the rubric isn't built to make, so I made it myself.

3. Anticipated difficulty in claiming it: low-to-moderate. The fix itself is small, but the repo enforces green CI across five jobs (lint, typecheck, unit, integration, frontend) before a PR is reviewable, and first PRs from a new fork sit queued as "waiting for approval to run workflows" until a maintainer releases them — so I'm expecting the actual coding to be quick and the review/CI loop to be the slower part.

---

Related paths: `eval-run.txt` in this directory; your skill's files in `tools/issue-select/`.
