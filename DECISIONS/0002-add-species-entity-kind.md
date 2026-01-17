# Decision 0002: Add species entity kind

**Date:** 2026-01-20  
**Status:** accepted

## Context

The corpus includes biological and mythic kinds (e.g., humans, nyelves, felierni) that are currently
modeled as institutions. This blurs ontology, complicates authority control, and makes it harder to
express species-specific assertions without conflating them with social institutions.

## Decision

Introduce a `species` canonical entity kind. Add a dedicated `entities/species/` directory and
permit `e.species.*` IDs. Migrate existing species-like institution entities (human, nyelf,
felierni) into the new kind and collapse humanity into the human species.

## Consequences

* Build contract, ID policy, and dataset layout specs must include the new entity kind.
* References to the migrated entities must be updated to the new `e.species.*` IDs.
* Future species should be authored under `entities/species/`.
