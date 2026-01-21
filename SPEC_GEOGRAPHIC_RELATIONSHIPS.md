# Geographic References and Relationship Assertions

This document defines how to encode geographic references and spatial relationships for `place`
entities in the prosopographical dataset. It establishes a consistent, assertion-first approach
for map-derived coordinates, region extents, and derived relationships while preserving provenance,
time scope, and narrative layering.

## 1. Scope and principles

* **All geographic facts are assertions.** There is no implicit or “self-evident” geography in the
  dataset; every spatial statement must be asserted with provenance.
* **Places are the only geographic base entity.** Continents, regions, seas, and landmarks are all
  `place` entities and gain meaning through assertions.
* **Spatial relationships are expressed as relations.** Containment, adjacency, and overlap are
  represented with relation predicates and may coexist with contradictory claims.

## 2. Canonical predicates for spatial relations

Use the relation predicates defined in `vocabularies/relation_types.yml` for geographic
relationships:

* `rel.located_in` — place is located within another place.
* `rel.contains` — place contains another place (inverse of `rel.located_in`).
* `rel.adjacent_to` — places are adjacent (symmetric).
* `rel.borders` — places share a border (symmetric).
* `rel.overlaps` — spatial extents overlap (symmetric).

All such relations must include:

* `narrative_layer`
* `provenance` (source or editorial synthesis)
* `time_scope` where relevant

## 3. Map-derived geometry as characterization assertions

Map-derived coordinates and extents are encoded as `rel.characterized_as` assertions. The value
object uses the `geometry` key as a structured payload. This is intentionally descriptive rather
than prescriptive; downstream tools may compute derived relationships, but the derived relationships
must still be asserted to become part of the canonical dataset.

Recommended value shape:

```yaml
predicate: rel.characterized_as
value:
  geometry:
    reference_frame: yneva_world_map_v1
    map_canvas_px: [700, 700]
    type: raster_label
    source: colorcodes_labels.npy
    value: 25
    bbox_px: [202, 168, 370, 409]
```

### Required geometry keys

* `reference_frame` — stable identifier for the map or coordinate system.
* `map_canvas_px` — pixel dimensions of the source map (width, height).
* `type` — geometry representation type (e.g., `raster_label`).
* `source` — source artifact (file name or dataset identifier).
* `value` — geometry label or identifier.
* `bbox_px` — bounding box in pixel coordinates `[min_x, min_y, max_x, max_y]`.

### Optional geometry keys

* `centroid_px` — explicit centroid, if precomputed.
* `notes` — clarifying notes for extraction or uncertainty.

## 4. Environmental and terrain characterization

Environmental summaries (climate, terrain, hydrology) are encoded as `rel.characterized_as`
assertions under a `geo` or `environment` key. The structure should remain stable to allow
consistent extraction, but values may be partial or uncertain.

Recommended shape:

```yaml
predicate: rel.characterized_as
value:
  geo:
    summary: ...
    climate:
      notes: ...
      seasonality:
        summer_dryness: 0.2
        winter_wetness: 0.5
    terrain:
      elevation_m:
        min: 150
        max: 900
      ruggedness: 0.35
      landforms:
        - name: gentle_hills
          weight: 0.6
    hydrology:
      drainage: exorheic
      seasonality:
        baseflow_index: 0.7
        drying_risk_summer: 0.2
```

## 5. Relationship derivation and assertion policy

Derived relationships **may be computed** from geometry (e.g., adjacency from overlapping
bounding boxes), but they **must be asserted** to be considered canonical.

When asserting derived relationships:

* Use **editorial provenance** indicating the derivation method.
* Include the `reference_frame` in provenance notes.
* Prefer `rel.adjacent_to` for proximity, `rel.borders` for explicit border claims, and
  `rel.overlaps` for intersecting extents.

Example provenance note:

```
notes: Derived from raster_label geometry overlap on yneva_world_map_v1.
```

## 6. Time scope and narrative layering

Spatial relationships may vary by period or narrative layer. If the relation is time-bound or
layer-specific, include `time_scope` and the appropriate `narrative_layer`. If timeless within a
layer, `time_scope` may be omitted per `SPEC_TIME_SCOPE.md`.

## 7. Implementation checklist

1. Create the `place` entity (region, sea, etc.).
2. Add `rel.designated_as` naming assertions.
3. Add `rel.characterized_as` assertions for geometry and environment.
4. Add explicit relation assertions (`rel.located_in`, `rel.adjacent_to`, etc.).
5. Record editorial or source-based provenance for every assertion.
