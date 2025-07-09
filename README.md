# OpenWrt-Cloudflared

OpenWrt package of Cloudflare Tunnel client (Cloudflared) compiled against [Go with Cloudflare experimental patches](https://github.com/cloudflare/go).

[![Build](https://github.com/MoetaYuko/openwrt-cloudflared/actions/workflows/build.yml/badge.svg)](https://github.com/MoetaYuko/openwrt-cloudflared/actions/workflows/build.yml)

Archive note: as indicated by <https://github.com/cloudflare/cloudflared/commit/43a3ba347b49d75ae6b92d5e040ed5a17a616ca6>, the post-quantum encryption are already supported by upstream go 1.24. Hence, this project is no longer meaningful. Please use the official OpenWrt packages (>=2025.6.1).

## Why?

OpenWrt has an official package[^1] for Cloudflared that appears to work well. However, Cloudflared has to be compiled with a fork of go to support post-quantum encryption since version 2024.1.0[^2]. Unfortunately, OpenWrt maintainers refuse[^3] to compile it against the downstream patched Go and continue to use the official Go, resulting in broken Cloudflared binaries with the following runtime errors:

```
Wed Mar  6 15:32:28 2024 daemon.err cloudflared[14145]: 2024-03-06T07:32:28Z ERR Failed to create new quic connection error="failed to dial to edge with quic: INTERNAL_ERROR (local): tls: CurvePreferences includes unsupported curve" connIndex=0 event=0 ip=198.41.192.57
Wed Mar  6 15:32:28 2024 daemon.err cloudflared[14145]: 2024-03-06T07:32:28Z INF Retrying connection in up to 2s connIndex=0 event=0 ip=198.41.192.57
Wed Mar  6 15:32:30 2024 daemon.err cloudflared[14145]: 2024-03-06T07:32:30Z ERR Failed to create new quic connection error="failed to dial to edge with quic: INTERNAL_ERROR (local): tls: CurvePreferences includes unsupported curve" connIndex=0 event=0 ip=198.41.192.107
Wed Mar  6 15:32:30 2024 daemon.err cloudflared[14145]: 2024-03-06T07:32:30Z INF Retrying connection in up to 4s connIndex=0 event=0 ip=198.41.192.107
Wed Mar  6 15:32:33 2024 daemon.err cloudflared[14145]: 2024-03-06T07:32:32Z ERR Failed to create new quic connection error="failed to dial to edge with quic: INTERNAL_ERROR (local): tls: CurvePreferences includes unsupported curve" connIndex=0 event=0 ip=198.41.192.67
Wed Mar  6 15:32:33 2024 daemon.err cloudflared[14145]: 2024-03-06T07:32:32Z INF Retrying connection in up to 8s connIndex=0 event=0 ip=198.41.192.67
Wed Mar  6 15:32:35 2024 daemon.err cloudflared[14145]: 2024-03-06T07:32:35Z ERR Failed to create new quic connection error="failed to dial to edge with quic: INTERNAL_ERROR (local): tls: CurvePreferences includes unsupported curve" connIndex=0 event=0 ip=198.41.192.37
Wed Mar  6 15:32:35 2024 daemon.err cloudflared[14145]: 2024-03-06T07:32:35Z INF Retrying connection in up to 16s connIndex=0 event=0 ip=198.41.192.37
Wed Mar  6 15:32:47 2024 daemon.err cloudflared[14145]: 2024-03-06T07:32:47Z ERR Failed to create new quic connection error="failed to dial to edge with quic: INTERNAL_ERROR (local): tls: CurvePreferences includes unsupported curve" connIndex=0 event=0 ip=198.41.192.47
Wed Mar  6 15:32:47 2024 daemon.err cloudflared[14145]: 2024-03-06T07:32:47Z INF Retrying connection in up to 32s connIndex=0 event=0 ip=198.41.192.47
Wed Mar  6 15:33:04 2024 daemon.err cloudflared[14145]: 2024-03-06T07:33:04Z ERR Failed to create new quic connection error="failed to dial to edge with quic: INTERNAL_ERROR (local): tls: CurvePreferences includes unsupported curve" connIndex=0 event=0 ip=198.41.192.57
```

## What does this project do?

This project aims to provide sane Cloudflared packages by compiling them against the Cloudflare-patched Go for all possible OpenWrt architectures. Except for Go, everything else (including the Cloudflared packaging scripts) directly comes from the OpenWrt upstream so compatibility shouldn't be an issue. The compiled .ipk files can be found in Releases.

[^1]: <https://github.com/openwrt/packages/tree/master/net/cloudflared>
[^2]: <https://github.com/cloudflare/cloudflared/issues/1151#issuecomment-1888819250>
[^3]: <https://github.com/openwrt/packages/issues/23596#issuecomment-1980469933>
