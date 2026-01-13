# ID Policy

This policy defines canonical identifier formats for entities, assertions, vocabularies, and related records.
All identifiers are stable, immutable, lowercase, ASCII, URL-safe, and suitable for filenames and YAML keys.

## 1) Namespaces and formats

ABNF-like formats:

```
entity-id     = "e." entity-kind "." slug
assertion-id  = "a." subject-id "." predicate "." time "." layer "." seq
vocab-term-id = vocab-namespace "." slug
source-id     = "s." slug
layer-id      = "l." slug
regime-id     = "r." slug

entity-kind   = "person" / "polity" / "institution" / "office" / "place" / "artifact" / "text" / "source"
slug          = 1*(lower / digit / "-")
predicate     = 1*(lower / digit / "-")
time          = "timeless" / 1*(lower / digit / "-")
layer         = layer-id
seq           = 1*digit
vocab-namespace = "rel" / "evt" / "role" / "cls" / "state" / "title"

lower         = %x61-7A
 digit        = %x30-39
```

Rules:

* IDs are immutable once minted.
* Slugs derive from stable labels but do not change if labels change.
* No dates embedded in entity IDs (dates belong in assertions).
* The kind segment is mandatory for entity IDs, preventing cross-kind collisions.

## 2) Entity IDs

Kinds:

* `person`
* `polity`
* `institution`
* `office`
* `place`
* `artifact`
* `text`
* `source` (source-as-entity)

Format: `e.<kind>.<slug>`

Examples:

* `e.person.yneva-sar` (the protagonist)
* `e.polity.river-crown` (polity)
* `e.institution.glass-archive` (archive)
* `e.office.high-legate` (office)
* `e.place.veil-harbor` (place)
* `e.artifact.river-sigil` (artifact)
* `e.text.ashen-annals` (text)
* `e.source.ashen-annals-copy-a` (source-as-entity)

## 3) Assertion IDs

Assertions are atomic statements about a subject and predicate, scoped by time, narrative layer, and attribution.

Format (deterministic, component-based):

`a.<subj>.<predicate>.<time>.<layer>.<seq>`

* `<subj>` is the full entity ID (with dots converted to hyphens for filename safety).
* `<predicate>` is a stable predicate slug (e.g., `holds-office`, `related-to`, `attested-in-source`).
* `<time>` is a normalized time token (e.g., `ye-0402`, `ye-0402-0410`, `timeless`).
* `<layer>` is a layer ID (e.g., `l.legendary`, `l.analytic`).
* `<seq>` is a zero-padded integer for disambiguation within the same subject/predicate/time/layer.

Rationale: deterministic components allow local minting without global coordination and remain stable under file reformatting.

Rules:

* Mint a new assertion ID when the subject, predicate, time, or layer changes.
* Contradictory claims must be represented as new assertion IDs; never overwrite or delete.
* If the same claim gains an additional source, add the source to the assertion record without changing the ID.
* If an assertion is split or merged, the original IDs remain and are deprecated with a pointer to replacements.

Examples:

* `a.e-person-yneva-sar.holds-office.ye-0402.l.legendary.001`
* `a.e-person-yneva-sar.holds-office.ye-0402.l.analytic.001`
* `a.e-person-yneva-sar.related-to.timeless.l.legendary.002`
* `a.e-place-veil-harbor.attested-in-source.timeless.l.analytic.001`
* `a.e-person-yneva-sar.characterized-as.ye-0402-0410.l.propagandistic.003`
* `a.e-polity-river-crown.claims-territory.ye-0390-0415.l.legendary.001`

Contradiction example (same subject/time/predicate, different layers):

* `a.e-person-yneva-sar.holds-office.ye-0402.l.legendary.001`
* `a.e-person-yneva-sar.holds-office.ye-0402.l.analytic.001`

## 4) Vocabulary term IDs + versioning

Vocabulary files live in `vocabularies/*.yml` and must include a minimal header:

```
vocabulary_id: vocab.relation-types
version: 0.1.0
status: draft
last_updated: 2025-01-01
terms:
  rel.parent-of:
    label: parent of
```

Conventions:

* Term IDs are stable keys prefixed by namespace: `rel.`, `evt.`, `role.`, `cls.`, `state.`, `title.`.
* Term IDs never change; display labels may be updated.
* Deprecate terms instead of deleting them, using `deprecated: true` and an optional `replaced_by` field.
* Vocabulary files use semantic versioning (`MAJOR.MINOR.PATCH`).
  * Increment MAJOR for breaking changes (removed terms or semantic shifts).
  * Increment MINOR for additive terms.
  * Increment PATCH for label or metadata fixes.
* Downstream consumers may assume backward compatibility within a MAJOR version.

## 5) File layout expectations

Entities:

* Each entity is a single YAML file named exactly `<entity_id>.yml`.
* Entity files live in `entities/<kind>/`.

Assertions:

* Assertions are grouped by subject in `assertions/<entity_id>.yml`.
* Each file contains a list of assertions about the subject, using the assertion IDs above.

Supporting records:

* Sources: `sources/<source_id>.yml`.
* Layers: `layers/<layer_id>.yml`.
* Rule regimes: `rule_regimes/<regime_id>.yml`.

This layout is mandatory for new data and should be followed when migrating existing data.
