# uping

Microsecond-precision ICMP ping for macOS and Linux.

Uses `clock_gettime(CLOCK_MONOTONIC)` for sub-millisecond RTT measurement and reports results in whole microseconds. Prefers `SOCK_RAW` for accurate timing, falling back to `SOCK_DGRAM` when privileges are unavailable. Output is colour-coded by RTT.

## Usage

```bash
./uping 1.1.1.1
./uping -c 10 -i 0.5 google.com
./uping -W 1 -6 ipv6.google.com
```

```
UPING google.com (142.251.151.119): ICMPv4, timeout 2.0s
seq=1    12054µs  from 142.251.151.119
seq=2    12229µs  from 142.251.151.119
seq=3    11986µs  from 142.251.151.119
^C
--- google.com uping statistics ---
3 packets transmitted, 3 received, 0.0% loss
rtt min/avg/max = 11986/12089.7/12229 µs
```

## Options

| Flag | Description |
|------|-------------|
| `-c COUNT` | Stop after COUNT pings (default: run until Ctrl+C) |
| `-i INTERVAL` | Seconds between pings, may be fractional (default: 1) |
| `-W TIMEOUT` | Per-ping timeout in seconds (default: 2) |
| `-4` | Force IPv4 |
| `-6` | Force IPv6 |
| `-n` | Disable colour output |

RTT colour coding: green < 1 ms, yellow < 10 ms, red >= 10 ms.

## Build

```bash
make
```

Requires `cc` / `gcc` / `clang` and a standard C library. No other dependencies.

## Install

```bash
sudo make install   # installs to /usr/local/bin/uping
```

On **Linux**, `sudo make install` also runs `setcap cap_net_raw+ep` on the binary so it can be used without `sudo` at runtime. Requires `libcap2-bin` (Debian/Ubuntu) or `libcap` (Fedora/RHEL):

```bash
sudo apt install libcap2-bin   # Debian/Ubuntu
sudo dnf install libcap        # Fedora/RHEL
```

If `setcap` is unavailable you can run `sudo uping` directly, or allow unprivileged ICMP system-wide:

```bash
sudo sysctl -w net.ipv4.ping_group_range="0 2147483647"
```

On **macOS**, `SOCK_RAW` requires root for accurate timing. Without root, uping falls back to `SOCK_DGRAM` which adds ~30 ms of kernel overhead to every reading. To run without `sudo`, apply setuid-root after installing (the same mechanism as the system `ping`):

```bash
sudo chown root:wheel /usr/local/bin/uping
sudo chmod u+s /usr/local/bin/uping
```

## Uninstall

```bash
sudo make uninstall
```

## License

MIT
