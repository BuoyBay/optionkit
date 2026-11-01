---
title: "OptionKit"
summary: "Parse explicit command-line schemas into immutable flags, String values, and positionals."
category: "Overview"
order: 0
keywords: ["cli", "options", "flags", "arguments", "schema"]
related: ["getting-started.md", "api.md", "typed-api.md"]
---

# OptionKit

OptionKit is a strict schema-based command-line parser in pure Buoy. Register flags and String-valued options on a reusable `OptionSchema`, then parse a user-argument array into a detached `OptionResult`. It has no native dependencies.

Parsing is separate from acquiring arguments. Help and Usage return plain text;
the application decides when to display it. Optional descriptions, defaults and
checked numeric accessors supplement the original String-valued parser. The
package does not write stdout/stderr, exit the process, consult the environment,
change files or choose application-specific date/path policy.

## Explicit option rules

Long names and optional one-letter short aliases are case-sensitive. Both names and aliases must be unique. Unknown or repeated options reject instead of silently accepting a final value. Grouped/attached short forms are deliberately unsupported.

Positionals may be interspersed with options. A lone `-` is positional; `--` ends option recognition. Value options consume their next token verbatim, even if it looks like another option or a terminator. This makes negative values possible without guessing their type.

## Ownership and limits

Parsed results snapshot names, values, and positionals. `Positionals()` returns a defensive copy. The schema remains reusable after a failed parse. Parsing bounds argument count, but callers remain responsible for total argument bytes and schema size when they accept synthetic untrusted arrays.

Read [Getting started](getting-started.md) for the argv convention and a complete CLI, or [API reference](api.md) for accepted syntax and failures.
