---
title: "OptionKit getting started"
summary: "Register a CLI schema and skip the executable name before parsing Buoy arguments."
category: "Guides"
order: 10
keywords: ["argv", "verbose", "output", "positionals", "parse"]
related: ["index.md", "api.md", "typed-api.md"]
---

# Getting started

Install `optionkit` through BPM or copy `optionkit-0.1.0.buipkg` into the consumer's `packages/`. Add `Import optionkit`.

## Parse user arguments

Buoy includes the executable name in `Main(args)`. Skip `args(0)` before passing input to `OptionSchema.Parse`. This complete program accepts `--verbose`/`-v`, `--output`/`-o`, and positional input names.

```buoy
Import optionkit

Sub Main(args As Array Of String)
    Dim schema As New OptionSchema()
    schema.AddFlag("verbose", "v")
    schema.AddValue("output", "o")
    Dim userArgs() As String
    For i As Integer = 1 To Array.Count(args) - 1
        Array.Append(userArgs, args(i))
    Next
    Dim parsed As OptionResult = schema.Parse(userArgs)
    Print "Verbose: " + String.From(parsed.Has("verbose"))
    If parsed.Has("output") Then Print "Output: " + parsed.Value("output")
    For Each positional As String In parsed.Positionals()
        Print "Input: " + positional
    Next
End Sub
```

For example, pass `--verbose --output=result.txt input.txt`. A positional filename starting with a dash can follow a standalone `--`. Use `--output=` for an explicitly empty value; presence and empty value are distinct.

## Separate parsing from validation

`Has("output")` checks whether an option was supplied. `Value("output")` returns a String or throws when absent. Flags return the String `"true"` through Value, but testing Has is usually clearer.

Application code must validate path existence, numeric ranges, allowed encodings, or other domain meanings after parsing. Catch `InvalidArgumentException` at the application boundary to choose your own usage/error text and exit behavior.

A value option consumes its next token unchanged. In `--output --`, the second token is the output value, not a terminator. Finish the value before supplying a later `--` when you need both behaviors.

Continue with [API reference](api.md), or return to [OptionKit](index.md).
