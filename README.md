# Parse command-line flags, values and positionals

A small strict command-line option schema for Buoy. `Import optionkit` loads the
source or sealed package. There are no native dependencies.

## Install

From a Buoy consumer project with BPM installed:

```sh
bpm add optionkit --git https://github.com/BuoyBay/optionkit.git --rev refs/heads/main
```

BPM records the resolved commit in its lockfile. Keep that lockfile with the
consumer project for reproducible installs.

## Example

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

## Build from source

With the native `build-packages` driver from [devtools](https://github.com/BuoyBay/devtools)
on your `PATH`, run from this checkout:

```sh
build-packages --root "$PWD" --compile-consumer optionkit
```

The driver checks source targets and compiles an import consumer. See the
[API guide](docs/API.md) for behavior, limits and any host prerequisites.

## Local Buoy setup

On your own machine, install Buoy and run:

```sh
buoy ai init --both
```

The generated agent files and editor settings stay local and are ignored by Git.

## Contributing

[Contributing](CONTRIBUTING.md) · [License](LICENSE)
