# Issue with `cargo vendor` + `cargo install`

I've found an issue with vendoring and installation of cargo subcommands
related to semver compatibility checks. To recreate the issue:

1. Vendor the dependencies using `cargo vendor .cargo_vendor` (this step is checked in for convenience, but feel free to delete the directory and recreate it)
1. Attempt to install the subcommand (from repository root CWD): `cargo install --locked --frozen --path .cargo_vendor/cargo-ndk/ --root bin`

You get the following error message:
```  Installing cargo-ndk v2.5.0 (/home/dcsommer/src/vendor-install-demo/.cargo_vendor/cargo-ndk)
error: failed to select a version for the requirement `cargo_metadata = "^0.14.0"`
candidate versions found which didn't match: 0.14.1
location searched: directory source `/home/dcsommer/src/vendor-install-demo/.cargo_vendor` (which is replacing registry `crates-io`)
required by package `cargo-ndk v2.5.0 (/home/dcsommer/src/vendor-install-demo/.cargo_vendor/cargo-ndk)`
perhaps a crate was updated and forgotten to be re-vendored?
```

As `0.14.1` is compatible with the constraint `^0.14.0`, this appears to
be a bug with `cargo install`.

I've tested this with 1.57 and 1.53 and it repros on both.