# SPEC: Assertion object shape

This document is normative for the shape of assertion objects embedded in entity files.

Assertions encode claims. They are not free-form notes. Meaning is defined by controlled vocabularies.

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
- Semantic meaning (arity, symmetry, domain/range, etc.) is defined by the vocabulary entry; this spec only
  defines encoding and contracts.

## Arguments: `object` vs `value`

Exactly one of `object` or `value` MUST be present.

### When to use `object`

Use `object` when the assertion’s argument is another entity:

- kinship / relationships
- office holding
- place containment
- participation in events
- identification/aliasing links
- attestations in a source (if modeled as entity relations)

`object` MUST be a canonical entity id (`e.<kind>.<slug>`).

### When to use `value`

Use `value` when the assertion’s argument is a literal or a small structured literal:

- name strings and name metadata (onomastics)
- classification tags (as vocabulary ids)
- numeric or textual values
- locational coordinates (if used later)

`value` may be a scalar or mapping. It must remain small, stable, and mechanically comparable.

### When both are forbidden

- A validator MUST reject assertions that have both `object` and `value`.
- A validator MUST reject assertions that have neither.

## Directionality rules

- The subject of every embedded assertion is the containing entity file (`id`).
- Direction is always: **subject** (file entity) —`predicate`→ **object/value**.

Do not encode a `subject` key inside assertions.

## Symmetric relations

If a predicate is symmetric (as declared in its vocabulary metadata), the dataset MUST represent the symmetry
as two assertions:

- one in the first entity file pointing to the second
- one in the second entity file pointing to the first

This preserves locality and makes entity-centric consumption deterministic.

If a predicate is not declared symmetric, do not assume symmetry.

## Narrative layer requirement: `narrative_layer`

- Every assertion MUST include `narrative_layer`.
- `narrative_layer` MUST be a layer key present in `L_LAYER_CATALOGUE.md` (e.g., `L1.1`, `L4.4`).
- Do not invent layers. If a needed layer is missing, follow governance to add it.

## Provenance requirement: `provenance`

- Every assertion MUST include `provenance` (see `SPEC_PROVENANCE.md`).
- Assertions may be attested (`kind: attested`) or editorial (`kind: editorial`).
- Unmarked unsourced factual claims are prohibited.

## Optional fields

### `time_scope`

Use when the claim is temporally bounded or temporally uncertain. See `SPEC_TIME_SCOPE.md`.

### `rule_regime`

Use only when a claim is scoped to an explicit regime context. The regime id must exist in `rule_regimes/*.yml`.

### `modality`, `confidence`, `notes`

These are optional, non-authoritative encodings for downstream ranking and filtering.

- `modality`: short tag such as `reported`, `allegorical`, `speculative`
- `confidence`: numeric in `[0, 1]`
- `notes`: brief editorial note (not a replacement for provenance)

Do not use these fields to smuggle semantics that belong in vocabularies or rule regimes.
