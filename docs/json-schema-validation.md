# JSON Schema Validation

MathJSON uses JSON Schema to enforce the structure of its dictionary
files. Two schemas validate the two core data files.

## Schema Files

| Schema | Validates | Location |
|--------|-----------|----------|
| `dictionary.schema.json` | `dictionary.json` | `schemas/` |
| `categories.schema.json` | `categories.json` | `schemas/` |

Both schemas use JSON Schema Draft 2020-12.

## Local Validation

Install the validation tool:

```bash
pip install check-jsonschema
```

Validate both files:

```bash
check-jsonschema --schemafile schemas/dictionary.schema.json dictionary.json
check-jsonschema --schemafile schemas/categories.schema.json categories.json
```

For verbose output with detailed error reporting:

```bash
check-jsonschema --schemafile schemas/dictionary.schema.json dictionary.json -v
```

Verify schemas themselves are valid:

```bash
check-jsonschema --check-metaschema schemas/dictionary.schema.json
check-jsonschema --check-metaschema schemas/categories.schema.json
```

## CI/CD Enforcement

A GitHub Actions workflow (`.github/workflows/validate-schemas.yml`)
runs schema validation automatically on:

- Every push to `main`
- Every pull request targeting `main`

If validation fails, the CI check blocks the merge and reports the
specific file and field that failed.

## Updating Schemas

When adding new fields to dictionary entries:

1. Add the field definition to the appropriate schema in `schemas/`.
2. If the field is always present, add it to the `required` array.
3. If the field is optional, add it to `properties` only.
4. Run local validation to verify existing data still passes.
5. Commit the schema change alongside the data change.

### Dictionary Entry Structure

**Operator** (required fields): `name`, `category`, `arity`,
`signature`, `associative`, `commutative`, `idempotent`, `lazy`,
`broadcastable`

**Operator** (optional fields): `description`, `wikidata`, `examples`

**Constant** (required fields): `name`, `category`, `type`

**Constant** (optional fields): `description`, `wikidata`, `value`

### Category Entry Structure

**Category** (required fields): `description`, `operators`, `constants`
