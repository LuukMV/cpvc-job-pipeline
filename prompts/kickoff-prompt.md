# Kickoff prompt

Before running this, copy `PROFILE_TEMPLATE.md` to `PROFILE.md` and fill it in with your own
real information. Every agent reads that file for facts about you.

Copy the block below into Claude Code as-is, nothing to fill in.

---

We're running a job-hunting pipeline using an agent team. Read `RULES.md` and `PROFILE.md`
first, both apply to every teammate below.

Spawn three teammates:

- **finder** (using the finder agent type) — search real job boards and company sites for
  roles matching my profile, write candidates to `runs/found.md`
- **validator** (using the validator agent type) — check each candidate for ghost-listing
  signals and fit against my profile, write verdicts to `runs/validated.md`
- **tailor** (using the tailor agent type) — for every GO verdict, draft a tailored resume
  bullet set, cover letter, and application-question answers, written to
  `runs/packets/<company>-<role>/packet.md`

Sequence: finder works first, but validator should start checking listings the moment
finder reports the first few rather than waiting for the full batch. Same for tailor once
validator marks something GO. Don't wait for full batches between steps, let the handoffs
overlap. Once all three are done, summarize back to me: how many found, how many GO, and
the list of packets ready for review.

---

### Going further

Rerun this with a different `PROFILE.md` (a different target role, say) to see it build a
fresh batch. Open `runs/validated.md` after a run and you'll see every SKIP has a real
reason attached, not just a silent drop, that's the validator doing its job.

When you're ready to actually apply to one of the packets, see
`prompts/form-filler-prompt.md` — that's a separate step on purpose, since it's the only
part of this pipeline that touches a real website with your real information.
