<p align="center">
  <img src="assets/forge_logo.png" width="130" alt="ForgeMiner logo">
</p>

<h1 align="center">ForgeMiner</h1>

<p align="center"><b>A fast, native NVIDIA GPU miner — Pearl (PRL), QubitCoin (QTC), KawPow (Ravencoin, Quai, Neurai), Cryptix (CYTX) and BTX (btx.dev)</b></p>

<p align="center">
  <a href="https://github.com/0xHashRaptor/ForgeMiner/releases"><img src="assets/badge_version.svg" alt="version 1.4.1"></a>
  <a href="#download"><img src="assets/badge_platform.svg" alt="platform: Windows | Linux | HiveOS | Docker"></a>
  <a href="#supported-gpus"><img src="assets/badge_gpu.svg" alt="GPU: NVIDIA Pascal | RTX 20/30/40/50 + CMP"></a>
</p>
<p align="center">
  <a href="https://forgeminer.org"><img src="assets/badge_site.svg" alt="site: forgeminer.org"></a>
  <a href="https://t.me/ForgeMiner"><img src="assets/badge_telegram.svg" alt="Telegram: Releases"></a>
  <a href="https://discord.gg/vxUTbb9B"><img src="assets/badge_discord.svg" alt="Discord: Community"></a>
</p>

---

## Contents

- [Overview](#overview)
- [Download](#download)
- [Quick start](#quick-start)
- [Coins & dev fee](#coins--dev-fee)
- [Performance](#performance)
- [Features](#features)
- [Options](#options)
- [Monitoring API](#monitoring-api)
- [Supported GPUs](#supported-gpus)
- [Resources](#resources)

---

## Overview

ForgeMiner is a high-performance, fully native NVIDIA GPU miner. It talks to the GPU directly through the CUDA Driver API — no Python, no WSL, no extra runtimes — so it starts instantly and runs lean even on low-spec rigs. It mines **Pearl (PRL)**, **QubitCoin (QTC)**, **KawPow** (Ravencoin RVN, Quai QUAI, Neurai XNA), **Cryptix (CYTX)** and **BTX (btx.dev)** from a single binary — pick the coin with one flag — and more coins are on the way.

Every algorithm ships a separate per-architecture build for each supported card, auto-selected at launch, so each GPU runs at its peak.

Website: **[forgeminer.org](https://forgeminer.org)** · ForgeMiner is closed-source; releases are published here and announced on [Telegram](https://t.me/ForgeMiner) and [Discord](https://discord.gg/vxUTbb9B).

<!-- Tip: drop a dashboard screenshot here for instant credibility, e.g. -->
<!-- <p align="center"><img src="assets/dashboard.png" width="720" alt="ForgeMiner live dashboard"></p> -->

---

## Download

Grab the latest build from the [**Releases**](https://github.com/0xHashRaptor/ForgeMiner/releases) page:

| Platform | Package |
|---|---|
| Windows | `ForgeMiner-<version>-windows.zip` |
| Linux | `ForgeMiner-<version>-linux.tar.gz` (glibc 2.17+) |
| HiveOS | `ForgeMiner-<version>.tar.gz` (flight-sheet install URL) |
| Docker | `docker pull hashraptor/forge` (tags `:latest` and the version) |

---

## Quick start

### Windows
1. Download and unpack the Windows release.
2. Open the `.bat` for your coin/pool/region and set your wallet and worker. Files are named `<algo>_<pool>_<region>.bat` (`_SSL` = encrypted). One per coin:
   - Pearl — `pearlhash_Baikal_Global.bat`, `pearlhash_HeroMiners_DE.bat`, `pearlhash_Kryptex_RU.bat`, …
   - QubitCoin — `qhash_LuckyPool_RU.bat`, `qhash_k1pool_EU.bat`, …
   - KawPow — `kawpow_RVN_HeroMiners_US.bat`, `kawpow_QUAI_Kryptex_EU.bat`, `kawpow_XNA_2Miners_EU.bat`, …
   - Cryptix — `cryptix_Baikalmine_Global.bat`
   - BTX — `btx_lproute_EU.bat`, `btx_lproute_RU.bat`
3. Double-click to start. Run as Administrator to apply the built-in overclock.

### Linux
```bash
chmod +x forge
# Pearl
FORGE_POOL=ru.pearl.herominers.com:1200 FORGE_WALLET=YOUR_PRL_WALLET FORGE_WORKER=rig01 ./forge
# QubitCoin
./forge --algorithm qhash  --wallet YOUR_QTC_WALLET  --pool ru.luckypool.io:8610          --worker rig01
# KawPow — RVN / QUAI (coin auto-detected from the pool) or XNA (set --coin xna)
./forge --algorithm kawpow --wallet YOUR_RVN_WALLET  --pool us.ravencoin.herominers.com:1140 --worker rig01
# Cryptix
./forge --algorithm cryptix --wallet YOUR_CYTX_WALLET --pool cytx.baikalmine.com:9010        --worker rig01
# BTX
./forge --algorithm btx     --wallet YOUR_BTX_WALLET  --pool btx-eu.lproute.com:8660          --worker rig01
```

### Docker
```bash
docker pull hashraptor/forge
docker run --rm --gpus all hashraptor/forge \
  --algorithm pearlhash --wallet YOUR_PRL_WALLET --worker rig01 --pool pearl.baikalmine.com:2010
# dashboard: add -p 7777:7777 and --api-bind 0.0.0.0:7777
```

### HiveOS
Custom miner flight sheet — installation URL `.../ForgeMiner-<version>.tar.gz`, wallet template `%WAL%.%WORKER_NAME%`, and in *Extra config* set `FORGE_ALGO=pearlhash` (or `qhash` / `kawpow` / `cryptix` / `btx`; for KawPow also `FORGE_COIN=rvn|quai|xna`). Ready-made flight sheets: **[forgeminer.org/#flightsheets](https://forgeminer.org/#flightsheets)**.

---

## Coins & dev fee

| Coin | `--algorithm` | Pools (ready-made `.bat` in the release) | Dev fee |
|------|---------------|-------------------------------------------|:------:|
| Pearl (PRL) | `pearlhash` | BaikalMine · HeroMiners · LuckyPool · Kryptex · 2Miners · AlphaPool | 2% |
| QubitCoin (QTC) | `qhash` | LuckyPool · k1pool | 1% |
| Ravencoin (RVN) | `kawpow` | HeroMiners · 2Miners · RavenMiner · Kryptex · k1pool | 0.7% |
| Quai (QUAI) | `kawpow` `--coin quai` | HeroMiners · k1pool · Kryptex | 0.7% |
| Neurai (XNA) | `kawpow` `--coin xna` | 2Miners · Vipor · Kryptex | 0.7% |
| Cryptix (CYTX) | `cryptix` | BaikalMine · CryptixNetwork | 2% |
| BTX (btx.dev) | `btx` | LuckyPool (lproute) | 1% |

The dev fee is interleaved (no graph dips) and verifiable on your pool. No hidden second fee. *More algorithms are on the way.*

---

## Performance

Pearl hashrate per GPU — **community-verified, varies with overclock and power limit**:

| GPU | Pearl (PearlHash) |
|-----|:-----------------:|
| RTX 5080 | ~195 TH/s |
| RTX 4070 Ti | ~122 TH/s |
| RTX 3060 Ti | ~58 TH/s |
| P104-100 (8 GB) | ~6.5 TH/s |

KawPow on the P104-100 runs at about **11.5 MH/s** per card. Post your own numbers in [Telegram](https://t.me/ForgeMinerChat) or [Discord](https://discord.gg/vxUTbb9B) — the table grows with the community.

---

## Features

- **Multiple coins, one binary** — Pearl, QubitCoin, KawPow (RVN / QUAI / XNA), Cryptix or BTX; select with `--algorithm`.
- **Architecture-tuned kernels** — a dedicated kernel per GPU generation (Pascal / Turing / Ampere / Ada / Blackwell), auto-selected at launch.
- **Native and lightweight** — direct CUDA Driver API, near-zero CPU load; no Python, WSL or extra runtimes. Starts in a second, runs on weak hosts and many-GPU boxes.
- **Efficient on crowded rigs** — keeps the GPUs fed even with many cards on a weak CPU, several miner instances, or slow x1 risers.
- **One self-contained binary** — everything embedded; no CUDA runtime or loose kernel files to manage. Even KawPow ships as a single executable.
- **Built-in overclocking & fan control** — lock clocks, apply offsets, set a power limit and drive fans straight from the miner — a different OC per card on mixed rigs. No third-party tool.
- **Multi-pool with fail-over** — standard Stratum for every coin, SSL/TLS pools, automatic reconnect and pool fail-over.
- **Live dashboard & read-only API** — per-GPU hashrate, temps (incl. VRAM on Windows), clocks, fans, power and shares — plus JSON, Prometheus and Claymore-compatible endpoints.
- **HiveOS ready** — drops straight into a custom-miner slot.

---

## Options

Anything you pass on the command line has an `FORGE_*` environment-variable twin — handy for HiveOS *Extra config* and `.bat` files.

| Flag | Env | Description |
|------|-----|-------------|
| `--algorithm` | `FORGE_ALGO` | `pearlhash`, `qhash`, `kawpow`, `cryptix` or `btx`. |
| `--coin` | `FORGE_COIN` | KawPow coin: `rvn`, `quai` or `xna` (auto-detected from the pool; set explicitly for Neurai / Vipor). |
| `--pool` | `FORGE_POOL` | Pool `host:port`. SSL/TLS supported; list several for fail-over. |
| `--wallet` | `FORGE_WALLET` | Payout wallet address. |
| `--worker` | `FORGE_WORKER` | Worker / rig name. |
| `--password` | `FORGE_PASS` | Pool password (usually `x`). |
| `--proto` | `FORGE_PROTO` | Pearl dialect: `stratum` or `alpha` (AlphaPool). |
| `--gpu` | `FORGE_GPU` | Mine only these indices, e.g. `0,1,2,6` (`nvidia-smi` order). |
| — | `FORGE_LOWVRAM` | Low-VRAM mode for 8 GB cards (Pearl). Auto by default. |

<details>
<summary><b>Overclocking &amp; fan control</b></summary>

> ForgeMiner is core-clock bound and memory-light — push the core high, leave memory low. Overclocking needs root (Linux/HiveOS) or Administrator (Windows). Each flag takes one value, or a comma list mapped to `--gpu`.

| Flag | Env | Description |
|------|-----|-------------|
| `--cclk` | `FORGE_CCLK` | Lock core clock (MHz). |
| `--coff` | `FORGE_COFF` | Core clock offset (MHz, +/−). |
| `--mclk` | `FORGE_MCLK` | Lock memory clock (MHz). |
| `--moff` | `FORGE_MOFF` | Memory clock offset (MHz, +/−). |
| `--plimit` | `FORGE_PLIMIT` | Power limit (Watts). |
| `--fan` | `FORGE_FAN` | Fixed fan speed (%). |
| `--fan-curve` | `FORGE_FANCURVE` | Temperature→speed curve, e.g. `45:30,60:55,70:75,80:100`. |

```text
# per-GPU (values map to --gpu order)
--gpu 0,1,2,6 --coff 300,250,300,200 --plimit 280,280,300,260
```
GeForce cards have a ~30% hardware fan floor; the driver's automatic control is restored on exit.
</details>

---

## Monitoring API

Off by default and **read-only** — it only reports stats, it can never control or reconfigure the miner.

```text
--api                    on 127.0.0.1:7777 (this machine only)
--api-bind 0.0.0.0:7777  on the LAN (watch from your phone / another PC)
```
On HiveOS set `FORGE_API=127.0.0.1:7777` in *Extra config*.

| Endpoint | Format | Use with |
|---|---|---|
| `GET /` | HTML | The web dashboard (total + per-GPU hashrate, temps, VRAM temp, clocks, fans, power, shares, live graph; dark/light theme). |
| `GET /summary` | JSON | Grafana, bots, custom dashboards. |
| `GET /metrics` | Prometheus | Grafana dashboards and alerts. |
| `miner_getstat1` | Claymore | Awesome Miner, mmpOS and the wider monitoring ecosystem. |

> Keep the default `127.0.0.1` for local-only access; use `0.0.0.0` only behind your own router / firewall / VPN.

---

## Supported GPUs

Kernels are tuned per architecture, so a whole generation is covered — desktop and laptop alike.

| Generation | Cards |
|---|---|
| **Blackwell** (RTX 50) | 5090 · 5080 · 5070 Ti · 5070 · 5060 Ti · 5060 · 50-series Laptop |
| **Ada** (RTX 40) | 4090 · 4080 (S) · 4070 Ti (S) · 4070 (S) · 4060 Ti · 4060 · 40-series Laptop |
| **Ampere** (RTX 30) | 3090 Ti · 3090 · 3080 Ti · 3080 · 3070 Ti · 3070 · 3060 Ti · 3060 · 30-series Laptop |
| **Turing** (RTX 20) | 2080 Ti · 2080 (S) · 2070 (S) · 2060 (S) · 20-series Laptop *(driver 545+)* |
| **Pascal** | P104-100 · P106 · P108 (8 GB mining cards) — Pearl, KawPow, Cryptix |
| **CMP** | 90HX · 50HX · 40HX · 30HX *(driver 545+)* |

---

## Resources

- **Website:** [forgeminer.org](https://forgeminer.org)
- **Releases & news:** [t.me/ForgeMiner](https://t.me/ForgeMiner)
- **Support & chat:** [t.me/ForgeMinerChat](https://t.me/ForgeMinerChat)
- **Discord:** [discord.gg/vxUTbb9B](https://discord.gg/vxUTbb9B)

---

<p align="center"><sub>© 2026 ForgeMiner. Not affiliated with NVIDIA. Mine responsibly.</sub></p>
