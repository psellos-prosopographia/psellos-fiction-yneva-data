# Assertion-first corpus conversion guide (LLM-assisted)

This guide defines a **manual, LLM-assisted conversion workflow** for taking a corpus of natural
language assertions or narrative text and turning it into psellos- and yneva-data compliant
assertions. It complements the [ASSERTION_FIRST_PIPELINE.md](ASSERTION_FIRST_PIPELINE.md) alternate
entry point and does **not** imply any deterministic tooling.

## When to use this guide

Use this guide when the intended workflow is **LLM-assisted conversation** (e.g., an agent like
ChatGPT) rather than scripted parsing. The goal is consistent conversion outcomes and clear
criteria for accepting or rejecting a proposed conversion.

## Required inputs (from the user)

An LLM conversion attempt must have enough concrete content to produce assertions with confidence.
At minimum, the user should provide:

1. **Narrative scope**
   * Timeframe, polity/region, and narrative layer (or a placeholder canon name that can be mapped
     to an existing `L_LAYER` or added to `L_LAYER_CATALOGUE.md`).
2. **Entity anchors**
   * Named persons, places, offices, institutions, events, and/or sources-to-be that can be turned
     into entities, or enough detail to create new entities.
3. **Assertion candidates**
   * The natural-language statements that are to be converted, with enough specificity to identify
     who did what, where, and when (or explicitly timeless where appropriate).
4. **Provenance posture**
   * Whether the assertions are **synthetic**, **editorial**, or backed by a source-to-be.

If any of these are missing or too vague, the conversion **must be rejected** with a report.

## Conversion workflow (LLM-assisted)

1. **Scope intake and layer mapping**
   * Confirm narrative scope and map the narrative layer to an existing `L_LAYER` or add a
     placeholder to `L_LAYER_CATALOGUE.md`.
2. **Entity preparation**
   * Create or extend entities needed for the assertions (persons, places, offices, events,
     sources-to-be, institutions).
3. **Assertion drafting**
   * Encode statements using the canonical assertion shape and core assertion operations.
4. **Provenance and time scoping**
   * Attach provenance (synthetic/editorial or source-to-be) and explicit time scopes.
5. **Validation checks (manual)**
   * Ensure each assertion has: attribution, layer, time scope, provenance, and structured
     identifiers.

## Rejection policy (required)

If the provided corpus or instructions do not supply enough semantic or concrete content to produce
assertions **with confidence**, the conversion must be **rejected**. The rejection response should
include:

* **A brief report of missing information**, such as:
  * missing narrative layer or L_LAYER mapping
  * missing or ambiguous entities
  * missing time scope or location
  * unclear action/relationship (who did what)
  * missing provenance posture
* **A short, actionable request** for the user to provide the missing details.

This rejection is not a failure state; it is an explicit feedback loop to ensure the resulting
assertions are trustworthy and consistent with dataset requirements.

## Relationship to the assertion-first pipeline

This guide operationalizes the assertion-first pipeline for LLM-assisted workflows while adhering
fully to the assertion shape, provenance rules, and narrative layer requirements. It does not
replace the canonical specs; it only describes how an agent should apply them.
