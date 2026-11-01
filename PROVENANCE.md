# Source study and intentional differences

This is an independent Buoy implementation of a standard schema-based CLI
parser, informed by the functionality of two projects listed in
[Xojo's open-source catalog](https://documentation.xojo.com/resources/extras/open_source_projects.html).
No upstream implementation or license text is copied into this package.

Repositories inspected on 2026-10-31:

| Project | Inspected commit | Upstream license |
| --- | --- | --- |
| [Jeremy Cowgar's OptionParser](https://github.com/jcowgar/xojo-option-parser) | `c1f752085f8c577ad909f1b07ef1b9fe0d7c60f3` | LGPL v3, `LICENSE.md` |
| [Greg O'Lone's XojoConsoleOptionParser](https://github.com/stretch327/XojoConsoleOptionParser) | `ac2c0abcde04be76c959eab5028ed5e1e391bde8` | MIT, copyright 2018 Greg O'Lone |

Inspected the Cowgar README, OptionParser XML class declarations/argument
expansion implementation and option test declarations; inspected the O'Lone
README, Params class and AllowedOption declarations. Both provide named long and
short options. O'Lone's AddOption also includes required flags and typed values;
Cowgar exposes typed Boolean/Date/Double/Integer/File accessors, argument expansion
and application metadata. Their full APIs and error behavior are not reproduced.

Buoy's installed `command-line-args.md` already supplies Unicode argv through
`Main(args As Array Of String)`, including argv[0]. OptionKit adds schema-based
recognition and validation on top. No platform-specific argv parser is needed.

Implemented overlap: long/short option names, flag presence, String values,
required value options, independent positional arguments and explicit parse
errors. OptionKit deliberately rejects repeats and grouped short options. It does
not perform shell tokenization, automatic help generation, environment expansion,
config-file merging, typed conversion, signals, SystemD integration or ANSI color
stream output. These are separate extension opportunities rather than claims of
upstream parity.
