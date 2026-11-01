---
title: "optionkit typed API"
summary: "Source-derived public signatures, properties, parameters, defaults, and return values."
category: "Reference"
order: 21
types: ["OptionResult", "OptionSchema"]
dependencies: []
symbols: ["OptionResult", "OptionSchema"]
related: ["api.md", "getting-started.md", "index.md"]
---

# optionkit typed API

Generated from the package source declarations. Platform guards are shown where present; inherited members are documented on the base type. See the [API contracts](api.md) for behavior and limitations.

## OptionResult

`Public Class OptionResult`

A parsed result owns its values; positional arguments are returned as copies.

### Has

`Public Function Has(name As String) As Boolean`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `name` | `String` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `Boolean` |

### Value

`Public Function Value(name As String) As String`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `name` | `String` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `String` |

### WithDefaults

`Public Function WithDefaults(names As Array Of String, values As Array Of String) As OptionResult`

Return a new result with owned fallback values; supplied presence is unchanged.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `names` | `Array Of String` | `ByVal` | Required |
| `values` | `Array Of String` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `OptionResult` |

### HasDefault

`Public Function HasDefault(name As String) As Boolean`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `name` | `String` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `Boolean` |

### ResolvedValue

`Public Function ResolvedValue(name As String) As String`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `name` | `String` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `String` |

### IntegerValue

`Public Function IntegerValue(name As String, minimum As Integer = -9223372036854775808, maximum As Integer = 9223372036854775807) As Integer`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `name` | `String` | `ByVal` | Required |
| `minimum` | `Integer` | `ByVal` | `-9223372036854775808` |
| `maximum` | `Integer` | `ByVal` | `9223372036854775807` |

#### Returns

| Type |
| --- |
| `Integer` |

### DoubleValue

`Public Function DoubleValue(name As String, minimum As Double = -1.7976931348623157e308, maximum As Double = 1.7976931348623157e308) As Double`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `name` | `String` | `ByVal` | Required |
| `minimum` | `Double` | `ByVal` | `-1.7976931348623157e308` |
| `maximum` | `Double` | `ByVal` | `1.7976931348623157e308` |

#### Returns

| Type |
| --- |
| `Double` |

### Positionals

`Public Function Positionals() As Array Of String`

No parameters.

#### Returns

| Type |
| --- |
| `Array Of String` |

### Constructor

`Public Sub Constructor(names As Array Of String, values As Array Of String, positionals As Array Of String)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `names` | `Array Of String` | `ByVal` | Required |
| `values` | `Array Of String` | `ByVal` | Required |
| `positionals` | `Array Of String` | `ByVal` | Required |

Returns: no value.

## OptionSchema

`Public Class OptionSchema`

Case-sensitive schema for flags and String-valued command-line options.

### AddFlag

`Public Sub AddFlag(name As String, shortName As String = "", description As String = "")`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `name` | `String` | `ByVal` | Required |
| `shortName` | `String` | `ByVal` | `""` |
| `description` | `String` | `ByVal` | `""` |

Returns: no value.

### AddValue

`Public Sub AddValue(name As String, shortName As String = "", required As Boolean = False, description As String = "", valueName As String = "VALUE")`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `name` | `String` | `ByVal` | Required |
| `shortName` | `String` | `ByVal` | `""` |
| `required` | `Boolean` | `ByVal` | `False` |
| `description` | `String` | `ByVal` | `""` |
| `valueName` | `String` | `ByVal` | `"VALUE"` |

Returns: no value.

### AddDefaultValue

`Public Sub AddDefaultValue(name As String, defaultValue As String, shortName As String = "", description As String = "", valueName As String = "VALUE")`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `name` | `String` | `ByVal` | Required |
| `defaultValue` | `String` | `ByVal` | Required |
| `shortName` | `String` | `ByVal` | `""` |
| `description` | `String` | `ByVal` | `""` |
| `valueName` | `String` | `ByVal` | `"VALUE"` |

Returns: no value.

### SetDefault

`Public Sub SetDefault(name As String, defaultValue As String)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `name` | `String` | `ByVal` | Required |
| `defaultValue` | `String` | `ByVal` | Required |

Returns: no value.

### Usage

`Public Function Usage(programName As String, positionalUsage As String = "") As String`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `programName` | `String` | `ByVal` | Required |
| `positionalUsage` | `String` | `ByVal` | `""` |

#### Returns

| Type |
| --- |
| `String` |

### Help

`Public Function Help(programName As String, summary As String = "", positionalUsage As String = "") As String`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `programName` | `String` | `ByVal` | Required |
| `summary` | `String` | `ByVal` | `""` |
| `positionalUsage` | `String` | `ByVal` | `""` |

#### Returns

| Type |
| --- |
| `String` |

### Parse

`Public Function Parse(arguments As Array Of String, maxArguments As Integer = 10000) As OptionResult`

Input contains user arguments only: skip argv[0] before calling.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `arguments` | `Array Of String` | `ByVal` | Required |
| `maxArguments` | `Integer` | `ByVal` | `10000` |

#### Returns

| Type |
| --- |
| `OptionResult` |
