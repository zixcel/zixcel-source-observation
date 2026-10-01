# zixcel-source-observation

`zixcel-source-observation` is the vendor-neutral, read-only boundary for observing
local folders and Office Open XML workbooks. It does not interpret project meaning,
retain credentials, modify the observed source, or decide whether an observation is
accepted by a project.

The caller supplies an absolute allow-listed root and a relative source path. The
package rejects traversal, symbolic links, special files, unsupported workbook
formats, oversized input, and unsafe ZIP/XML structures. Workbook cell values,
formulas, and labels are not returned by default.

```bash
zixcel-source-observation workbook observe \
  --root /absolute/read-only/root \
  --source relative/path/to/book.xlsx
```

The JSON result is a bounded observation receipt. A caller binds the operation to its project procedure and decides how to use the receipt.

Large source repositories may declare `maximum_entries` (up to 65535) and an
`excluded_directory_names` list in the private resource configuration. Exclusions
are exact directory-name components, are included in the observation digest, and
are intended for generated trees such as `.git`, `node_modules`, or `build`.
The observer never guesses exclusions or removes the entry bound.

For an installed caller resource, a machine-owned local configuration maps the
logical resource reference to its allowed root and relative source. The receipt
does not expose either machine path:

```bash
zixcel-source-observation resource observe \
  --configuration /absolute/private/source-observation.json \
  --resource-ref resource/example/workbook
```

## Package integration

The package is an independently consumable unit. Callers reference its documented
interface through a versioned dependency and own application-specific composition
and integration.
