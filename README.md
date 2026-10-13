# CPVC Job Pipeline

A real, working job-application pipeline built for the Cal Poly Vibe Coding Club. By the
end of this walkthrough you'll have four AI teammates finding real job listings, checking
them for ghost-listing red flags, tailoring your resume and cover letter to each one, and
(optionally) pre-filling the actual application form in your browser, stopping right before
you'd click submit.

This isn't a toy. Point it at your own job search and it goes to work on real postings with
your real information. Treat every output like a first draft from a fast, tireless intern:
useful, but yours to review before anything goes out.

## What you'll end up with

Four teammates:

- **finder** — searches real job boards and company career pages for roles matching your
  profile
- **validator** — re-checks each listing for ghost-listing signals (dead links, evergreen
  "always hiring" language, scraped/duplicate postings) and your actual fit
- **tailor** — drafts tailored resume bullets, a cover letter, and application-question
  answers, pulling only from facts you've actually written down
- **form-filler** (optional, needs Claude in Chrome) — opens the real application and fills
  it in, stopping before the Submit button

They hand off to each other as they go, not in a rigid batch, so validator starts checking
a listing the moment finder reports it instead of waiting for the full list.

## Prerequisites

- Claude Code Desktop, signed in with your CPVC seat
- Pro plan or higher (agent teams needs it, covered by your seat)
- Git
- For the form-filler step only: the Claude in Chrome browser extension, connected

## Step 1 — Get this repo

```bash
git clone <this-repo-url>
cd cpvc-job-pipeline
```

## Step 2 — Fill in your real profile

```bash
cp PROFILE_TEMPLATE.md PROFILE.md
```

Open `PROFILE.md` and fill in your real info: contact details, what you're looking for,
education, and real experience with real numbers. `PROFILE.md` is gitignored, it never gets
committed. Every agent in this pipeline reads this file and only this file for facts about
you, that's what keeps it from making things up.

Also skim `RULES.md`. It's short, and it's the whole reason this is safe to point at a real
job search: never fabricate a fact, never submit anything for you, never check a consent
box on your behalf.

## Step 3 — Open it in Claude Code Desktop

Open the folder as your project. Claude Code reads `.claude/settings.json` (agent teams is
already turned on) and `.claude/agents/` (the four teammate definitions) automatically.

Worth actually reading `.claude/agents/validator.md` before you run anything. It's a plain
markdown file with a little YAML on top, that's the entire mechanism.

## Step 4 — Run the kickoff prompt

Open `prompts/kickoff-prompt.md`, copy the prompt block, paste it into Claude Code as-is.

Watch the agent panel populate as finder, validator, and tailor spawn. Arrow keys to select
a teammate, Enter to watch its transcript live.

## Step 5 — Read your packets

Once it's done, open `runs/validated.md` to see every listing's verdict and why (SKIP always
comes with a real reason, never a silent drop), then open whatever landed in
`runs/packets/`. Each packet has resume bullets, a cover letter, and question answers ready
to review.

## Step 6 — (Optional) Pre-fill a real application

Only do this for a packet you've actually read and want to apply to. See
`prompts/form-filler-prompt.md` for the exact steps. The form-filler opens a brand-new
browser tab for each application, fills what it can, and stops before Submit. You click
submit.

## What's actually happening here

- **Connectors**: web search/fetch for finder and validator, browser control for the
  optional form-filler step.
- **Reusable roles (skills-style)**: the four agent definitions in `.claude/agents/` are
  plain markdown, you could drop them into any other project and reference them the same
  way.
- **Agent teams**: four independent Claude sessions with their own context windows,
  coordinating through messages and files on disk (`runs/found.md` →
  `runs/validated.md` → `runs/packets/`) instead of you manually copy-pasting between them.
  That file-handoff pattern is the actual lesson here: orchestration doesn't need a special
  framework, just files with a clear contract between steps.
- **The safety rail is the point**: `RULES.md` is what makes this usable on a real job
  search instead of a demo you'd never trust. Notice that the tailor agent would rather
  write `[CONFIRM: ...]` than invent a number, and the form-filler would rather stop and
  tell you something looks wrong than guess.

## Using this with Codex instead of Claude Code

Codex doesn't have Claude Code's agent-teams spawning, so you run the same four roles
sequentially in one session instead of as parallel teammates:

1. Open `.claude/agents/finder.md`, paste its instructions (everything after the `---`) into
   Codex along with "read PROFILE.md first." It writes `runs/found.md`.
2. Open `.claude/agents/validator.md`, paste its instructions in a fresh message, tell it to
   read `runs/found.md`. It writes `runs/validated.md`.
3. Same for `tailor.md`, reading `runs/validated.md`.
4. Same for `form-filler.md` when you're ready, if Codex has browser control available to
   it, if not, skip straight to reviewing `runs/packets/` by hand.

It's the exact same files and the exact same rules, just invoked one at a time instead of
run as a team. That's worth pointing out live: the orchestration pattern (files as the
handoff, not a framework) is what made this portable across tools in the first place.

## Troubleshooting

- **Teammates not appearing**: make sure the task is meaty enough to warrant a team; if
  Claude thinks it's simple it may just do the work itself. Explicitly ask it to spawn the
  team if this happens.
- **Too many permission prompts**: teammate permission requests all bubble up to the lead's
  session, approve them there.
- **A teammate stalls or errors out**: select it in the agent panel, check what happened,
  either nudge it or ask the lead to spawn a replacement.
- **Form-filler can't find Claude in Chrome tools**: the extension needs to be connected in
  this conversation first, ask in `#help`.
- **A filled-out form disappeared**: almost always a tab got reused instead of opened fresh.
  `RULES.md` and `form-filler.md` both call this out, it's a real failure mode, not a
  hypothetical.

## Going further

- Add a fifth teammate: an outreach agent that finds a real hiring-manager contact and
  drafts a connection note (draft only, you send it).
- Rerun `finder` with a different target role in `PROFILE.md` to see it build a fresh batch.
- Package this as a plugin so the rest of the club can install it in one step instead of
  cloning a repo.
