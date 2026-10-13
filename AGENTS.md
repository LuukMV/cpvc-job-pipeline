# AGENTS.md — Codex entry point

This repo is a 4-stage job pipeline, written as Claude Code subagents in `.claude/agents/`.
Codex doesn't have Claude Code's agent-teams spawning, so you run the same four roles
yourself, in sequence, in this one session, instead of spawning teammates.

**Read `RULES.md` in full before doing anything.** Those rules apply to every stage below,
no exceptions: never fabricate a fact not in `PROFILE.md`, never submit anything, never
check a consent box, one fresh browser tab per application.

## Core pipeline (run this whenever asked to find/tailor jobs)

1. Confirm `PROFILE.md` exists (the user copies it from `PROFILE_TEMPLATE.md` and fills in
   their real info). If it doesn't exist, stop and tell them to create it first, don't guess
   at their profile.
2. Read `.claude/agents/finder.md`. Adopt that role completely for this phase and do exactly
   what it says, ending with `runs/found.md` written.
3. Read `.claude/agents/validator.md`. Adopt that role, reading `runs/found.md`, writing
   `runs/validated.md` with a verdict and reason for every listing.
4. Read `.claude/agents/tailor.md`. Adopt that role for every GO verdict, writing
   `runs/packets/<company>-<role-slug>/packet.md` for each.
5. Stop there and report back: how many found, how many GO, where the packets landed. Do
   not run the form-filler step automatically, see below.

Treat `runs/found.md`, `runs/validated.md`, and `runs/packets/` as the real handoff between
phases, re-read them rather than relying on your own memory from a few steps back. That's
intentional, it's the same file-based handoff the Claude Code version uses between its
parallel teammates, just run one phase at a time here instead of concurrently.

## Form-filler step (separate, explicit, never automatic)

Only run this when the user names a specific packet and explicitly asks for it to be
pre-filled. This is the only stage that touches a real website with their real information.

1. Check you actually have browser-control tooling available in this session. If you don't,
   say so plainly and stop. Don't attempt it with plain HTTP requests, that doesn't fill out
   a real form.
2. If you do, read `.claude/agents/form-filler.md` and the browser-automation section of
   `RULES.md`. Adopt that role for exactly the one packet named, open the listing's URL in a
   brand-new tab (never reuse a tab from a previous application), fill what you can from the
   packet and `PROFILE.md`, and stop before Submit.
3. Report exactly what's filled, what's left blank and why, and that the tab is open and
   ready for the user to review and submit themselves.

## Quick start

See `prompts/codex-kickoff-prompt.md` for the copy-paste version of the core pipeline
prompt, if you'd rather paste it explicitly than rely on this file loading automatically.
