# Schema — Signal & Shadow Tools Reference

Schema version: **1**

This document defines the structure of `tools.json`. The schema is stable. Breaking changes increment the major version and are documented in CHANGELOG.md.

## Top-level structure

```json
{
  "meta": { ... },
  "tools": [ ... ]
}
```

### `meta` object

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `schema_version` | string | yes | Schema major version. Currently `"1"`. |
| `lst_version` | string | yes | The LST-001 version that entries are written under. Example: `"LST-001 v1.0.3"`. |
| `index_updated` | string (ISO date) | yes | Date the dataset as a whole was last updated. |
| `license` | string | yes | License identifier. Currently `"MIT"`. |
| `canonical_url` | string | yes | The canonical web URL where this dataset is published. |
| `repo_url` | string | yes | The source repository URL. |
| `publisher` | string | yes | `"Signal & Shadow"`. |

### `tools` array

An array of tool entries. Each entry is an object with the fields below.

## Tool entry fields

### `id` (required)

Stable identifier. Lowercase alphanumeric and hyphens only. No spaces, no underscores, no uppercase.

**Once published, an `id` is never renamed.** External citations use it.

Examples: `"marinetraffic"`, `"opencorporates"`, `"crt-sh"`.

### `name` (required)

Display name as the tool presents itself.

Examples: `"MarineTraffic"`, `"OpenCorporates"`, `"crt.sh"`.

### `url` (required)

Canonical domain. No `https://`, no trailing slash, no path unless the tool's canonical entry point requires one.

Examples: `"marinetraffic.com"`, `"aleph.occrp.org"`, `"yandex.com/images"`.

### `category` (required)

One of the editorial categories. Current canonical list:

- `Maritime`
- `Aviation`
- `Corporate`
- `Domains`
- `Image`
- `Archiving`
- `Network`
- `Geolocation`
- `Social`
- `Public Records`
- `Sanctions`
- `Crypto`

New categories require an editorial decision. Open an issue before introducing one.

### `jurisdiction` (required)

Where the tool is operationally strong, in editorial shorthand.

Examples: `"Global"`, `"EU"`, `"US"`, `"Russia/CIS"`, `"140+ jurisdictions"`, `"Global · IMO-backed"`.

The page groups entries by jurisdiction, normalising the string into one of: Global, EU/Europe, US, Russia/CIS, Regional.

### `pricing` (required)

Exactly one of:

- `Free` — no cost to use, including any required registration
- `Freemium` — usable free tier, paid tier exists for higher volume or features
- `Paid` — no usable free tier

### `cost` (required)

Real pricing as checked on the verification date. Specific numbers, not vague tiers.

Good: `"Free with mandatory registration."`, `"Free tier limited to 4-day track. Pro from $11/mo."`, `"API from £350/yr."`

Bad: `"Partially Free"`, `"Pricing varies"`, `"Contact for quote"` (unless that is the literal pricing model).

### `desc` (required)

One or two sentences. Editorial register, not marketing.

Good: `"Live and historical AIS vessel tracking with port-call records and ownership lookups via IHS data."`

Bad: `"The world's leading vessel tracking platform, trusted by professionals everywhere."`

### `opsec` (required)

Operational security notes. What the tool logs, who can see it, what credential exposure looks like, what the request signature looks like to third parties.

Sources for OPSEC claims must be one of:
1. The verifier's direct operational experience
2. Cited published research (Bellingcat, IDI, DigitalDigging, Tactical Tech, etc.)
3. The tool's own published documentation

If none of the above can support the claim, the field stays empty.

### `reliability` (required)

Coverage and reliability notes. Where the tool works well, where it fails, what to cross-reference for any claim that depends on it.

Same sourcing requirement as `opsec`.

### `verified` (required)

ISO date format: `YYYY-MM-DD`. The date the entry was last verified by the named verifier.

A verification check means: the verifier confirmed that the URL still works, the pricing matches what's stated, the OPSEC and reliability notes still hold, and the cards array reflects current methodology.

Verification cadence: at least every 90 days for the headline tools, every 180 days for the long tail.

### `verifier` (required)

Initials of the editor who performed the most recent verification. Two letters, no punctuation.

The verifier roster is maintained in the repo's CONTRIBUTORS.md.

### `cards` (required)

Array of LAN card identifiers that cite this tool. Empty array if no card references it yet.

Examples: `["MAR-002", "MAR-005"]`, `["LAN-001"]`, `[]`.

Card identifiers follow the format `[SERIES]-[NUMBER]` where SERIES is a three-letter editorial series code and NUMBER is a three-digit identifier.

## Validation

Entries are validated on every commit by GitHub Actions. The validator checks:

- All required fields present
- `id` matches the stable-identifier pattern
- `pricing` is one of the three allowed values
- `verified` is a valid ISO date and not in the future
- `category` is on the canonical list
- `cards` array contains only valid card identifiers

Entries failing validation block the commit.

## Versioning

The schema version increments only when a change breaks consumers of `tools.json`:

- Renaming a field
- Removing a field
- Changing a field's type
- Tightening a constraint that existing entries violate

Adding a new optional field is not a breaking change. The major version stays the same.

History of schema versions is in CHANGELOG.md.
