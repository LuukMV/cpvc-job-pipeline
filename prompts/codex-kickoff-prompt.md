# Codex kickoff prompt

Before running this, copy `PROFILE_TEMPLATE.md` to `PROFILE.md` and fill it in with your own
real information.

Codex should pick up `AGENTS.md` automatically when you open this folder. If it doesn't act
on it, paste the block below as-is.

---

Follow the core pipeline described in `AGENTS.md`: read `RULES.md` and `PROFILE.md`, then
run the finder role, then the validator role, then the tailor role, each as described in
their files under `.claude/agents/`, writing to `runs/` as you go. Don't run the form-filler
step, that's separate. When done, tell me how many listings you found, how many were marked
GO, and where the packets landed.

---

### Pre-filling a real application afterward

Only once you've read a packet in `runs/packets/` and actually want to apply to it. Tell
Codex: "Run the form-filler step from AGENTS.md on `runs/packets/<company>-<role>/packet.md`."
It opens a fresh tab, fills what it can, and stops before Submit.
