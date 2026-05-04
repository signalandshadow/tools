# Signal & Shadow · OSINT Tool Database

A vetted operational tool database for investigative journalists.

Every entry is signed, dated, and linked to the methodology cards that use it. The project ships fewer tools than aggregator catalogues, verified by the Signal & Shadow editorial team, with operational notes covering OPSEC, reliability, and cost.

**Live page:** [signalandshadow.io/osint-tool-database](https://signalandshadow.io/osint-tool-database)
**Open dataset:** [tools.json](./tools.json)
**Editorial standard:** [LST-001](https://github.com/signalandshadow/lst-001)

## What this is

A single-page database, hosted as a static site, with the underlying dataset published as a citable JSON file. Built for two audiences:

1. Working investigative journalists who need an operational reference. What does the tool actually do, what's the OPSEC posture, where does it fail, what does it cost.
2. Other publishers and tool-builders who want to consume the dataset directly. The schema is stable and versioned. MIT licensed.

For breadth across the wider OSINT ecosystem, see [OSINT Navigator](https://navigator.indicator.media). This database is deliberately narrower and deeper.

## Schema

Each tool entry has the following fields:

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `id` | string | yes | Stable identifier. Lowercase, alphanumeric and hyphens only. Once published, never renamed. |
| `name` | string | yes | Display name. |
| `url` | string | yes | Canonical domain (no scheme). |
| `category` | string | yes | One of the editorial categories. See `schema.md`. |
| `jurisdiction` | string | yes | Where the tool is operationally strong. Examples: "Global", "EU", "US", "Russia/CIS", "140+ jurisdictions". |
| `pricing` | string | yes | One of `Free`, `Freemium`, `Paid`. |
| `cost` | string | yes | Real pricing as checked on the verification date. Specific numbers, not vague tiers. |
| `desc` | string | yes | One or two sentences. Editorial register. |
| `opsec` | string | yes | Operational security notes. What the tool logs, who can see it, credential exposure. |
| `reliability` | string | yes | Coverage and reliability notes. Where it works, where it fails, what to cross-reference. |
| `verified` | string (ISO date) | yes | Date the entry was last verified by the named verifier. |
| `verifier` | string | yes | Initials of the editor who verified the entry. |
| `cards` | array of strings | yes | LAN card identifiers that cite this tool. Empty array if no card references it yet. |

Full schema documentation in [`schema.md`](./schema.md).

## How entries are written

Every entry follows the LST-001 editorial standard:

- No em dashes
- Maximum 13 words per declarative bullet inside operational fields where bullets are used
- Practitioner voice, not marketing voice
- British English, AP Style
- Specific numbers over vague tiers (write "$11/mo" not "Partially Free")
- Provenance per claim where the operational note is non-obvious

OPSEC and reliability notes come from one of three sources:

1. The verifier's direct operational experience with the tool
2. Cited published research or community guidance (Bellingcat, IDI, DigitalDigging, Tactical Tech, etc.)
3. The tool's own published documentation about logging, retention, and authentication

If a claim cannot be sourced to one of these three, the field stays empty rather than carrying unsourced text.

## How to contribute

Contributions are welcome via pull request. Two flows:

**Propose a new tool:** open a [new-tool issue](./.github/ISSUE_TEMPLATE/new-tool.md) describing the tool and why it earns a slot. The Signal & Shadow editorial team adds the entry after a verifier has used the tool operationally.

**Propose a correction:** open a [correction issue](./.github/ISSUE_TEMPLATE/correction.md) identifying the entry and the correction with a source. Corrections without a source are not merged.

Direct PRs that add or modify entries will be reviewed but not auto-merged. Every entry that ships under the Signal & Shadow name needs a verifier on the editorial team.

## Citing this dataset

The dataset is MIT licensed. Cite it as:

> Signal & Shadow OSINT Tool Database, [version], [date accessed]. https://signalandshadow.io/osint-tool-database

Individual tool entries can be cited by their stable identifier:

> signalandshadow.io/osint-tool-database#marinetraffic

## Development

The site is one HTML file plus one JSON file. No build step.

To run locally:

```bash
git clone https://github.com/signalandshadow/tools.git
cd tools
python3 -m http.server 8000
# open http://localhost:8000
```

The HTML fetches `tools.json` from the same directory.

## License

MIT. See [LICENSE](./LICENSE).

Editorial content (the operational notes) is © Signal & Shadow but freely usable under the MIT terms with attribution.

## Changelog

See [CHANGELOG.md](./CHANGELOG.md).
