---
name: commit
description: Stage and commit the current changes in the catalog with messages following the commit standard in AGENTS.md, after showing an overview and getting the user's confirmation. Use when the user asks to commit or for a commit message ("committe das", "commit msg", "commit the changes").
---

# Commit

Commit the working tree changes, but only after the user has confirmed an
overview. Never commit without that confirmation, and never push.

1. Read the "Commits" section of `AGENTS.md`. It decides the message, the
   area, the verb and when changes belong in separate commits.
2. Collect the changes: `git status --short` and `git diff --stat HEAD`,
   plus untracked files. Read the diffs only as far as needed for the
   message. If nothing changed, say so and stop.
3. Leave out files that must never be committed and name them in the
   overview: anything in `ranges/`, local settings, and anything that
   looks like a secret or a local artifact.
4. If the user only asked for a message, answer with the line(s) in a code
   block and stop here.
5. Show the overview in this form and nothing else:

   ```
   ## Commit overview
   Branch: <branch>

   ### Commit 1: `orks: add Beast Snagga Boyz`
   | Status | File | Lines |
   |--------|------|-------|
   | A      | kits/games-workshop/orks/beast-snagga-boyz.yaml | +24 |
   | M      | factions/warhammer-40000.yaml                   | +3 −0 |

   ### Commit 2: ...

   Not committed: <files and why>, or "none"
   ```

   Status is `A` (new), `M` (changed), `D` (deleted) or `R` (renamed);
   untracked files count as `A`. If something was already staged, say so
   below the branch line, since it ends up in the commit as well. A
   released kit file that was renamed, moved or deleted breaks the README;
   point it out above the commits.
6. Ask with the `AskUserQuestion` tool, options: "Commit as shown",
   "Change message or split", "Cancel". The question lists only the
   commits, one per line with number and message. Then wait.
   - "Change message or split": apply the user's change, show the updated
     overview and ask again.
   - "Cancel": stage nothing and stop.
7. After confirmation, for each commit in the order shown: stage exactly
   its files by path (`git add -- <paths>`, `git rm` for deletions; never
   `git add -A` or `git add .`), then `git commit -m "<message>"` with the
   confirmed line only.
8. If a git hook or the commit fails, stop, report the output and don't
   retry with `--no-verify` or similar. Leave what was already committed.
9. Report the result: `git log --oneline -<number of commits>`.

Never push, amend, rebase or change the git config.
