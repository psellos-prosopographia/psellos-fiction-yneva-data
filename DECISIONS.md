# Decisions (procedure)

This document defines how to create and track local decision records. Constitutional authority and
precedence are defined in [GOVERNANCE.md](GOVERNANCE.md).

## Where decisions live

* Decision records live in the `DECISIONS/` directory as individual markdown files.
* `DECISIONS/README.md` serves as the index of recorded decisions.

## When a decision is required

A decision record is required for:

* changes to assertion semantics,
* changes to vocabularies or rule regimes,
* structural changes to canonical data layout or policy boundaries.

## Decision file format

Each decision file should include, at minimum:

* **Title** (short descriptive name)
* **Date** (YYYY-MM-DD)
* **Status** (proposed/accepted/superseded)
* **Context** (why the decision is needed)
* **Decision** (what was decided)
* **Consequences** (impacts and follow-up work)

## Recording workflow

1. Create a new markdown file in `DECISIONS/` using the format above.
2. Add the decision to the index in `DECISIONS/README.md`.
3. If the decision changes canonical policy boundaries or schemas, update the relevant policy files
   and note the decision ID in commit or change logs as appropriate.
