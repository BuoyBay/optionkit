---
title: "OptionKit API"
summary: "Reusable schemas, detached parse results, exact option syntax, and strict errors."
category: "Reference"
order: 20
keywords: ["OptionSchema", "OptionResult", "required", "alias", "terminator"]
symbols: ["OptionSchema", "OptionSchema.AddFlag", "OptionSchema.AddValue", "OptionSchema.AddDefaultValue", "OptionSchema.SetDefault", "OptionSchema.Help", "OptionSchema.Usage", "OptionSchema.Parse", "OptionResult", "OptionResult.Has", "OptionResult.Value", "OptionResult.ResolvedValue", "OptionResult.IntegerValue", "OptionResult.DoubleValue", "OptionResult.Positionals"]
related: ["index.md", "getting-started.md", "typed-api.md"]
---

# API reference

## Schema and result

| Member | Contract |
| --- | --- |
| `New OptionSchema()` | Empty reusable schema |
| `AddFlag(name, shortName = "", description = "")` | Register a Boolean presence flag |
| `AddValue(name, shortName = "", required = False, description = "", valueName = "VALUE")` | Register a String value, optionally required |
| `AddDefaultValue(name, defaultValue, shortName = "", description = "", valueName = "VALUE")` | Register an optional value with fallback metadata |
| `SetDefault(name, defaultValue)` | Set an optional value's fallback; flags and required options reject |
| `Usage(programName, positionalUsage = "")`, `Help(programName, summary = "", positionalUsage = "")` | Return ordered plain text without printing or registering --help |
| `Parse(arguments, maxArguments = 10000)` | Parse user arguments without argv[0]; return a fresh `OptionResult` |
| `OptionResult.Has(longName)` | Presence check using the canonical long name |
| `OptionResult.Value(longName)` | Supplied String; flags yield `"true"`; absent names throw |
| `HasDefault(name)`, `ResolvedValue(name)` | Inspect a copied fallback or read supplied value before fallback |
| `IntegerValue(name, minimum, maximum)`, `DoubleValue(name, minimum, maximum)` | Convert the resolved value with explicit grammar, finite/range checks |
| `WithDefaults(names, values)` | New result with copied fallback metadata; original is unchanged |
| `OptionResult.Positionals()` | Detached array of positional Strings |
| `New OptionResult(names, values, positionals)` | Construct a copied result after equal-length/duplicate-name validation |

Long names begin with an ASCII letter, followed by ASCII letters, digits, or hyphens. Short aliases are one ASCII letter. Matching and uniqueness checks are case-sensitive. The direct result constructor checks array lengths and duplicate names but does not apply schema name rules or required-option validation.

## Accepted input forms

| Kind | Forms |
| --- | --- |
| Flag | `--verbose`, `-v` |
| Long value | `--output file`, `--output=file`, `--output=` |
| Short value | `-o file`, `-o=file` |
| Positional | Ordinary tokens, lone `-`, or any token after standalone `--` |

Positionals can appear between options. The next token after a value option is consumed verbatim, including `-12` or `--`. Empty explicit values are valid. Results use the long name regardless of which alias appeared.

## Rejection rules and bounds

`InvalidArgumentException` reports unknown options, repeat occurrences through either name/alias, missing values, missing required options, values supplied to flags, grouped/attached short forms such as `-vv` or `-ofile`, invalid names, duplicate registration, and exceeded/nonpositive argument-count limits.

A schema remains reusable after parse failure. Values remain Strings; numeric
validation runs only when an accessor is called. Integer conversion checks the
full signed-64 decimal range before arithmetic can wrap. Double conversion
rejects whitespace, non-decimal syntax, NaN, infinity and overflow; finite IEEE
rounding and underflow-to-zero still apply. Bounds/conversion failures do not
mutate schema or results.

Defaults never change supplied-only Has/Value behavior or satisfy required
presence. Results snapshot fallbacks, so later schema changes cannot alter an
existing result. Help preserves registration order, descriptions and aliases and
escapes text controls; it does not wrap to terminal width. The parser has no
environment lookup, filesystem validation or process-exit side effect. Keep
synthetic argument byte totals and schema size bounded in the host.

See [Getting started](getting-started.md) or the [overview](index.md).


[Full typed API: properties, parameters, defaults, and returns](typed-api.md)
