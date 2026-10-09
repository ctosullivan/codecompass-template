# Optional: intermediate knowledge layer

**An honesty note before anything else:** no template files for this
directory were available in the evidence used to write this
documentation. What follows is grounded in the directory's name and its
position in the scaffold relative to the other two knowledge-adjacent
places in this template — not in any actual file content inspected
inside it. Don't treat this page as a spec; treat it as a placeholder
description until real templates exist here to document properly.

## What this is for, as best can be told

This template already has two knowledge-adjacent mechanisms at
different weights:

- `planning/knowledge/` — an informal running log (candidate → reviewed
  → promoted/retained/discarded), good for "things learned," with no
  requirement for evidence citations or structured fields.
- `optional-clean-room-workflow/assertions/` + `snapshots/` — a heavy,
  fully-specified apparatus: one record per claim, with a required
  evidence citation, a basis classification, a status lifecycle, and a
  frozen, versioned, hash-checked bundling format.

`optional-intermediate-knowledge/` sits, by name, **between** those two
— presumably for a project that wants to capture structured facts about
its own domain or codebase (more durable and more checkable than an
informal log) without taking on the full clean-room verification
pipeline (frozen snapshots, independent reconstruction, mechanical
isolation).

## What's not known

Whether this directory ships its own template file(s), what fields they
have, whether it has any relationship to `planning/knowledge/` beyond
sitting at an adjacent weight, and whether it's meant to be a stepping
stone toward later adopting the full clean-room workflow or a permanent
standalone choice — none of that is answered by the evidence available
here.
