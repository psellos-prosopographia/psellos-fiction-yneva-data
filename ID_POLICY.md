# ID Policy

This policy defines canonical identifier syntax, type prefixes, and stability guarantees. Identity and
normalization decisions that drive ID minting are governed by
[NORMALIZATION_AND_AUTHORITY_CONTROL_POLICY.md](NORMALIZATION_AND_AUTHORITY_CONTROL_POLICY.md).

## 1) ID syntax and character rules

* IDs are lowercase, ASCII, URL-safe, and stable once minted.
* Allowed characters: `a-z`, `0-9`, and `-` (hyphen) for slugs.
* Separators: `.` between namespace segments, `-` within slug segments.

ABNF-like summary:

```
entity-id     = "e." entity-kind "." slug
vocab-term-id = vocab-namespace "." slug
source-id     = "s." slug
layer-id      = "l." slug
regime-id     = "r." slug

entity-kind   = "person" / "polity" / "institution" / "office" / "place" / "artifact" / "text" / "source"
slug          = 1*(lower / digit / "-")
vocab-namespace = "rel" / "evt" / "role" / "cls" / "state" / "title"

lower         = %x61-7A
 digit        = %x30-39
```

## 2) Type prefixes

Entity IDs use the `e.<kind>.` prefix with one of the supported entity kinds listed above. Vocabulary
terms use their vocabulary namespace prefixes (e.g., `rel.`, `evt.`).

## 3) Immutability and corrections

* Canonical IDs do not change once minted.
* If a correction is unavoidable, record the change in a decision record and retain the legacy ID as
  a deprecated reference. If/when a redirect registry exists, add a redirect mapping; otherwise
  preserve a stub or explicit deprecation note so downstream consumers can resolve the change.
