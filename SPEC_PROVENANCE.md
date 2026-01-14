# SPEC: Provenance encoding

This document is normative for encoding attestation and editorial synthesis in assertions.

It aligns with `NORMALIZATION_AND_AUTHORITY_CONTROL_POLICY.md` §9.

## Provenance is mandatory

Every assertion MUST include a `provenance` object. Assertions MUST NOT make unmarked, unsourced factual claims.

## Provenance object

`provenance` has:

- `kind` (required): `attested` or `editorial`
- `attested_by` (required when kind is `attested`): list of attestations
- `editorial_synthesis` (required when kind is `editorial`): editorial method encoding

### Attested provenance

Use `kind: attested` for assertions grounded in a source.

Shape:

```yml
provenance:
  kind: attested
  attested_by:
    - source: e.source.some-source
      locator:
        page: '12'
        line: '3-7'
      notes: Optional short note
```

#### `attested_by` cardinality

- `attested_by` is plural and MUST be a list.
- If multiple sources attest the same claim, include multiple entries.

#### `source`

- `source` MUST be an entity id referring to an entity of kind `source` or `text`.
- Prefer stable source entities over ad-hoc citation strings.

#### Locators

Locators are encoded under `locator` and may include any of:

- `page`
- `folio`
- `line`
- `section`
- `chapter`
- `paragraph`
- `uri_fragment`
- `note`

Locator values are strings to preserve formats like `12r`, `III.4`, `p. 12–13`.

### Editorial provenance

Use `kind: editorial` only when the claim is not directly attested as a claim in a source, but is an editorial
product (synthesis, inference, or normalization scaffolding).

Shape:

```yml
provenance:
  kind: editorial
  editorial_synthesis:
    method: synthesis
    basis:
      - e.source.some-source
    notes: How the synthesis was produced and what it is (and is not) claiming.
```

#### `method`

Allowed:

- `synthesis` — editorial summary or harmonization, explicitly not quoting a single source claim verbatim
- `inference` — editorial inference from attested facts
- `normalization_scaffold` — minimal structural claims needed for dataset coherence (e.g., containment scaffolding)

#### `basis`

- Optional list of entity ids (usually source entities) used as inputs.
- When absent, the editorial claim should be treated as “repository-internal” and reviewed carefully.

## Multiple sources per assertion

- Multiple attestations go in `attested_by` as multiple list items.
- Do not create multiple separate assertions solely to list multiple sources unless the claims differ by layer,
  wording, or modality.

## Explicit prohibition

- A validator MUST reject any assertion where `provenance` is missing.
- A validator MUST reject `kind: attested` without a non-empty `attested_by`.
- A validator MUST reject `kind: editorial` without `editorial_synthesis`.

## Quotation field (optional)

`attested_by[].quoted` MAY be used for short excerpts when necessary, but should be used sparingly and must not
replace locators.
