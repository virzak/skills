# Skills

Agent skills for everyday decisions, not code. Each folder is one skill
with its own `SKILL.md`, in the format Claude Code and other agents that
support Agent Skills load on demand.

| Skill | What it does | Last verified |
| --- | --- | --- |
| [used-car-buying](used-car-buying/SKILL.md) | Finds and compares used cars, and reads every vehicle history report before recommending one | 2026-10-13 |

Skills that drive websites rot as those sites change. The date says when a
skill last ran end to end; past that, expect to fix selectors and URLs.

## Why used-car-buying exists

While helping shop for a family SUV, an agent searched AutoTrader.ca,
compared prices, trims and warranties, and recommended a Honda-certified
2022 CR-V, with a 2021 CR-V as the runner-up. Neither recommendation had
looked at the CARFAX reports linked on the listings.

When it did:

- The 2022 had been driven off the road into a ditch, with front and
  undercarriage damage and a $25,632 collision claim.
- The 2021 had two collision claims, $6,164 and $10,076.
- A third car's listing said "no accident"; its report showed a
  police-reported sideswipe.

Nothing in the listings hinted at any of it. The research was thorough
about everything except the one record that mattered most.

The skill makes that record a gate: no car is recommended until its
history report has been read, and the listing's own claims about its
history count for nothing. The rest of the skill (requirements, search,
model-year problems, dealer fine print, what to check in person) is the
checklist that came out of the same session.

## Install

Copy a skill folder into `~/.claude/skills/` to use it everywhere, or into
a project's `.claude/skills/` to share it with everyone working in that
project:

```sh
git clone https://github.com/virzak/skills
cp -r skills/used-car-buying ~/.claude/skills/
```

## Disclaimer

These skills come with no warranty. They are not professional, legal or
financial advice, and they do not replace an inspection by a mechanic.
Verify everything yourself before you buy. When a skill reads a website, it
does so in your browser, at your request; you are responsible for using
those sites within their terms.

## License

[MIT](LICENSE)
