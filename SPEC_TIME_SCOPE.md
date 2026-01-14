# SPEC: Time scope encoding

This document is normative for encoding temporal scope on assertions.

It aligns with `NORMALIZATION_AND_AUTHORITY_CONTROL_POLICY.md` §7.

## Time belongs to assertions

- Time scope is encoded on assertions, not on entities.
- Assertions may be timeless when time is not meaningful or not assertable.

## Field name

The field is named `time_scope` and, when present, has the following shape:

```yml
time_scope:
  qualifier: exact
  start_year: 740
  end_year: 744
```

## Allowed keys

- `qualifier` (required)
- `start_year` (optional integer)
- `end_year` (optional integer)

If both years are present, `start_year` MUST be <= `end_year`.

## Allowed qualifiers

- `exact`
- `circa`
- `before`
- `after`
- `terminus_post_quem`
- `terminus_ante_quem`

## Mechanical rules by qualifier

- `exact`
  - requires `start_year`, optionally `end_year`
  - if `end_year` is present, it encodes an exact closed interval

- `circa`
  - requires at least one of `start_year` or `end_year`
  - if both are present, they bound the uncertainty range

- `before`
  - requires `end_year`
  - forbids `start_year`

- `after`
  - requires `start_year`
  - forbids `end_year`

- `terminus_post_quem`
  - requires `start_year`
  - forbids `end_year`

- `terminus_ante_quem`
  - requires `end_year`
  - forbids `start_year`

## Examples

### Exact year

```yml
time_scope:
  qualifier: exact
  start_year: 812
```

### Exact range

```yml
time_scope:
  qualifier: exact
  start_year: 812
  end_year: 814
```

### Circa

```yml
time_scope:
  qualifier: circa
  start_year: 812
  end_year: 814
```

### Before / after

```yml
time_scope:
  qualifier: before
  end_year: 900
```

```yml
time_scope:
  qualifier: after
  start_year: 900
```

### Terminus post quem / ante quem

```yml
time_scope:
  qualifier: terminus_post_quem
  start_year: 740
```

```yml
time_scope:
  qualifier: terminus_ante_quem
  end_year: 744
```

## Timeless assertions

If an assertion is timeless, omit `time_scope` entirely.
Do not encode “timeless” as a qualifier.
