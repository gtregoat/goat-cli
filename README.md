# goat-cli

Published binaries of **`goat`**, the CLI for the goat platform.

This repository holds **no source code**. It exists only to carry releases: each
release carries the `goat` executable for every supported platform, a SHA-256
checksum for each, the version being published, and the install script.

The source lives in the private `gtregoat/goat` repository, under `goatctl/`.

## Start here (new to the project)

You need one thing from the owner: an invite code. Then:

```sh
curl -fsSL https://github.com/gtregoat/goat-cli/releases/latest/download/install.sh | sh
~/.local/bin/goat onboard        # paste the code when it asks; add --dev if your invite says so
```

The code is never a command-line argument (it would stay in your shell history): paste it
at the prompt or pipe it in. `goat onboard` puts `goat` on your PATH, signs you in, accepts the invite, sets up git,
clones the repos into `~/goat-workspace` and ends with a green/red checklist saying what,
if anything, is left. Re-run it any time; `goat onboard --check` only re-checks. The full
guide is `docs/ONBOARDING.md` in `gtregoat/goat`, which you can read once onboarded.

## Install

```sh
curl -fsSL https://github.com/gtregoat/goat-cli/releases/latest/download/install.sh | sh
```

That installs `goat` into `~/.local/bin`. After the first install, updates are:

```sh
goat update
```

## Why these artifacts are public

Deliberately. Signing in to the goat platform is `goat auth login`, which needs
the very binary you are trying to obtain — so the first hop cannot present a
goat credential, and a machine that has never installed anything has no GitHub
credential either. A gate on this download could only be satisfied by
distributing a credential some other way, which is a worse problem than the one
it would solve.

What secures the download is not secrecy but **integrity**: TLS, plus a SHA-256
published beside every binary. `goat update` verifies that checksum before it
writes anything, and `install.sh` verifies it with `sha256sum -c` / `shasum -c`
before it installs anything.

These binaries contain **no credentials**. Everything `goat` authenticates with
is supplied at run time.

## What a release contains

| Asset | What it is |
|---|---|
| `version` | the version this release publishes, e.g. `0.2.0` |
| `install.sh` | the bootstrap script |
| `goat-aarch64-apple-darwin` | macOS, Apple silicon |
| `goat-aarch64-apple-darwin.sha256` | its checksum |
| `goat-x86_64-unknown-linux-gnu` | Linux, x86-64 |
| `goat-x86_64-unknown-linux-gnu.sha256` | its checksum |

Releases are cut by CI in `gtregoat/goat`
(`.github/workflows/goatctl-release.yaml`) when a `goatctl-v<version>` tag is
pushed. Nothing is published by hand.
