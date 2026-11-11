# AI Steward Rules (ai-steward)

> The token you spend on records is prepayment; the token you spend on confusion is loan shark money.

For people who use AI agents **for the long haul**: as your agent takes on more domains and the context grows, sooner or later **mystery scripts appear in your environment** — nobody knows who wrote them, what they do, or what they affect, and cleaning them up becomes dangerous. This repository is a **steward record-keeping system** proven in real production use: two iron rules + a three-manual structure + a triage methodology, turning any AI agent into a **steward with a clear ledger**.

- 中文文档（默认）：[README.md](README.md)
- English docs: [README_EN.md](README_EN.md)
- Full text of the rules (bilingual): [rules/](rules/)

## Why this exists

Real (anonymized) lessons:

- A script created ten days earlier with **zero records** — troubleshooting required guessing from file timestamps and reverse-reading source code, burning more tokens than a hundred lines of registration would ever have cost;
- A cleanup that deleted a script file **without killing the process it spawned** — the port stayed occupied, the service would not start, and the incident trace took a long detour.

Conclusion: **in an unregistered environment, problems never reach their root.** And registration must happen early — within the same reply where the script lands.

## The two iron rules

### Rule 1: Script registration (six elements)

Any script-class asset **created / modified / deleted** by a conversation (shell or python scripts, cron entries, systemd units, compose fragments, scheduler tasks, ...) must be written **on the spot** into the manual **and** the agent's memory:

| # | Element | Description |
|---|---------|-------------|
| 1 | Path | Exact location of the file or entry |
| 2 | Purpose | One or two sentences on what it does |
| 3 | Affected modules | What breaks if it breaks |
| 4 | Trigger | cron / panel hook / manual / container-bundled |
| 5 | Date | Creation or modification time |
| 6 | **Provenance** | **Which agent, which conversation** (agent name + session ID + task context) |

> Deletions must be registered too, including **whether the process / cron entry was stopped along with the file** — "deleted the file, not the process" is a real incident that happened.

### Rule 2: The three registration surfaces

**A file existing does not mean it is running.** Persistent effects come only from registration surfaces; triage in this fixed order:

1. cron / crontab -l
2. systemd units / init.d
3. OS autostart (startup folders, registry Run keys)
4. Scheduler layers (e.g. DSH schedule_list, Windows Task Scheduler)

Only when all four are clean are leftover files merely "annoying to look up", not "actively causing harm".

## Three-manual structure

One manual per domain, cross-linked as siblings, each headed by the iron-rule banner:

    manuals/
    ├── <host-A>.md      # network device / server domain
    ├── <host-B>.md      # workstation domain
    └── <platform>.md    # platform / plugin / toolchain domain

- **Newest entries on top** (reverse chronological), full elements per entry with cross references
- Triage conclusions, withdrawn misjudgments, rollback points all go in — **misjudgments are worth recording more than successes**
- Templates under [templates/](templates/)

## Triage methodology

When something is wrong and "cleanup did not help", three fixed steps:

1. **grep the script name** across manuals + memory → get "purpose + affected modules + source conversation" immediately
2. **Check the registration surfaces** → confirm whether it actually runs
3. **Nothing matches** → it is a new stray → register it on the spot (post-hoc registration must be labeled as such)

## Adopting this in your agent

Paste the rule texts from [rules/](rules/) as system instructions / skills / custom instructions (works with Claude Code, DeepSeek Harness, Codex, ChatGPT, ...), and tell the agent your jurisdiction list (hosts, platforms) in the first conversation. **Starting with the next script matters more than starting tomorrow.**

## Disaster recovery

This repository is also your recovery plan: if local data is lost, clone this repository to recover the full steward system (rules + manual structure + templates), then re-roll it out via "Adopting this in your agent".

## License

[MIT](LICENSE)