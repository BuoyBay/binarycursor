# BinaryCursor 0.1.0

`Import binarycursor` provides `BinaryCursor`, an owned, bounded snapshot for
parsing binary records. Existing Buoy `BinaryStream` handles file I/O and typed
reads; this package adds exact in-memory reads with explicit failure-position
semantics. It has no package dependencies; float bit copies use the system C runtime.

## API

| Operation | Contract |
| --- | --- |
| `New BinaryCursor(data, maxBytes = 16777216)` | Copies `Bytes`; rejects Nil, oversized input and limits outside 0..16 MiB |
| `Length`, `Position`, `Remaining`, `EOF` | Read-only byte coordinates; EOF is exactly Position = Length |
| `Seek(position)` | Absolute position in 0..Length |
| `Skip(offset)` | Signed relative movement within input, checked without overflow |
| `PeekBytes(count)` | Fresh owned copy; does not advance |
| `ReadBytes(count)` | Exact owned copy; advances by count |
| `ReadUTF8(count)` | Exact byte count, strict UTF-8 decoding before position commits |
| `ReadUInt8()`, `ReadInt8()` | Unsigned/signed byte as Integer |
| `ReadUInt16(littleEndian = True)`, `ReadInt16(...)` | Explicit 16-bit byte order; Integer result |
| `ReadUInt32(littleEndian = True)`, `ReadInt32(...)` | Explicit 32-bit byte order; Integer result, including 4294967295 unsigned |
| `ReadFloat32(littleEndian = True)` | IEEE binary32 as Single; exact bits |
| `ReadFloat64(littleEndian = True)` | IEEE binary64 as Double; exact bits |
| `ReadVarUInt()` | Canonical unsigned LEB128, 0..2^63-1, at most 9 bytes |
| `ReadVarInt()` | Canonical ZigZag LEB128, full signed Integer range, at most 10 bytes |
| `ReadCursor(count)` | Independent child over the next count bytes; parent advances |

Indices and sizes are bytes, not characters. All counts must be nonnegative and
fit in Remaining. Empty reads and seeking to EOF are valid. Invalid arguments,
truncation, invalid seeks and invalid skips throw `InvalidArgumentException`
without changing Position. Invalid UTF-8 throws `EncodingException`, also without
changing Position. Reads never substitute zero for missing bytes.

Failure leaves the position unchanged for the failing call only. If parsing a
record takes several reads, successful earlier reads remain consumed. Save
`Position` before the record and call `Seek` with that saved position if the
whole parse must be retried. A child from `ReadCursor` owns its bytes, but creating
it already advances the parent; a later failure while parsing the child does
not rewind the parent.

Input, peeked bytes, returned bytes and child cursors are independent snapshots.
There is no shared slicing or writable view. A cursor is not thread-safe;
concurrent callers must provide their own synchronization. The 16 MiB cap bounds
each snapshot, not the total retained by multiple cursors or returned copies.

## Example and verification

The WAV header example (not included in this repository)
parses a canonical PCM RIFF/WAVE fixture, then reads its two sample values. It
deliberately expects the fixed 16-byte PCM fmt chunk followed by data; it is not
a general WAV reader and does not implement additional/reordered chunks or codecs.


The runner seals a fresh package, checks module/test/example on six desktop
targets, runs the example and regression tests on Linux, and verifies 576 numeric
reads against independently packed Python `struct` data. It isolates compilation
from this checkout's unrelated application resource manifests. `BUOY` selects
the compiler, `BPM_PACKAGER` the package command (default `/usr/bin/buoy`).

## Deliberate scope

This is an independent in-memory parsing API inspired by Norman Palardy's
BufferedBinaryInputStream. It does not port the original file buffering or claim
a speed improvement over BinaryStream. No file handles need closing. Streaming,
file-backed buffering, 64-bit unsigned integers, bit fields,
full-range UInt64 varints and writable cursors remain future work. See
[provenance and parity](../PROVENANCE.md).

## Variable-length integers

`ReadVarUInt()` matches `BinaryWriter.WriteVarUInt()` for 0 through
9223372036854775807. `ReadVarInt()` matches `WriteVarInt()` using ZigZag
(0 → 0, -1 → 1, 1 → 2), including -9223372036854775808 and
9223372036854775807. Both use canonical unsigned LEB128 byte order,
independent of fixed-width endianness. The terminal payload must be nonzero
after the first byte; redundant zero groups are rejected. Signed encodings
may use ten bytes only with a terminal payload of 1. Unsigned encodings
may use at most nine bytes.

Truncated, overflowing, and overlong encodings throw `InvalidArgumentException`
and leave Position unchanged. Successful reads consume only their encoding,
leaving subsequent fields available. No full-range UInt64 read API is added.

Maintainers run the private validation suite before release. See the repository README for public build instructions.

## IEEE floating-point fields

`ReadFloat32` / `WriteFloat32` use `Single` (IEEE binary32);
`ReadFloat64` / `WriteFloat64` use `Double` (IEEE binary64). Both default
to little-endian and accept `False` for big-endian. They copy the value's
bits without arithmetic, preserving signed zero, subnormals, infinities,
and NaN sign/payload bits. Keep values in their original type for exact
roundtrips: numeric conversion or arithmetic by the caller can alter NaNs
or round values. No finite-only policy is imposed; format consumers such as
GLTF must enforce their own validity rules.

Reads require all 4/8 bytes before advancing Position. Writes check the
entire 4/8-byte capacity before changing output. Truncated reads or
insufficient writer capacity throw `InvalidArgumentException` without
changing Position or output, respectively.

Implementation uses `memcpy` from the system C runtime (`ucrtbase` on
Windows), with no extra shipped native library or Xojo dependency. The six
supported desktop targets are little-endian Intel/ARM64; byte order on
the wire remains explicit.

Maintainers run the private validation suite before release. See the repository README for public build instructions.
