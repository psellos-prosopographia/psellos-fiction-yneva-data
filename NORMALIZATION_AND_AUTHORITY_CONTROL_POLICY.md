# Normalization and Authority Control Policy
**Status:** normative (prescriptive)  
**Effective date:** 2026-01-13  
**Governs:** `psellos-fiction-yneva-data` (Yneva corpus)  
**Compliance targets:** Psellos governance model; yneva-data modeling constraints  

---

## 0. Authority and precedence

1. **Repository authority.** This policy is authoritative only when stored in-repo under the repository’s governance rules and referenced by the Psellos authority chain.
2. **Governance precedence.** If any clause here conflicts with the Psellos ecosystem authority index, the authority index prevails.
3. **Non-authoritative artifacts.** Generated content outside the repository (including conversational outputs) is non-authoritative unless incorporated via the repository’s recorded workflow.
4. **Retroactive revision.** Canonical records and rules may be revised **only with explicit versioning** and changelog entries; silent revisions are forbidden.

---

## 1. Scope

1. This policy governs **normalization** and **authority control** for the **Yneva fictional prosopography dataset**, implemented under the Psellos model.
2. It specifies:
   - what may become a **canonical identity**,
   - how canonical IDs are minted and stabilized,
   - how assertions are normalized (atomicity, directionality, time scope, provenance),
   - how contradictions and narrative layers are preserved.

---

## 2. Canonical entity ontology

### 2.1 Permitted canonical entity types
The following entity types are permitted as **canonical identities**:

- `person`
- `place`
- `polity`
- `polity_phase`
- `office`
- `ethnos`
- `group` (of persons)
- `event`
- `source`
- `institution`
- `religion`

No entity types are explicitly forbidden; prohibition is implemented through validation rules.

### 2.2 Type reinterpretation over time
An entity’s interpreted category (e.g., “settlement”, “region”, “cult center”, “legendary figure”) is expressed through **assertions**, not schema forks. Reinterpretation over time is permitted.

---

## 3. Authority control: identity decisions

### 3.1 Default identity presumption
If two mentions share a name, they are presumed to refer to the **same entity**, unless evidence demonstrates incompatibility that cannot be resolved through assertions or narrative layering.

### 3.2 When to split entities
Entities with the same name are split only when at least one is true:

- **Logical incompatibility:** mutually exclusive attributes that cannot coexist within any narrative layer.
- **Chronological incompatibility:** time-scoped assertions that cannot be reconciled via ranges/qualifiers.
- **Source-explicit differentiation:** a source explicitly distinguishes two distinct referents.
- **Unresolvable role/lineage conflict:** the combination of offices, kinship, and memberships yields a contradiction across all plausible reconciliations.

Splitting should be treated as a last resort; prefer **layering**, **time scoping**, and **source attribution** first.

### 3.3 Legendary/mythic agents
Fictional, legendary, or mythic figures **may be canonical entities**. Their ontology (e.g., divine, legendary, disputed) is represented via assertions and narrative layers.

### 3.4 Unnamed but necessary entities
Unnamed but necessary entities are allowed via **placeholder canonical IDs**, with minimum required assertions to support referential integrity (e.g., “Unknown mother of X”).

### 3.5 Entities that exist only to support assertions
Entities may exist solely to support assertions (e.g., an attestation-only source entity, or a minimally defined group), provided they meet validation requirements (ID + type + required fields).

---

## 4. Identifier (ID) policy

Canonical ID syntax, prefix rules, redirects, and immutability guarantees are defined in
[ID_POLICY.md](ID_POLICY.md). Normalization decisions that determine identity boundaries or
disambiguation should be recorded via the repository’s decision process.

---

## 5. Normalization rules by domain

## 5.1 Places
1. `place` is the only geographic base entity type; continents, regions, settlements, and landmarks are modeled as `place`.
2. Settlement categories (city, fortress, village, etc.) are **assertions**, not entity subtypes.
3. Containment/association relations are not restricted to place→place; geographic modeling may relate places to polities and other entities when warranted.
4. Borders/frontiers are modeled **only via assertions** (e.g., treaties, claims, cartographic reconstructions), never as intrinsic immutable place properties.

## 5.2 Polities and polity phases
1. Polities are distinct from places.
2. `polity_phase` is modeled as a **separate entity**, linked to the parent polity by assertions.
3. A polity is not required for governance assertions; governance can be asserted without forcing a polity assignment.
4. Movements (e.g., crusades) may be represented as event/institution/polity depending on narrative layer and modeling need; normalization must preserve this as layer-scoped assertions rather than schema branching.

## 5.3 Offices, titles, roles
1. Offices are abstract entities (`office`) and persist as institutional continuities when sources treat them as continuous.
2. Ad-hoc, brief, unofficial roles (e.g., “leader of a revolt”) are modeled as **predicate-level roles** (`acts_in_role`) rather than minting new office entities.
3. Multiple simultaneous office holdings are permitted.
4. “Supernatural/charismatic offices” are handled as **assertions only** unless they meet all promotion criteria:
   - named,
   - persistent across time,
   - transmissible (held by multiple agents),
   - treated as institutionally continuous by sources.

---

## 6. Assertion normalization

### 6.1 Canonical assertion inventory
The canonical assertion forms include (non-exhaustive list, but normative for current corpus):

- `named_as`
- `identified_as`
- `related_to` (person)
- `located_in` (place)
- `associated_with` (place)
- `holds_office`
- `acts_in_role` (predicate-level role)
- `member_of` (group or institution)
- `participates_in` (event)
- `attested_by` (person/source/institution)
- `ascribed_attribute` (eligible for promotion to `office` if institutionalized)
- `ascribed_by` (person/source/institution)

New assertion types are allowed **only by editorial approval** recorded in governance-tracked decisions.

### 6.2 Atomicity
Assertions must be **atomic**: one claim per assertion. Composite claims must be decomposed unless decomposition would destroy necessary semantics.

### 6.3 Multi-party assertions
Multi-party assertions are allowed but discouraged. Normalization should attempt decomposition first; retain a multi-party form only as a last resort.

### 6.4 Directionality
All assertions are **directional**. Symmetric relations (e.g., siblings) are represented by **two assertions**, one in each direction.

---

## 7. Time scoping and uncertainty

### 7.1 Time scope representation
Time is represented using start/end bounds plus an explicit qualifier:

- `exact`
- `circa`
- `before`
- `after`
- `terminus_post_quem`
- `terminus_ante_quem`

Example shape:
```yml
time_scope:
  start_year: 740
  end_year: 744
  qualifier: circa
```

### 7.2 Uncertainty
Uncertainty is encoded via bounds and qualifiers. Avoid inventing precision.

### 7.3 Timeless assertions
Timeless assertions are permitted when time is not meaningful or not assertable (e.g., `child_of`). Use time bounds whenever time matters.

---

## 8. Narrative and epistemic layers

### 8.1 Layer catalog
Narrative layers are controlled by the repository’s layer classification specification and layer catalogue.

### 8.2 Cross-layer duplication
The same propositional content may appear in multiple layers. In that case:
1. Duplicate the assertion record.
2. Set distinct `narrative_layer`.
3. Adjust provenance (e.g., distinct `attested_by`) as appropriate.

### 8.3 Conflicts
Conflicting assertions **coexist**. Normalization must not force resolution by rank-ordering authority unless the model explicitly provides a ranking mechanism (by separate, explicit assertions).

### 8.4 Mythic/religious claims
Mythic or religious claims are modeled as assertions and placed in appropriate narrative layers. Do not convert them into “facts” by removing layer/provenance markers.

---

## 9. Provenance and source control

### 9.1 Mandatory provenance
Provenance is mandatory for canonical assertions.

### 9.2 Allowed provenance forms
Provenance may be expressed as:
- references to a `source` entity (preferred),
- citation strings as a temporary fallback (must be replaceable),
- other repository-defined mechanisms, if standardized.

### 9.3 Editorial (unsourced) assertions
Assertions without explicit sources are permitted only when they are:
- necessary for structural normalization (e.g., containment scaffolding),
- products of scholarly synthesis or cartographic reasoning,
- explicitly marked as **editorial**,
- explicitly *not* claiming source attestation.

---

## 10. Names, epithets, language (onomastics)

Name form, transcription, language handling, and attestation rules are governed by
[ONOMASTICS_POLICY.md](ONOMASTICS_POLICY.md). Normalization uses names only as evidence for identity
decisions and ID disambiguation; it must not collapse distinct name forms or delete variants.

---

## 11. Canonization workflow

### 11.1 Trigger
Canonization is triggered by **editorial approval** under the repository’s workflow.

### 11.2 Change tracking
All canonical changes are tracked by version (policy versions, data versions, and/or decision records as defined by governance).

### 11.3 Multiple canons
Multiple canons may exist in a limited and explicitly marked manner (e.g., alternate histories). They must be separated by narrative layer and/or explicit canon markers defined by governance; do not fork schema.

---

## 12. Enforcement and validation

1. Policy is enforced by **schema validation**.
2. When data violates policy, it is **flagged** for editing and re-validation; it is not silently corrected.
3. Automated normalization may propose changes, but any transformation that changes semantics must be reviewable and recorded.

---

## 13. Rationale visibility
Rules are stated without mandatory embedded rationales. Rationale may exist in decision logs where required by governance.
