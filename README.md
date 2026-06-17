<p align="center">
  <img src="assets/forge_logo.png" width="130" alt="ForgeMiner logo">
</p>

<h1 align="center">ForgeMiner</h1>

<p align="center"><b>A fast, native NVIDIA GPU miner — Pearl (PRL) and QubitCoin (QTC), more soon</b></p>

<p align="center">
  <a href="https://github.com/0xHashRaptor/ForgeMiner/releases"><img src="https://img.shields.io/badge/version-1.1.1-orange.svg"></a>
  <a href="#quick-start"><img src="https://img.shields.io/badge/platform-Windows%20%7C%20Linux%20%7C%20HiveOS-blue.svg"></a>
  <a href="#supported-algorithms"><img src="https://img.shields.io/badge/GPU-NVIDIA%20RTX%2020%2F30%2F40%2F50%20%2B%20CMP-76b900.svg"></a>
  <a href="https://t.me/ForgeMiner"><img src="https://img.shields.io/badge/Telegram-Releases-26A5E4.svg?logo=telegram"></a>
</p>

---

## Overview

ForgeMiner is a high-performance, fully native NVIDIA GPU miner. It talks to the GPU directly through the CUDA Driver API — no Python, no WSL, no extra runtimes — so it starts instantly and runs lean even on low-spec rigs. It mines **Pearl (PRL)** and **QubitCoin (QTC)** from a single binary — pick the coin with one flag — and more coins are on the way.

Every algorithm ships a separate per-architecture build for each supported card, auto-selected at launch, so each GPU runs at its peak.

> ForgeMiner is closed-source. Releases are published here and announced on the [Telegram channel](https://t.me/ForgeMiner).

---

## Features

- **Two coins, one binary** — mine Pearl (PRL) or QubitCoin (QTC); select with `--algorithm`. More coins coming.
- **Architecture-tuned kernels** — a dedicated kernel per GPU generation (Turing / Ampere / Ada / Blackwell), auto-selected at launch.
- **Efficient on crowded rigs** — keeps the GPUs fed even with many cards on a weak CPU, several miner instances, or slow x1 risers.
- **Native and lightweight** — direct CUDA Driver API, near-zero CPU load (blocking-sync design); runs great on weak hosts and many-GPU boxes.
- **Self-contained and protected** — a single binary with everything embedded and encrypted; no loose kernel files to manage or leak.
- **Built-in overclocking** — lock clocks and apply core/memory offsets and a power limit straight from the miner; no third-party OC tool required.
- **Per-GPU control** — choose which cards to mine (`--gpu`) and set a different overclock per card on mixed rigs.
- **Multi-pool with fail-over** — standard Stratum pools for both coins; automatic reconnect and pool fail-over.
- **Clean live dashboard** — per-GPU hashrate, accepted/stale/rejected shares, efficiency, temperatures, clocks, fans and power at a glance.
- **HiveOS ready** — drops straight into a HiveOS custom-miner slot.

See the [Releases](https://github.com/0xHashRaptor/ForgeMiner/releases) page for version history and the latest changes.

---

## Quick start

### Windows
1. Download and unpack the Windows release.
2. Open the `.bat` for your coin/pool in a text editor and set your wallet and worker name:
   - **Pearl:** `Baikal.bat`, `HeroMiners.bat`, `LuckyPool.bat`, `AlphaPool.bat`
   - **QubitCoin:** `qhash_LuckyPool_RU.bat`, `qhash_LuckyPool_CA.bat`, `qhash_k1pool_RU.bat`, `qhash_k1pool_EU.bat`
3. Double-click the `.bat` to start mining. (Run as Administrator if you want the built-in overclock to apply.)

### Linux
```bash
chmod +x forge
# Pearl
FORGE_POOL=ru.pearl.herominers.com:1200 FORGE_WALLET=YOUR_PRL_WALLET FORGE_WORKER=rig01 FORGE_PROTO=stratum ./forge
# QubitCoin (qhash)
./forge --algorithm qhash --wallet YOUR_QTC_WALLET --worker rig01 --pool ru.luckypool.io:8610
```
…or use the included `start.sh` (Pearl) / `start-qhash.sh` (QubitCoin) after editing your wallet.

### HiveOS
Add a Custom miner flight sheet:

| Field | Pearl | QubitCoin (qhash) |
|---|---|---|
| Installation URL | `https://github.com/0xHashRaptor/ForgeMiner/releases/download/v1.1.1/ForgeMiner-1.1.1.tar.gz` | same URL |
| Wallet template | `%WAL%.%WORKER_NAME%` (Pearl wallet) | your QTC address `.%WORKER_NAME%` |
| Pool URL | `pearl.baikalmine.com:2010` · `ru.pearl.herominers.com:1200` · `prl-ru.kryptex.network:7048` | `ru.luckypool.io:8610` |
| Pass | `x` | `x` |
| Extra config | *empty for Stratum* · `FORGE_PROTO=alpha` for AlphaPool · OC e.g. `FORGE_CCLK=2505` | **`FORGE_ALGO=qhash`** · OC e.g. `FORGE_CCLK=2505` |

Apply → the dashboard shows per-GPU hashrate, temperatures, fans and shares. Run one miner per rig.

---

## Options

Options can be passed as command-line flags (`--flag value`) or as environment variables (`FORGE_FLAG`) — handy for HiveOS and `.bat` files.

| Flag | Env variable | Description |
|------|--------------|-------------|
| `--algorithm` | `FORGE_ALGO` | Algorithm to mine: `pearl` or `qhash` (QubitCoin). |
| `--pool` | `FORGE_POOL` | Pool address as `host:port`. |
| `--wallet` | `FORGE_WALLET` | Your payout wallet address. |
| `--worker` | `FORGE_WORKER` | Worker / rig name shown on the pool. |
| `--password` | `FORGE_PASS` | Pool password (usually `x`). |
| `--proto` | `FORGE_PROTO` | Pearl pool dialect: `stratum` (HeroMiners, LuckyPool, Kryptex) or `alpha` (AlphaPool). *(Pearl only.)* |
| `--gpu` | `FORGE_GPU` | Mine only these GPU indices, e.g. `--gpu 0,1,2,6` (default: all GPUs). Indices match `nvidia-smi` order. |
| — | `FORGE_LOWVRAM` | Low-VRAM mode for 8 GB cards (Pearl). `1` force on, `0` force off. Auto-detected by default. |

### Overclocking

> **Tip:** ForgeMiner is core-clock bound and memory-light — for the best hashrate set the core high; memory can stay low. Overclocking requires root (Linux/HiveOS) or Administrator (Windows).
>
> **Per-GPU OC:** each flag takes a single value (applied to all cards) or a comma list that maps to `--gpu`:
> `--gpu 0,1,2,6 --coff 300,250,300,200 --plimit 280,280,300,260`

| Flag | Env variable | Description |
|------|--------------|-------------|
| `--cclk` | `FORGE_CCLK` | Lock GPU core clock (MHz). |
| `--coff` | `FORGE_COFF` | GPU core clock offset (MHz, `+`/`-`). |
| `--mclk` | `FORGE_MCLK` | Lock memory clock (MHz). |
| `--moff` | `FORGE_MOFF` | Memory clock offset (MHz, `+`/`-`). |
| `--plimit` | `FORGE_PLIMIT` | Power limit (Watts). |

---

## Supported algorithms

| Algorithm | Coin | Dev fee |
|-----------|------|:-------:|
| PearlHash | Pearl (PRL) | 3% |
| qhash | QubitCoin (QTC) | 2.5% |

*More algorithms are coming — follow the channel for updates.*

Supported GPUs: NVIDIA RTX 20 (Turing), RTX 30 (Ampere), RTX 40 (Ada), RTX 50 (Blackwell), and NVIDIA CMP mining cards (e.g. CMP 50HX). *(RTX 20-series and CMP cards need driver 545+.)*

---

## Resources

- Releases and news: https://t.me/ForgeMiner
- Support and chat: https://t.me/ForgeMinerChat

---

<p align="center"><sub>© 2026 ForgeMiner. Not affiliated with NVIDIA. Mine responsibly.</sub></p>
