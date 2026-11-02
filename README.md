# Bounded binary reads

`Import binarycursor` provides `BinaryCursor`, an owned, bounded snapshot for
parsing binary records. Existing Buoy `BinaryStream` handles file I/O and typed
reads; this package adds exact in-memory reads with explicit failure-position
semantics. It has no package dependencies; float bit copies use the system C runtime.

## Install

From a Buoy consumer project with BPM installed:

```sh
bpm add binarycursor --git https://github.com/BuoyBay/binarycursor.git --rev refs/heads/main
```

BPM records the resolved commit in its lockfile. Keep that lockfile with the
consumer project for reproducible installs.

## API at a glance

| Operation | Contract |
| --- | --- |
| `New BinaryCursor(data, maxBytes = 16777216)` | Copies `Bytes`; rejects Nil, oversized input and limits outside 0..16 MiB |
| `Length`, `Position`, `Remaining`, `EOF` | Read-only byte coordinates; EOF is exactly Position = Length |
| `Seek(position)` | Absolute position in 0..Length |
| `Skip(offset)` | Signed relative movement within input, checked without overflow |
| `PeekBytes(count)` | Fresh owned copy; does not advance |
| `ReadBytes(count)` | Exact owned copy; advances by count |

## Build from source

With the native `build-packages` driver from [devtools](https://github.com/BuoyBay/devtools)
on your `PATH`, run from this checkout:

```sh
build-packages --root "$PWD" --compile-consumer binarycursor
```

The driver checks source targets and compiles an import consumer. See the
[API guide](docs/API.md) for behavior, limits and any host prerequisites.

## Contributing

[Contributing](CONTRIBUTING.md) · [License](LICENSE)
