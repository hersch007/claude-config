Push all local changes to GitHub for both the Claude Projects folder and the Claude config folder.

1. Stage and commit claude-projects:
   - `git -C "C:\Users\richa\Documents\Claude Projects" add .`
   - `git -C "C:\Users\richa\Documents\Claude Projects" status --short` — show the user what's being committed
   - `git -C "C:\Users\richa\Documents\Claude Projects" commit -m "sync: $(Get-Date -Format 'yyyy-MM-dd HH:mm')"` (skip if nothing to commit)
   - `git -C "C:\Users\richa\Documents\Claude Projects" push`

2. Stage and commit claude-config:
   - `git -C "C:\Users\richa\.claude" add .`
   - `git -C "C:\Users\richa\.claude" status --short` — show the user what's being committed
   - `git -C "C:\Users\richa\.claude" commit -m "sync: $(Get-Date -Format 'yyyy-MM-dd HH:mm')"` (skip if nothing to commit)
   - `git -C "C:\Users\richa\.claude" push`

Report what was pushed from each repo, or confirm if everything was already up to date.
