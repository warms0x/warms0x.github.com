---
layout: default
title: "Using cargo with Tor"
---

Last year [Tor](https://torproject.org) began to ship with [Rust
components](https://twitter.com/torproject/status/979752522679226368). In
theory the C code in the daemon is going to be slowly replaced by [Rust](https://rust-lang.org).
A major component of the Rust ecosystem is the command-line tool `cargo` which
helps with dependency resolution, tool installation, and build processes. Out
of the box `cargo` does not appear to work very well with Tor. Fortunately this
is easy to fix with a simple configuration change.


As of version 1.34.0, `cargo` supports directly utilizing the Tor SOCKS proxy:

```toml
[http]
proxy = "socks5h://localhost:9050"
```


With this addition to `~/.cargo/config`, `cargo` should route traffic through Tor.



