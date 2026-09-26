---
name: setting-up-local-repo
description: Use when someone needs this AWS League site on their own computer so they can edit it and push changes to GitHub - first-time setup on a new or borrowed laptop, "how do I work on this locally", git push rejected with 403 or "permission denied to", or git prompting for a username and password.
---

# Setting Up This Repo Locally

## Overview

Takes a non-technical person from "nothing installed" to "I can edit the site and my changes are live on GitHub."

**You run every command.** The person you are helping has no technical ability. They do exactly one thing you are incapable of doing: sign into GitHub in a browser. Everything else is yours.

- **Repo:** `https://github.com/awsbadminton-del/AWS-League-Nov-Dec-2026`
- **Owner account:** `awsbadminton-del`, which signs in **with Google**

**Done means:** repo cloned, `gh` authenticated as the owner, and a real commit pushed and confirmed present on GitHub. Not "should work" — confirmed.

## The One Handoff — read this before Step 3

Browser login is the only step you cannot perform. You must stop and wait.

**The failure this prevents:** you print the login command as though it were documentation, add helpful notes after it, and end your turn. The person reads it as background reading, never realizes they are the blocker, and the setup stalls indefinitely. This has actually happened.

**REQUIRED — end that message with this block, and nothing after it:**

```
⛔ YOUR TURN — I can't do this one, you have to.

Copy this whole line into the chat box and press Enter (keep the ! at the front):

! gh auth login --hostname github.com --git-protocol https --web

What happens next:
1. It prints a code like 74A9-0EE7. Copy that code.
2. Press Enter. A browser tab opens to github.com/login/device.
3. Paste the code.
4. Sign in with GOOGLE, as awsbadminton-del. If the page already shows a
   different account, sign out first or use a private/incognito window.

I am stopped here and waiting for you. Paste back whatever it prints.
```

Rules for the message containing that block:

- The block is the **last thing** in the message. Nothing follows it — no tips, no preview of next steps, no "meanwhile I'll...".
- Make **no tool calls** in that turn. Stopping is the entire point.
- Ask for **only this**. Do not bundle a second request into the same message.
- Never offer to run the login yourself, and never narrate interactive prompts for them to answer. The `--web` command above is deliberately non-interactive on your side. Saying you will "walk them through it live" strands them: they wait for you while you wait for them.
- Success looks like `✓ Logged in as awsbadminton-del`. Anything else, fix it and re-issue the same block unchanged.

## Steps

### 1. Check what is already installed

```bash
git --version
gh --version
gh auth status
```

Install only what is missing, then **open a new terminal** so the commands are found:

| OS | Git | GitHub CLI |
|---|---|---|
| Windows | `winget install --id Git.Git -e` | `winget install --id GitHub.cli -e` |
| macOS | `brew install git` | `brew install gh` |

### 2. Clone

```bash
git clone https://github.com/awsbadminton-del/AWS-League-Nov-Dec-2026.git .
```

Clone into an empty folder. If the folder has files, clone into a subfolder instead.

### 3. Log in as the owner — HANDOFF, see section above

`gh auth status` may already show a different account (e.g. a personal one). That account can read this repo but **cannot push**. Adding the owner account alongside it is fine; `gh` holds several accounts and the **active** one is what git uses.

### 4. Wire git to use that login

```bash
gh auth setup-git --hostname github.com
```

No more password prompts after this.

### 5. Set who the commits belong to

```bash
git config user.name "awsbadminton-del"
git config user.email "330161580+awsbadminton-del@users.noreply.github.com"
```

Repo-local on purpose — it leaves their global identity alone for other projects. Confirm the number with `gh api user --jq .id` if the account ever changes.

### 6. Verify — do not skip

```bash
gh repo view awsbadminton-del/AWS-League-Nov-Dec-2026 --json viewerPermission
git push --dry-run origin main
```

Expect `ADMIN` (or `WRITE`), and a dry-run that completes without an auth error. Dry-run contacts the remote and proves the credentials without changing anything.

Then push something real, because a dry run does not prove write access:

```bash
git commit --allow-empty -m "Verify local push setup"
git push origin main
gh api repos/awsbadminton-del/AWS-League-Nov-Dec-2026/commits/main --jq '{sha:.sha[0:7], msg:.commit.message, user:.author.login}'
```

The last command must echo back your commit and `"user":"awsbadminton-del"`. If `user` is null, Step 5's email is wrong — the commit still pushed, but GitHub won't credit the account.

## Their everyday loop

Once set up, teach them only this:

```bash
git add -A
git commit -m "what changed"
git push
```

## Common problems

| Symptom | Cause | Fix |
|---|---|---|
| `permission denied to <name>` / 403 on push | Active `gh` account isn't the owner | `gh auth switch`, confirm with `gh auth status` |
| Git asks for username/password | Credential helper missing | Re-run Step 4 |
| `gh` not recognized after installing | PATH not refreshed | Open a new terminal |
| Browser logged them into the wrong account | Existing GitHub session | Sign out, or incognito window, then redo Step 3 |
| Push rejected only for `.github/workflows/` files | Token lacks `workflow` scope | `gh auth refresh -h github.com -s workflow` (browser again — use the handoff block) |
| `Everything up-to-date` on a real push | Nothing was committed | `git status`, then `git add -A && git commit` |
| Push rejected, "fetch first" | Repo changed on github.com (web edits) | `git pull --rebase` then push |

## Talking to a non-developer

- One command at a time. Wait for the result before sending the next.
- Say what success looks like *before* they run it, so they can tell you if it differs.
- They cannot judge whether output is an error. Ask them to paste all of it.
- Never say "just" — "just run", "just click". If it were just anything they wouldn't need you.
- Report verified facts, not hopes. "Confirmed on GitHub" beats "that should be working now."

## Red flags — stop

- About to send the login command without the ⛔ block → they will not know to act.
- Adding anything after the ⛔ block → it stops reading as an instruction.
- Making a tool call in the handoff turn → you are not actually waiting.
- Writing "I'll run the login for you" or listing terminal prompts they will answer → you cannot run it, and they will never act.
- The ask is longer than a screen, or sits under a heading partway down → it reads as background material.
- Saying setup is complete without Step 6's real push → unverified.
- Assuming the logged-in account can push because it can clone → reading access is not writing access.
