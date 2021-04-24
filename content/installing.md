+++
title = "Installing rbspy"
description = ""
keywords = []
categories = []
url = "/installing/"
+++

### Download a binary

Installing rbspy is easy: it's a single static binary. There are binaries for Linux and Mac, which
are the two supported platforms.

1. Download the latest release from [the releases page](https://github.com/rbspy/rbspy/releases)
2. Unzip + move the rbspy binary to `/usr/local/bin/rbspy`

### Compile from source

**Step 1**: Install Cargo (if you haven't already). There's a guide in the [cargo docs](https://doc.rust-lang.org/cargo/getting-started/installation.html), or just run this command.

```
curl -sSf https://static.rust-lang.org/rustup.sh | sh
```

**Step 2**: Add `~/.cargo/bin` to your PATH.

**Step 3**: Build rbspy!

```
git clone https://github.com/rbspy/rbspy
cd rbspy
cargo build --release
./target/release/rbspy --help
```
