# Assertion-first pipeline as alternate entry point

**Date:** 2025-09-15
**Status:** accepted

## Context

The fictional corpus may be authored after assertions are already enumerated for narrative coverage
and world completeness. The existing workflow implicitly assumes sources or attestation notes are
available early, which makes it harder to scale assertion cataloguing when sources are delayed.

## Decision

Adopt an **assertion-first pipeline** as an **alternate entry point** (not a replacement). This
pipeline allows assertions to be authored before source artifacts exist, while preserving
provenance scaffolding and narrative layering for later backfill.

Key requirements:

* Assertions must still conform to canonical assertion shape and provenance rules.
* Placeholder canon or narrative notes used to justify assertions must be anchored to an explicit
  `L_LAYER` identifier.
* If a placeholder canon is named in narrative notes or in a list of natural-language text queued
  for assertion extraction, it must either map to an existing `L_LAYER` or be added as an explicit
  entry in `L_LAYER_CATALOGUE.md`.

## Consequences

* Document the alternate pipeline in a new repository doc and reference it in README.
* Validation should treat assertion-first data as legitimate so long as layer and provenance
  scaffolding are present.
* Later source ingestion can attach attestations without restructuring assertions.

