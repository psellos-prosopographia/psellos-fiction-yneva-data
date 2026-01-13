# Fictional Prosopography Repository — Initialization & Specification

## 0. Purpose and scope

This repository is the **canonical data and modeling repository** for a fictional prosopographical corpus built using the *psellos* approach to assertions, provenance, and historiography.

Its purpose is to:

* Represent **people, polities, institutions, offices, places, artifacts, and sources** as stable entities.
* Represent **all historical, legendary, analytic, and propagandistic statements** as *attributed, time-scoped assertions*.
* Support **multiple cultures, polities, and legal regimes** with differing rules (kinship, office eligibility, marriage, adoption, legitimacy, territory).
* Preserve **ambiguity, contradiction, and narrative layering** as first-class data, not errors.
* Serve as the **single source of truth** for downstream consumers (including a user-facing website), without embedding any presentation logic.

This repository **does not** implement UI, visualization, or narrative presentation. A separate user-facing website exists that consumes the outputs of this repository but is **out of scope** and **not visible or editable** here.

---

## 1. Relationship to the psellos ecosystem

This repository **conforms to and extends** the psellos ecosystem, and is governed by the principles articulated in the psellos-hub authority index:
https://raw.githubusercontent.com/psellos-prosopographia/psellos-hub/refs/heads/main/AUTHORITY_INDEX.yml

In particular:

* Assertions are first-class, attributed, and layered.
* There is **no global notion of “truth”** outside analytic assertions.
* Governance, version pinning, and architectural roles follow psellos conventions.
* This repository is a **dataset + rules + build contract**, not a fork of psellos-spec or psellos-builder unless a fundamental incompatibility is later identified.

The repository:

* **Consumes** psellos-spec semantics.
* **Produces** data artifacts compatible with psellos-builder.
* **May be visualized** by psellos-web or a derivative presentation site.

---

## 2. Canonical docs and IDs

* Canonical authoritative docs live at repository root per the psellos-hub authority index.
* The `docs/` directory is reserved for non-canonical longform notes and stubs.
* IDs are mandatory for all entities, assertions, and vocabulary terms. See [ID_POLICY.md](ID_POLICY.md).
* Local decisions are tracked in [DECISIONS/README.md](DECISIONS/README.md).

---

## 3. High-level repository pillars

The repository is organized around the following conceptual pillars:

### 2.1 Canonical entities

Stable identifiers for things the world talks about:

* persons (including disputed or composite identities)
* polities and polity phases (polity-as-process)
* institutions (religious, academic, legal, political)
* offices (roles distinct from holders)
* places and regions (with multiple names and fuzzy extents)
* artifacts and symbols (ritual, political, ethnic)
* texts and sources (epics, chronicles, laws, archaeology)

Entities are **structurally minimal**; they gain meaning through assertions.

---

### 2.2 Assertions (the core corpus)

All statements about the world—legendary, historical, analytic—are represented as assertions.

**Non-negotiable properties of every assertion:**

* asserting actor (person or institution)
* narrative/analytic layer
* time scope (explicit or explicitly timeless)
* provenance (source or method)
* optional confidence/uncertainty

Assertions are never deleted to resolve contradiction; they coexist.

---

### 2.3 Minimal core assertion operations

The repository recognizes the following **minimal core assertion operations** as irreducible:

* `attested_in_source`
* `participates_in_event`
* `holds_office`
* `related_to`
* `identified_with`
* `claims_identity_as`
* `characterized_as`

No new core assertion type may be added unless it cannot be expressed as a composition of the above and recurs across domains.

---

### 2.4 Controlled vocabularies

Specific meaning is pushed into vocabularies, not schema:

* relation-type metadata (kinship, political ties, office tenures, claims)
* event types and participant roles
* classification labels (periodization, regime types, historiographical judgments)
* office incompatibilities and exclusivities (as metadata, not truth)

Vocabularies define *what a relation means*, not whether it is valid in all cultures.

---

### 2.5 Scoped rule regimes (culture- and polity-specific)

Cultural and legal variation is handled through **scoped rule regimes**, not schema forks.

Rule regimes may be scoped to:

* a polity phase (preferred)
* an institution
* both

Rule regimes may define:

* marriage norms (monogamy, termination at death)
* adoption revocability
* office eligibility requirements
* succession constraints
* territorial norms
* ritual prerequisites for legitimacy

Assertions reference a regime context when relevant.
If context is absent, validation must be permissive.

---

### 2.6 Sources and narrative layers

The repository explicitly models:

* sources as entities
* types of sources (epic, law, inscription, archaeology, later historian)
* narrative layers (legendary, propagandistic, institutional orthodoxy, outsider, analytic/editorial)

This enables:

* competing interpretations
* institutional bias
* later reinterpretation of earlier material

---

### 2.7 Territory, borders, and disputes

Territory is modeled as **entities + claims + events**, not static ownership.

* Regions, marches, and zones are entities.
* Control and borders are asserted via claims.
* Change occurs through events (conquest, treaty, inheritance).
* Disputes are represented as parallel, contradictory assertions.

There is no single authoritative border unless asserted analytically.

---

## 3. Office holding and eligibility

Offices are neutral entities.
Holding an office is a **time-scoped assertion**.

Eligibility requirements:

* are not properties of the office itself
* live in scoped rule regimes
* are evaluated against existing assertions (elections, coronations, lineage, recognition)
* may be partially met, ignored, or contested

Failure to meet a requirement does **not** invalidate office holding; it generates legitimacy tension.

---

## 4. Temporal rules

* Time belongs to assertions, not entities.
* Assertions may overlap.
* Death terminates roles and contracts only if the relation type or regime defines it.
* Kinship and ancestry persist beyond death.
* Open-ended intervals are allowed when historically appropriate.

---

## 5. Repository invariants

The following invariants must hold:

1. No un-attributed assertions.
2. No silent overwriting of prior claims.
3. No culture-specific rules baked into global vocabularies.
4. No presentation logic in the data repository.
5. No “truth fields” outside analytic assertions.

---

## 6. Non-goals

This repository explicitly does **not**:

* decide historical truth
* enforce a single canonical narrative
* encode UI structure or storytelling order
* collapse myth and history into one layer
* replace psellos governance or specification

---

## 8. Expected repository structure (conceptual)

```
/entities
  /person
  /polity
  /institution
  /office
  /place
  /artifact
  /text
  /source

/assertions
  <entity_id>.yml

/vocabularies
  relation_types.yml
  event_types.yml
  roles.yml
  classifications.yml

/rule_regimes
  polity_phase_rules.yml
  institutional_rules.yml

/build
  build_contract.yml
  version_pins.yml

/docs
  GOVERNANCE.md
  MODEL_NOTES.md
  DECISIONS.md
```

Exact filenames are not mandated yet; structure is conceptual.

---

## 9. User-facing website acknowledgment

A separate user-facing website exists to explore, visualize, and narrate this corpus.

* This repository does not depend on the website’s implementation.
* The website must consume built artifacts from this repository.
* No assumptions are made here about routing, UI metaphors, or narrative flow.

---

## 10. Governance and decision-making

All modeling decisions that:

* change assertion semantics
* alter core vocabularies
* introduce new rule regimes
* reinterpret prior data

must be documented as explicit decisions (ADR-style), with rationale and scope.

---

## 11. Bootstrap files

On initialization, the repository should include:

* `README.md` (this document, adapted)
* `AGENTS.md`

  * defines roles for automated or human agents (e.g. data entry, review, validation)
* `DECISIONS/README.md` (initially empty)
* `MODEL_NOTES.md` (living design commentary)
* `LICENSE.md` containing the full text of CC BY-NC-SA 4.0.
* `.gitignore` (excluding build artifacts, caches, editor files)

---

## 12. Final principle

> This repository models **how worlds are argued about**, not just what happened in them.

If future work adheres to that principle, it will remain coherent even as scope and complexity grow.
