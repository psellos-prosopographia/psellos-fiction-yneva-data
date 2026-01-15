# SPEC: Assertion object shape
This document is normative for the shape of assertion objects embedded in entity files. Assertions encode claims. They are not free-form notes. Meaning is defined by controlled vocabularies.

## Canonical assertion keys
Required keys:
- `id`
- `predicate`
- `narrative_layer`
- `provenance`

Optional keys (only when appropriate):
- `object`
- `value`
- `time_scope`
- `rule_regime`
- `modality`
- `confidence`
- `notes`

The complete machine contract is `build/build_contract.yml`.

## Assertion identity: `id`
- `id` MUST be unique within the containing entity file.
- `id` is a local identifier; it is not an entity ID and does not mint global identity.
- Recommended: short stable tokens (`a1`, `a2`, …) and never reuse them within the file.

## Predicate usage: `predicate`
- `predicate` MUST be a vocabulary term id, matching `ID_POLICY.md` (`rel.*`, `evt.*`, `role.*`, `cls.*`, etc.).
- `predicate` MUST exist in the corresponding vocabulary registry under `vocabularies/*.yml`.
- Semantic meaning (arity, symmetry, domain/range, etc.) is defined by the vocabulary entry; this spec only defines encoding and contracts.

## Arguments: `object` vs `value`
Exactly one of `object` or `value` MUST be present.

### When to use `object`
Use `object` when the assertion’s argument is another entity:
- kinship / relationships
- office holding
- place containment
- participation in events
- identification or co-reference
- attestations linking entities

`object` MUST be a canonical entity id (`e..`).

### When to use `value`
Use `value` when the assertion’s argument is a literal or small structured literal:
- personal, place, or occurrence names
- titles, epithets, and labels
- classification tags (as vocabulary ids)
- numeric or textual values

`value` may be a scalar or a mapping. It must remain small, stable, and mechanically comparable.

### When both are forbidden
- A validator MUST reject assertions that have both `object` and `value`.
- A validator MUST reject assertions that have neither.

## Directionality rules
- The subject of every embedded assertion is the containing entity file.
- Direction is always: **subject** —`predicate`→ **object/value**.
- Do not encode a `subject` key inside assertions.

## Symmetric relations
If a predicate is symmetric (as declared in its vocabulary metadata), the dataset MUST represent the symmetry as two assertions:
- one in the first entity file pointing to the second
- one in the second entity file pointing to the first

If a predicate is not declared symmetric, do not assume symmetry.

## Narrative layer requirement: `narrative_layer`
- Every assertion MUST include `narrative_layer`.
- `narrative_layer` MUST be a layer key present in `L_LAYER_CATALOGUE.md`.
- Do not invent layers.

## Provenance requirement: `provenance`
- Every assertion MUST include `provenance` (see `SPEC_PROVENANCE.md`).
- Assertions may be `attested` or `editorial`.
- Unsourced factual claims are prohibited.

## Designation assertions (including event naming)
Designation is the canonical mechanism for encoding how a source refers to a person, place, or occurrence.

### Scope
- There is no dedicated event entity in this data model.
- Names applied to historical occurrences (battles, councils, campaigns, revolts) are treated as **designation assertions**, not as attributes of event objects.

### Encoding rules
- Use a designation-capable predicate (e.g. `rel.designated_as`).
- The name MUST be encoded in `value`.
- Each source or cultural tradition asserting a name MUST be represented as a separate assertion.
- No designation is authoritative by default.
- The absence of a designation does not imply namelessness in the sources.

### Example pattern
```yaml
- id: a7
  predicate: rel.designated_as
  value:
    text: "Battle of Manzikert"
    language: en
  narrative_layer: L4.2
  provenance:
    kind: editorial
````

```yaml
- id: a8
  predicate: rel.designated_as
  value:
    text: "Մանազկերտի պարտությունը"
    language: hy
  narrative_layer: L4.2
  provenance:
    kind: attested
    source: s.arm_chron_12c
```

These assertions may be co-referent in interpretation, but this specification does not mandate co-reference resolution or canonical naming.

## Optional fields

### `time_scope`

Use when the claim is temporally bounded or uncertain. See `SPEC_TIME_SCOPE.md`.

### `rule_regime`

Use only when a claim is scoped to an explicit regime context. The regime id MUST exist in `rule_regIMES/*.yml`.

### `modality`, `confidence`, `notes`

Optional, non-authoritative metadata:

* `modality`: e.g. `reported`, `allegorical`, `speculative`
* `confidence`: numeric in `[0,1]`
* `notes`: brief editorial clarification

These fields MUST NOT be used to encode semantics that belong in vocabularies or rule regimes.
