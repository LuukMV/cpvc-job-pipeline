# Pipeline rules — every agent follows these

These apply to the finder, validator, tailor, and form-filler. They exist because a system
that fabricates facts or submits things on your behalf is worse than no system at all.

## Source of truth
Every fact about you comes from `PROFILE.md` (copy `PROFILE_TEMPLATE.md` to this name and
fill it in — it's gitignored). If a form or a cover letter needs something that isn't in
`PROFILE.md`, write `[CONFIRM: what's missing]` in the output and list it under "Questions
for you" at the end. Never invent a number, a date, a title, or a result.

## Never
- Fabricate a fact, number, date, person, or outcome not present in `PROFILE.md`.
- Submit an application, create an account, accept terms, or send an email/message on your
  behalf. Every agent stops short of the final action and hands it to you.
- Enter your SSN, date of birth, or financial info into any form, ever.
- Check an attestation, e-signature, or consent checkbox for you.
- Answer a voluntary EEO/diversity question — leave those blank, or use your stated default
  from `PROFILE.md`, unless the form forces a choice (then pick "decline to self-identify"
  or closest equivalent).

## Browser automation (form-filler agent only)
- **One tab per listing, always.** Never reuse a tab across two different applications —
  overwriting one before you've seen it destroys the filled-out form with no way back. Open
  a fresh tab for every new site.
- Stop at the Submit button. Don't click it. Say what's left for the human to review.
- If a judgment-call question shows up (e.g. "do you meet all the minimum qualifications?",
  background-check consent, a salary question with no range in `PROFILE.md`), leave it
  blank and flag it rather than guessing.

## Writing voice
Plain, specific, first person. No corporate buzzwords ("synergy," "leverage," "passionate
about"). No em dashes. Cover letters open "Hi [Company] team," and sign "Best," then your
name. Under 250 words.

## Ghost-listing signals (validator uses these; see `.claude/agents/validator.md` for the
full checklist)
A listing that fails enough of these gets marked SKIP, not silently dropped — always say why.
