# SPEC: Dataset element format

This document is normative for *how to write* a dataset element file (YAML) in this repository.
For policy and meaning, see the authority index and the vocabulary/rule-regime files.

## Element philosophy

- One **entity** = one YAML file.
- Entities are structurally minimal; they gain content through **assertions**.
- Assertions are the only place where the dataset records claims (including names), and every assertion
  must carry narrative layering and provenance.

## Where element files live

Entity files live under:

- `entities/person/`
- `entities/place/`
- `entities/source/`
- (and other entity kinds listed in `ID_POLICY.md`)

The filename MUST equal the element `id` plus `.yml`.

Example: an entity with `id: e.person.alexios-i` must be stored as:

- `entities/person/e.person.alexios-i.yml`

See `SPEC_DATASET_LAYOUT.md` for the full layout contract.

## Canonical structure of an entity file

Every entity file has:

- `id` (required): canonical entity identifier
- `kind` (required): entity kind enum
- `label` (required): short human label for display/debugging
- `assertions` (required): array of assertion objects

Optional top-level keys are allowed, but must remain minimal and non-semantic:
`description`, `notes`, `redirects_from`, `deprecated`, `meta`.

### Top-level fields

#### `id` (required)

- Must match the ID syntax in `ID_POLICY.md`.
- Must match the filename.

#### `kind` (required)

Must be one of:

`person`, `polity`, `institution`, `office`, `place`, `artifact`, `text`, `source`.

#### `label` (required)

A short human-readable label (not a canonical name field). Names are modeled as assertions
per `ONOMASTICS_POLICY.md`.

#### `assertions` (required)

A list of assertion objects. Each assertion is a single claim about the entity.

- Assertions are embedded in the entity file (recommended and normative here).
- The entity file is the implicit **subject** of each assertion; do not include `subject` fields.

See `SPEC_ASSERTION_SHAPE.md` and `SPEC_PROVENANCE.md`.

## Worked examples

All examples below are valid under `build/build_contract.yml`.

### Example: person

```yml
id: e.person.soller-aurion
kind: person
label: Aurion of Soller
description: A fictional cleric referenced in Sollerian and Magocratic traditions.
assertions:
  - id: a1
    predicate: rel.named_as
    value:
      string: Aurion
      name_kind: primary
      language: und
    narrative_layer: L1.1
    provenance:
      kind: attested
      attested_by:
        - source: e.source.sollerian-chronicle
          locator:
            book: I
            chapter: '3'
  - id: a2
    predicate: rel.characterized_as
    value:
      classification: cls.cleric
    narrative_layer: L4.4
    provenance:
      kind: editorial
      editorial_synthesis:
        method: synthesis
        basis:
          - e.source.sollerian-chronicle
        notes: Synthesized for normalization; not claiming direct source wording.
```

### Example: place

```yml
id: e.place.soller
kind: place
label: Soller
assertions:
  - id: a1
    predicate: rel.named_as
    value:
      string: Soller
      name_kind: primary
      language: und
    narrative_layer: L1.1
    provenance:
      kind: attested
      attested_by:
        - source: e.source.sollerian-chronicle
          locator:
            book: I
            chapter: '1'
  - id: a2
    predicate: rel.located_in
    object: e.polity.church-of-the-sun
    narrative_layer: L4.4
    provenance:
      kind: editorial
      editorial_synthesis:
        method: synthesis
        basis:
          - e.source.sollerian-chronicle
        notes: Polity containment asserted analytically for browsing.
```

### Example: source

```yml
id: e.source.sollerian-chronicle
kind: source
label: Sollerian Chronicle
description: A canonical narrative source entity.
assertions:
  - id: a1
    predicate: rel.characterized_as
    value:
      classification: cls.chronicle
    narrative_layer: L4.4
    provenance:
      kind: editorial
      editorial_synthesis:
        method: synthesis
        basis: []
        notes: Source classification is an editorial categorization.
```

## Anti-patterns

### Do not put names in top-level fields

Bad:

```yml
label: Alexios
names:
  - Alexios
```

Good: use `rel.named_as` assertions (see `ONOMASTICS_POLICY.md`).

### Do not omit narrative layering

Bad:

```yml
- predicate: rel.child_of
  object: e.person.x
```

Good: include `narrative_layer` and provenance.

### Do not make unsourced factual claims

Bad:

```yml
- predicate: rel.holds_office
  object: e.office.patriarch
  narrative_layer: L1.1
  provenance: {}
```

Good: use `provenance.kind: attested` with `attested_by`, or mark as `editorial`.

### Do not split one entity across multiple files

One entity = one file. If an identity is composite or disputed, model the dispute via assertions and layers.
