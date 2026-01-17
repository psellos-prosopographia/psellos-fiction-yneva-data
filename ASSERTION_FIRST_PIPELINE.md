# Assertion-first pipeline (alternate entry point)

This document defines an **alternate intake pipeline** for the fictional corpus. It does not replace
source-first or source-parallel workflows; it provides a sanctioned entry point for building the
assertion catalogue **before** creating the fictional corpus or source objects.

## When to use this pipeline

Use the assertion-first pipeline when narrative coherence and world coverage demand that assertions
be enumerated prior to crafting the fictional sources that will later attest to them.

## Core principle

Assertions remain the core corpus. Sources can be backfilled later. The alternate pipeline exists so
that the dataset can grow through **assertion coverage** even when source artifacts are not yet
created.

## Entry point and steps

1. **Define the narrative scope** (places, period, polities, characters, institutions).
2. **Create or extend entities** needed for the assertions (persons, places, offices, sources-to-be,
   events, etc.).
3. **Normalize and resolve inputs** (names, entity types, identifiers, and terminology) using the
   normalization and onomastics policies before drafting assertions.
4. **Draft assertions** using normal assertion shapes and provenance rules, marking provenance as
   synthetic or editorial where appropriate.
5. **Register narrative layers for placeholders** (see requirement below).
6. **Track unresolved source placeholders** so later ingestion can backfill attestations without
   reworking the assertion corpus.

## Required reference specs (keep updated)

Use these canonical documents when drafting or validating assertion-first material:

* `SPEC_ASSERTION_SHAPE.md` — assertion structure and required fields.
* `SPEC_PROVENANCE.md` — provenance encoding rules.
* `SPEC_TIME_SCOPE.md` — time scoping requirements.
* `SPEC_ELEMENT_FORMAT.md` — formatting and element conventions.
* `ID_POLICY.md` — identifier conventions.
* `SPEC_DATASET_LAYOUT.md` — canonical file placement.
* `NORMALIZATION_AND_AUTHORITY_CONTROL_POLICY.md` — normalization rules and authority control.
* `ONOMASTICS_POLICY.md` — naming and designation modeling rules.
* `L_LAYER_CLASSIFICATION_SPEC.md` — narrative layer classification rules.
* `L_LAYER_CATALOGUE.md` — narrative layer registry and IDs.

## Placeholder canon requirement (assertion-first)

When using placeholder canon or narrative notes as the basis for assertions, **the narrative layer
must be explicit**:

* If a placeholder canon is named in narrative notes or in a list of natural-language text queued
  for assertion extraction, it **must** either:
  * map to an existing `L_LAYER` identifier in the dataset, or
  * be added to `L_LAYER_CATALOGUE.md` as an explicit entry (even if marked provisional in prose).

This ensures that assertion-first material is still anchored to a concrete narrative layer, and can
later be reconciled with the fictional corpus without losing epistemic provenance.

## Relation to the normal pipeline

* The **normal pipeline** remains authoritative when sources and attestations are available.
* The **assertion-first pipeline** acts as an alternate entry point with a clear bridge to later
  source ingestion: assertions should already carry layer + provenance scaffolding so that sources
  can be attached without rewriting assertion semantics.
