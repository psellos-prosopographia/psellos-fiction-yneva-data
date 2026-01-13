## L* Layer Classification Spec (v0.1)

### 1. Purpose

Provide a consistent, queryable classification of statements (assertions) by **narrative layer relative to a chosen polity** (“focal polity”), while preserving each statement’s **native** polity context.

### 2. Core Concepts

#### 2.1 Polity

A geopolitical/cultural authority domain (empire, kingdom, city, church-state, etc.).

#### 2.2 Narrative

A bounded discourse context that produces assertions (chronicle, inscription tradition, court historiography, sectarian corpus, later compilation).

#### 2.3 Assertion

A single claim about some subject (person, event, institution, relationship), attributed to a narrative.

### 3. Required Data Fields

#### 3.1 Narrative fields

* `narrative_id` (stable identifier)
* `polity_id` (the narrative’s home polity)
* `native_layer` ∈ `{L1, L2, L4}`

  * `L1` = institutional/official within `polity_id`
  * `L2` = internal counter-narrative within `polity_id`
  * `L4` = later synthesis/analytic/compilation (may be within a polity, cross-polity, or meta-polity)
* Optional but recommended:

  * `time_range` (when the narrative is in force / composed)
  * `stance_tags[]` (orthodox/heterodox, loyalist/reformist, etc.)
  * `genre_tags[]` (legal, epic, liturgical, administrative, polemical, etc.)

#### 3.2 Assertion fields

* `assertion_id`
* `narrative_id`
* `subject_refs[]` (entities referenced)
* `claim` (structured payload)
* Optional:

  * `asserted_time` (time the claim concerns)
  * `confidence` / `modality` (certain, probable, reported, allegorical, etc.)

### 4. Computed Layer (Effective Layer)

Layer classification is **relative** to a `focal_polity_id`.

Define:

* `P = narrative.polity_id`
* `N = narrative.native_layer`
* `F = focal_polity_id`

Compute `effective_layer` per assertion as:

1. **L0 (Diegetic Ground Truth)**
   Not inferred automatically. Only if explicitly marked:

   * `assertion.layer_override = L0` (or equivalent explicit flag)

2. **If P == F** (home polity)

   * `effective_layer = N` where `N ∈ {L1, L2, L4}`

3. **If P != F** (foreign polity)

   * If `N ∈ {L1, L2}` then `effective_layer = L3`
   * If `N == L4` then `effective_layer = L4` (analytic remains analytic even if foreign)

### 5. Constraints / Invariants

* `native_layer` MUST NOT be `L3`. (`L3` is only a relative/effective classification.)
* `L0` MUST be explicit; never auto-derived.
* Every assertion MUST be attributable to exactly one narrative.
* Every narrative MUST belong to exactly one home polity (use a designated meta-polity if needed).

### 6. Materialization Policy (Optional)

Implementations MAY store `effective_layer` for performance, but it must be reproducible from:

* `focal_polity_id`
* `narrative.polity_id`
* `narrative.native_layer`
* `assertion.layer_override` (if used)

### 7. Recommended Queries Supported

* “All L1 accounts within polity F”
* “All outsider perspectives (L3) on event X relative to polity F”
* “Compare L1 vs L2 within polity F over time”
* “Show L4 syntheses that reconcile conflicting L1/L2/L3 claims”

### 8. Extension Points

* Sub-layering via `stance_tags` (e.g., `christian`, `pagan`, `co-emperorship`, `centralist`, `provincialist`)
* Time-versioning via `time_range` (avoid proliferating layers)
* Domain scoping via `institution_id` (imperial court, priesthood, senate, etc.)
