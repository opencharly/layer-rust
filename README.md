# rust

The Rust toolchain (rustc compiler + cargo package manager) for OpenCharly
images, installed from distro repositories.

The `rust` candy installs the Rust compiler and Cargo from the distro repos —
`rust` + `cargo` on Fedora, `rustc` + `cargo` on Debian and Ubuntu, and the `rust`
package on Arch — with `~/.cargo/bin` appended to `PATH`. Both binaries land at
`/usr/bin` and report a version string (the candy's `plan:` asserts `rustc` and
`cargo` on every arm), so a developer can compile and build Rust crates inside
the image.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `rust` |
| Binaries | `/usr/bin/rustc`, `/usr/bin/cargo` |
| Packages | RPM: `rust`, `cargo` · DEB: `rustc`, `cargo` · PAC: `rust` |
| PATH additions | `~/.cargo/bin` |
| Service / port | none |

## How to use it

Compose the layer by pinning this repo in a box's `candy:` list:

```yaml
my-rust-box:
  candy:
    base: fedora
    candy:
      - '@github.com/opencharly/layer-rust:v2026.239.1627'
```

Then, inside the built image (or on a dev host):

```bash
rustc --version
cargo --version
cargo build --release
```

The candy's `plan:` asserts both binaries exist at `/usr/bin`, report a version
string, and that the `rust` package is recorded (mapped per distro).

## rust candy vs the build-toolchain rust packages

Two places ship a Rust toolchain. Use this candy when you need rustc/cargo at
**runtime** in the final container; use the builder-stage `rust`+`cargo` packages
from `/charly-coder:build-toolchain` when you only need to compile a cdylib that
is copied into the final image — the builder-stage toolchain stays out of the
runtime layers.

## Layout

- `charly.yml` — the `rust:` candy entity (the `path_append:`, the
  `distro.{arch,debian,fedora,ubuntu}:` package sections, and the `check:`
  probes) and the embedded `rust-skill:` skill entity.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Owning skill: `/charly-coder:rust`
- Builder-stage alternative: `/charly-coder:build-toolchain`
- Consumers: `/charly-coder:language-runtimes`, `/charly-coder:pre-commit`
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
