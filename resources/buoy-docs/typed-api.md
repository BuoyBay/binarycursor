---
title: "binarycursor typed API"
summary: "Source-derived public signatures, properties, parameters, defaults, and return values."
category: "Reference"
order: 21
types: ["BinaryCursor"]
dependencies: []
symbols: ["BinaryCursor"]
related: ["api.md", "getting-started.md", "index.md"]
---

# binarycursor typed API

Generated from the package source declarations. Platform guards are shown where present; inherited members are documented on the base type. See the [API contracts](api.md) for behavior and limitations.

## BinaryCursor

`Public Class BinaryCursor`

Bounded, owned byte snapshot with exact reads and explicit byte order.

### Properties

| Name | Type | Read-Only | Shared | Default | Description |
| --- | --- | --- | --- | --- | --- |
| `Length` | `Integer` | Yes | No | — | — |
| `Position` | `Integer` | Yes | No | — | — |
| `Remaining` | `Integer` | Yes | No | — | — |
| `EOF` | `Boolean` | Yes | No | — | — |

### Constructor

`Public Sub Constructor(data As Bytes, maxBytes As Integer = 16777216)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `data` | `Bytes` | `ByVal` | Required |
| `maxBytes` | `Integer` | `ByVal` | `16777216` |

Returns: no value.

### Seek

`Public Sub Seek(position As Integer)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `position` | `Integer` | `ByVal` | Required |

Returns: no value.

### Skip

`Public Sub Skip(offset As Integer)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `offset` | `Integer` | `ByVal` | Required |

Returns: no value.

### PeekBytes

`Public Function PeekBytes(count As Integer) As Bytes`

Copy bytes at the current position without advancing it.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `count` | `Integer` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `Bytes` |

### ReadBytes

`Public Function ReadBytes(count As Integer) As Bytes`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `count` | `Integer` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `Bytes` |

### ReadUTF8

`Public Function ReadUTF8(count As Integer) As String`

Decode before committing position, so malformed UTF-8 leaves it unchanged.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `count` | `Integer` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `String` |

### ReadUInt8

`Public Function ReadUInt8() As Integer`

No parameters.

#### Returns

| Type |
| --- |
| `Integer` |

### ReadInt8

`Public Function ReadInt8() As Integer`

No parameters.

#### Returns

| Type |
| --- |
| `Integer` |

### ReadVarUInt

`Public Function ReadVarUInt() As Integer`

Canonical unsigned LEB128 in 0..2^63-1; failure preserves Position.

No parameters.

#### Returns

| Type |
| --- |
| `Integer` |

### ReadVarInt

`Public Function ReadVarInt() As Integer`

Canonical ZigZag LEB128 over the full signed Integer range.

No parameters.

#### Returns

| Type |
| --- |
| `Integer` |

### ReadUInt16

`Public Function ReadUInt16(littleEndian As Boolean = True) As Integer`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `littleEndian` | `Boolean` | `ByVal` | `True` |

#### Returns

| Type |
| --- |
| `Integer` |

### ReadInt16

`Public Function ReadInt16(littleEndian As Boolean = True) As Integer`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `littleEndian` | `Boolean` | `ByVal` | `True` |

#### Returns

| Type |
| --- |
| `Integer` |

### ReadUInt32

`Public Function ReadUInt32(littleEndian As Boolean = True) As Integer`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `littleEndian` | `Boolean` | `ByVal` | `True` |

#### Returns

| Type |
| --- |
| `Integer` |

### ReadInt32

`Public Function ReadInt32(littleEndian As Boolean = True) As Integer`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `littleEndian` | `Boolean` | `ByVal` | `True` |

#### Returns

| Type |
| --- |
| `Integer` |

### ReadCursor

`Public Function ReadCursor(count As Integer) As BinaryCursor`

The child owns a copy; parent advances only once the child exists.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `count` | `Integer` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `BinaryCursor` |

### ReadFloat32

`Public Function ReadFloat32(littleEndian As Boolean = True) As Single`

Read IEEE binary32 bits without numeric conversion; failure preserves Position.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `littleEndian` | `Boolean` | `ByVal` | `True` |

#### Returns

| Type |
| --- |
| `Single` |

### ReadFloat64

`Public Function ReadFloat64(littleEndian As Boolean = True) As Double`

Read IEEE binary64 bits without numeric conversion; failure preserves Position.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `littleEndian` | `Boolean` | `ByVal` | `True` |

#### Returns

| Type |
| --- |
| `Double` |
