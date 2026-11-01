# OptionKit 0.1.0

A small strict command-line option schema for Buoy. `Import optionkit` loads the
source or sealed package. There are no native dependencies.

## API

- `New OptionSchema()` creates a reusable schema.
- `schema.AddFlag(name, shortName = "", description = "")` registers a Boolean presence flag.
- `schema.AddValue(name, shortName = "", required = False, description = "", valueName = "VALUE")` registers a String value.
- `schema.AddDefaultValue(name, defaultValue, shortName = "", description = "", valueName = "VALUE")` registers an optional value with a fallback.
- `schema.SetDefault(name, value)` changes the fallback of an existing optional value option; flags and required options reject.
- `schema.Usage(programName, positionalUsage = "")` and `Help(programName, summary = "", positionalUsage = "")` return text without printing or exiting.
- `schema.Parse(arguments, maxArguments = 10000)` returns a fresh `OptionResult`.
- `result.Has(longName)` reports supplied presence; `Value(longName)` returns the supplied
  String (flags yield `"true"`) and throws when absent.
- `result.HasDefault(name)` identifies fallback registration; `ResolvedValue(name)` returns a supplied value first, then a fallback, or throws.
- `result.IntegerValue(name, minimum = Int64 minimum, maximum = Int64 maximum)` and `DoubleValue(name, minimum = -Double max, maximum = Double max)` convert resolved values with checked bounds.
- `result.Positionals()` returns a defensive copy of all positional arguments.

Pass user arguments **without argv[0]**. Buoy includes the executable name in
`Main(args)`; the example (not included in this repository) skips
it explicitly. Parsing is separate from argument acquisition and has no stdout,
stderr, process-exit, environment, signal or filesystem side effects.

Long names start with an ASCII letter and continue with ASCII letters, digits or
hyphens. Short aliases are one ASCII letter. Names and aliases are case-sensitive;
duplicate long names or short aliases are rejected during registration.

Accepted forms are `--verbose`, `-v`, `--output file`, `--output=file`, `-o file`
and `-o=file`. Empty explicit values (`--output=`) are valid. A value option
consumes its next token verbatim, including `-12` or `--`; to use a subsequent
`--` terminator, first finish supplying the value. Positionals can be interspersed
with options; `--` ends option recognition and a lone `-` is positional.

Unknown options, repeated options (including the same option via its alias),
missing values, absent required options, values on flags and grouped/attached
short forms (`-vv`, `-ofile`) throw `InvalidArgumentException`. These deliberate
rules avoid implicit last-value-wins behavior and short-option ambiguity. A
schema remains reusable after failed parsing. Values stay Strings; the optional
numeric accessors validate on access, while date/path validation belongs to the caller. Input argument count is bounded; callers
remain responsible for limiting total argument bytes and schema size if they
accept untrusted synthetic arrays.

Results snapshot their inputs. `New OptionResult(names, values, positionals)` is
also available for constructing a result; it validates equal name/value lengths
and duplicate names, and clones all arrays. This constructor does not apply schema
name rules or required-option validation.

## Verification and packaging


The runner seals a fresh package, checks the module, tests and example for all
six desktop architecture/platform targets, and executes both sealed-package
consumers on the host. It also invokes the example with Unicode, a terminator and
an invalid option. Builds use a temporary tree to avoid unrelated resource
manifests in the repository.

`BPM_PACKAGER` selects the package command; it defaults to `/usr/bin/buoy` because
the installed Linux compile wrapper does not handle `pkg`. `BUOY` selects the
compiler. The local distributable is `packages/optionkit-0.1.0.buipkg`; no remote
registry publication has occurred.

## Help, defaults and numeric access

Help lists options in registration order with aliases, value labels, required
markers, quoted defaults and descriptions. Unicode descriptions are preserved;
backslash, quote, CR/LF and tab in labels/descriptions/defaults are escaped rather
than inserting extra help lines. Value labels contain 1..64 ASCII letters, digits,
underscore or hyphen. No automatic `--help` option is registered; applications
choose whether and when to display the returned text. Help output is plain text,
without terminal-width wrapping or ANSI styling.

Defaults never satisfy required presence, change `Has`, or make `Value` return an
unsupplied value. Required options cannot have defaults. Fallbacks are copied into
each parsed result, so later `SetDefault` calls do not alter earlier results.
An explicit empty supplied value overrides a fallback. Numeric defaults are
validated when converted, not while defining the schema.

`IntegerValue` accepts an optional leading ASCII `+`/`-` followed by decimal digits
(up to 20 characters total); it rejects whitespace, fractions, exponent notation,
non-ASCII digits and overflow before arithmetic can wrap. Both Int64 endpoints are
supported. It deliberately avoids the installed runtime's `Integer.Parse`
overflow behavior. `DoubleValue` accepts signed decimal forms such as `.5`, `1.`,
`-1.25e+3`; whitespace, hexadecimal syntax, suffixes, NaN, infinity and overflow
reject. Underflow follows `Double.Parse` and may round to signed zero. IEEE rounding
still applies to finite values. Conversion/bounds errors throw
`InvalidArgumentException`; no schema/result state is changed.

`result.WithDefaults(names, values)` also returns a new independent result with
validated unique fallback names. It replaces fallback metadata rather than
mutating the receiver and does not apply schema-required/flag rules. Those rules
are enforced by `OptionSchema`.


The additional verifier runs 18 six-target checks, a Python integer oracle covering
205 Int64 values, three sealed-package executions and the original OptionKit
verification suite. See the help/default example (not included in this repository).

Test suites and fixtures are maintainer-local and are not part of this public repository.
