# SPEC: Dataset filesystem layout

This document is normative for the repository layout and naming rules for dataset elements.

## Top-level directories

Required:

- `entities/` — canonical dataset elements (one file per entity)
- `vocabularies/` — controlled vocabularies
- `rule_regimes/` — scoped rule regimes
- `build/` — build contracts and tooling pins

Recommended/available (may be empty during initialization):

- `templates/` — canonical authoring templates
- `layers/` — reserved for future layer registry materialization (catalogue is authoritative)
- `sources/` — reserved for raw source assets or ingestion notes (not canonical assertions)
- `assertions/` — reserved for build outputs or secondary indexes

## Entities directory

Canonical entity files live under `entities/<kind>/`.

`<kind>` MUST be one of:

`person`, `polity`, `institution`, `office`, `place`, `artifact`, `text`, `source`, `species`.

## One entity = one file

- A single entity id MUST NOT be split across multiple YAML files.
- Model plurality and contradiction through assertions and narrative layers.

## File naming conventions

- Filenames are lowercase.
- Filename MUST equal the entity `id` plus `.yml`.
- IDs must follow `ID_POLICY.md`.

Example:

- `entities/person/e.person.soller-aurion.yml`
  - must contain `id: e.person.soller-aurion`
  - must contain `kind: person`

## Assertions are embedded

This repository’s normative authoring practice is embedded assertions:

- assertions live inside the entity file under `assertions:`

Do not author standalone assertion files unless a later decision creates a separate assertion store.

## Adding new entity types

Adding a new entity kind changes ID syntax and directory layout and therefore requires governance tracking:

- update `ID_POLICY.md`
- create `entities/<newkind>/`
- update `build/build_contract.yml`
- update templates (if applicable)
- record the change per `DECISIONS.md`
