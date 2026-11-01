# Provenance and parity

Inspected [npalardy/BufferedBinaryInputStream](https://github.com/npalardy/BufferedBinaryInputStream)
at commit `b7d60a81eb6f7e7ea0acc087ab0b7a312ad804a0`.
The source class and root MIT license were read. The original buffers file reads,
has ReadUInt8, Length, EOF, writable Position and Close. Its constructor explicitly
ignores readWrite, and its EOF ReadUInt8 returns zero rather than an exact-read
exception. Seeking outside the current buffered area has incomplete branches.

This implementation is newly written Buoy code and copies no upstream code.
The upstream notice is retained in UPSTREAM-LICENSE.txt for attribution.
Its useful portable abstraction is byte positioning and reading; Buoy already
supplies file buffering primitives. This package chooses immutable owned input,
atomic exact reads, bounded allocation, strict UTF-8 and explicit integer byte
order. It does not claim original file-stream API compatibility or benchmarks.

The installed `bytes.md` and `binarystream.md` references were checked against the
compiler. Sized reads return Buoy Integer rather than narrowing FFI integer types.
Unsigned 32-bit arithmetic fits exactly in its signed 64-bit range; signed
conversion uses subtraction of 2^width, avoiding overflow and pointer casts.
