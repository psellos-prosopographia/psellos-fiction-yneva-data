# Onomastics Policy

*(Normalization and Authority Control)*

## 1. Scope and Principles

This policy governs how **names are modeled, normalized, and interpreted** within the corpus.
Identity boundary decisions are governed by
[NORMALIZATION_AND_AUTHORITY_CONTROL_POLICY.md](NORMALIZATION_AND_AUTHORITY_CONTROL_POLICY.md).

### Core principles

1. **Identity is singular; names are plural.**
   An entity has one identity and may have many names.

2. **Names are assertions, not attributes.**
   Names are treated as first-class assertions with provenance, scope, and context.

3. **Names encode usage, not essence.**
   A name reflects how an entity is referred to by someone, somewhere, at some time.

4. **Surface strings do not determine meaning.**
   The same name string may represent different social functions across time or persons.

---

## 2. Names as First-Class Assertions

All names are modeled using a canonical assertion type (e.g. `named_as`).

A name assertion MAY include:

* name string
* name kind
* relation basis (if applicable)
* language or cultural context
* time scope (optional)
* provenance / attestation
* narrative or epistemic layer

Names MUST NOT be:

* hard-coded as entity fields
* treated as immutable identifiers

---

## 3. Canonical Name Kinds

The following **name kinds** are recognized as academically standard. The list is intentionally limited; precision is added via metadata, not proliferation.

### Core kinds

* **primary** — default scholarly reference form
* **birth_name** — name given at birth
* **personal_name** — given name when distinct
* **family_name / lineage_name** — hereditary designation
* **byname** — descriptive or contextual addition
* **epithet / honorific** — evaluative or laudatory name
* **title_name** — name derived from office or rank
* **regnal_name** — name used in rulership
* **religious_name / monastic_name** — name taken upon initiation
* **alias / alternative_form** — interchangeable variant
* **exonym / endonym** — outsider vs self designation
* **translated / adapted_form** — linguistic transformation

No name kind implies ontology; all are descriptive.

---

## 4. Relational Name Modeling (General Rule)

Names derived from **relationships** are modeled using a **relational basis**, not culture-specific categories.

### Relational basis

A relational name MUST specify:

* `name_kind: relational`
* `relation_basis` (e.g., `child_of`, `parent_of`, `associated_with_place`)

This subsumes:

* patronymics
* matronymics
* teknonyms
* toponymics (“of X”)
* disciple-of constructions
* saint-of / follower-of forms

### Optional cultural specificity

Culture-specific terms MAY be recorded as annotations, not required categories.

Example:

```yml
name_kind: relational
relation_basis: parent_of
cultural_term: teknonym
```

**Policy rule:**

> Relational semantics are canonical; cultural labels are optional metadata.

---

## 5. Patronymics, Teknonyms, and Similar Forms

* **Patronymics / matronymics**
  → `relation_basis: child_of`

* **Teknonyms (e.g., Arabic kunya)**
  → `relation_basis: parent_of`
  → not family names; not hereditary by default

These are modeled as **name assertions**, optionally linked to a kinship assertion.

---

## 6. Honorifics, Epithets, and Victory Names

### Earned names (e.g., *Germanicus*)

* Modeled as:

  * `name_kind: honorific`
  * subtype optional (e.g., `victory_name`, *agnomen ex virtute*)
* Basis may reference:

  * an event
  * a place
  * an achievement

These names:

* are not kinship-based
* are not hereditary by default
* do not alter identity

---

## 7. Inherited Honorifics and Name Function Change

When an originally earned name becomes inherited:

* **Do not reclassify the original usage**
* **Do not merge meanings**

Instead:

* create **distinct name assertions** with different `name_kind` and `basis`

Example pattern:

* Original bearer:

  * `name_kind: honorific`
  * `basis: achievement`

* Descendant:

  * `name_kind: family_name` (or lineage_name)
  * `basis: inheritance`

**Policy rule:**

> Changes in name function are modeled through additional assertions, not retroactive reinterpretation.

---

## 8. Titles vs Names

* Titles that **confer authority** are modeled as **offices**, not names.
* Title-derived name forms (e.g., “the Great”, “the Elder”) are modeled as:

  * `name_kind: epithet` or `title_name`
* Names do not imply office unless separately asserted.

---

## 9. Time Scope for Names

Names MAY be:

* time-scoped (when usage is period-specific)
* timeless (when time is irrelevant or unknown)

**Timeless usage is permitted** for:

* primary scholarly names
* stable kinship-based names
* logical identity markers

Names MUST NOT:

* be given artificial life-long ranges unless evidence exists

---

## 10. Provenance and Attestation

Every name assertion SHOULD have provenance.

Allowed forms:

* attested by a source (text entity)
* attested by a person or institution
* explicitly marked editorial

Editorial names (e.g., normalized scholarly forms) MUST be marked as such.

---

## 11. Narrative and Epistemic Layers

Names MAY exist in multiple narrative layers.

Examples:

* mythic epithets
* polemical names
* propagandistic titles

The same name string in different layers represents **different claims**, not different identities.

---

## 12. What Is Explicitly Forbidden

* Treating names as entity identifiers
* Encoding ontology in name strings
* Collapsing distinct name functions into one
* Retroactively reinterpreting name origins
* Using culture-specific name categories as mandatory

---

## 13. Policy Summary (Normative)

> Names are first-class assertions that record how an entity is referred to, by whom, in what context, and with what social meaning. Relational semantics take precedence over culture-specific labels. Changes in name function are modeled through multiple assertions, not identity changes.
