+++
title = "Installing rbspy"
description = ""
keywords = []
categories = []
url = "/installing/"
+++

Installing rbspy is easy: it's a single binary. There are binaries for Linux, Mac, and Windows, which are the three supported platforms. It's also possible to build rbspy on FreeBSD.

### Download a binary

1. Download the latest release from [the releases page](https://github.com/rbspy/rbspy/releases)
    - Note: There are two types of Linux releases. Those tagged with `gnu` are dynamically linked against GNU libc, which needs to be installed on the system where rbspy runs. Those tagged with `musl` are statically linked against musl libc and can be used without a specific libc being installed.
2. Unpack the release. On Unix systems (Linux, Mac, FreeBSD), move the rbspy binary to `/usr/local/bin/rbspy`.

### Install from a package

If your preferred operating system or distribution isn't listed here, please consider making a package for it.

#### Alpine Linux

`apk add rbspy`

#### macOS (homebrew)

`brew install rbspy`

### Install in a CI pipeline

If you're using Docker, we publish images on [Docker Hub](https://hub.docker.com/r/rbspy/rbspy/tags). You can also download release binaries from [the releases page](https://github.com/rbspy/rbspy/releases).

### Compile from source

**Step 1**: Install Cargo (if you haven't already). There's a guide in the [cargo docs](https://doc.rust-lang.org/cargo/getting-started/installation.html), or you can run this command if you're on Linux.

```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

For other systems, please check the instructions at https://rustup.rs/.

**Step 2**: Add `~/.cargo/bin` to your PATH.

**Step 3**: Build rbspy!

```
git clone https://github.com/rbspy/rbspy
cd rbspy
cargo build --release
./target/release/rbspy --help
```

Alternatively, you can build and install from crates.io:

```
cargo install rbspy
```