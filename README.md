<h1 align="center">⚒️ ForgeMiner</h1>

<p align="center"><b>A fast, native NVIDIA GPU miner — first coin: Pearl (PRL), more soon</b></p>

<p align="center">
  <img src="https://img.shields.io/badge/version-1.0.5-orange.svg">
  <img src="https://img.shields.io/badge/platform-Windows%20%7C%20Linux%20%7C%20HiveOS-blue.svg">
  <img src="https://img.shields.io/badge/GPU-NVIDIA%20RTX%2020%2F30%2F40%2F50-76b900.svg">
  <a href="https://t.me/ForgeMiner"><img src="https://img.shields.io/badge/Telegram-Releases-26A5E4.svg?logo=telegram"></a>
</p>

---

## ⚒️ Overview

**ForgeMiner** is a high-performance, fully native NVIDIA GPU miner. It talks to the GPU directly through the CUDA Driver API — **no Python, no WSL, no extra runtimes** — so it starts instantly and runs lean even on low-spec rigs. **Its first supported coin is Pearl (PRL)** (Proof-of-Useful-Work); more coins are on the way.

The Pearl kernel is a hand-tuned tensor-core engine, with a separate per-architecture build for every supported card so each GPU runs at its peak.

> ForgeMiner is closed-source. Releases are published on our [Telegram channel](https://t.me/ForgeMiner).

---

## ✨ Miner Features

- 🚀 **Architecture-tuned kernels** — a dedicated kernel per GPU generation (Turing / Ampere / Ada / Blackwell), auto-selected at launch. Class-leading on RTX 40-series.
- 🆕 **v1.0.4** — pick which GPUs to mine (`--gpu 0,1,2,6`) and set a **different overclock per card**; dashboard stability fix for long runs.
- 🆕 **v1.0.3** — RTX **20-series (Turing) now mines on Stratum pools** (HeroMiners / LuckyPool / Kryptex), not just AlphaPool; **~4% faster** on RTX 30 / 40 / 50.
- ⚡ **Native & lightweight** — direct CUDA Driver API, near-zero CPU load (blocking-sync design), runs great on weak rigs and many-GPU boxes.
- 🔒 **Self-contained & protected** — a single binary with everything embedded and encrypted; no loose kernel files to manage or leak.
- 🔥 **Built-in overclocking** — lock clocks and apply core/memory offsets and a power limit straight from the miner (no third-party OC tool required).
- 🌐 **Multi-pool & dialects** — works with standard Pearl Stratum pools (BaikalMine, HeroMiners, LuckyPool, Kryptex) and the AlphaPool dialect out of the box.
- 🖥️ **Clean live dashboard** — per-GPU hashrate, accepted/stale/rejected shares, efficiency (GH/W), temps, clocks, fans and power at a glance.
- 🛠️ **HiveOS ready** — drops straight into a HiveOS custom-miner slot.
- ♻️ **Resilient** — automatic reconnect and pool fail-over; the mining pipeline never stalls.

---

## 🚀 Quick Start

### 🪟 Windows
1. Download and unpack the **Windows** release.
2. Open the `.bat` for your pool (`Baikal.bat`, `HeroMiners.bat`, `LuckyPool.bat`, `AlphaPool.bat`) in a text editor and set your **wallet** and **worker** name.
3. Double-click the `.bat` to start mining. *(Run as Administrator if you want the built-in overclock to apply.)*

### 🐧 Linux
```bash
chmod +x forge
FORGE_POOL=ru.pearl.herominers.com:1200 \
FORGE_WALLET=YOUR_WALLET FORGE_WORKER=rig01 \
FORGE_PROTO=stratum ./forge
```
…or use the included `start.sh` after editing your wallet.

### ⛏️ HiveOS
Add a **Custom miner** flight sheet with these fields:

| Field | Value |
|---|---|
| **Miner name** | `ForgeMiner` *(auto-fills from the URL)* |
| **Installation URL** | `https://github.com/0xHashRaptor/ForgeMiner/releases/download/v1.0.5/ForgeMiner-1.0.5.tar.gz` |
| **Hash algorithm** | *(leave empty)* |
| **Wallet and worker template** | `%WAL%.%WORKER_NAME%` *(with a Pearl wallet attached)* — or hard-code `YOUR_WALLET.%WORKER_NAME%` |
| **Pool URL** | `pearl.baikalmine.com:2010` *(BaikalMine, 0.5% fee)* · `ru.pearl.herominers.com:1200` *(HeroMiners)* · `45.151.62.119:3361` *(LuckyPool)* · `prl-ru.kryptex.network:7048` *(Kryptex)* |
| **Pass** | `x` |
| **Extra config arguments** | *empty for Stratum* · `FORGE_PROTO=alpha` for AlphaPool · OC e.g. `FORGE_CCLK=2505` |

Apply → the dashboard shows per-GPU hashrate, temps, fans and shares. Run **one** miner per rig.

---

## ⚙️ Miner Options

Options can be passed as **command-line flags** *(`--flag value`)* **or** as **environment variables** *(`FORGE_FLAG`)* — handy for HiveOS and `.bat` files.

| Flag | Env variable | Description |
|------|--------------|-------------|
| `--algorithm` | `FORGE_ALGO` | Algorithm to mine (`pearl`). |
| `--pool` | `FORGE_POOL` | Pool address as `host:port`. |
| `--wallet` | `FORGE_WALLET` | Your payout wallet address. |
| `--worker` | `FORGE_WORKER` | Worker / rig name shown on the pool. |
| `--password` | `FORGE_PASS` | Pool password (usually `x`). |
| `--proto` | `FORGE_PROTO` | Pool dialect: `stratum` (HeroMiners, LuckyPool, Kryptex) or `alpha` (AlphaPool). |
| `--gpu` | `FORGE_GPU` | Mine only these GPU indices, e.g. `--gpu 0,1,2,6` (default: all GPUs). |

### 🔥 Overclocking
> 💡 **Tip:** ForgeMiner is **core-clock bound and memory-light** — for the best hashrate set the **core high**; memory can stay low. Overclocking requires **root** (Linux/HiveOS) or **Administrator** (Windows).
>
> 🆕 **Per-GPU OC:** each flag takes a single value (applied to all cards) **or a comma list** that maps to `--gpu` — so mixed-card rigs can OC each card differently:
> `--gpu 0,1,2,6 --coff 300,250,300,200 --plimit 280,280,300,260`

| Flag | Env variable | Description |
|------|--------------|-------------|
| `--cclk` | `FORGE_CCLK` | Lock GPU core clock (MHz). |
| `--coff` | `FORGE_COFF` | GPU core clock offset (MHz, `+`/`-`). |
| `--mclk` | `FORGE_MCLK` | Lock memory clock (MHz). |
| `--moff` | `FORGE_MOFF` | Memory clock offset (MHz, `+`/`-`). |
| `--plimit` | `FORGE_PLIMIT` | Power limit (Watts). |

---

## 📊 Supported Algorithms

| Algorithm | Coin | Dev fee |
|-----------|------|:-------:|
| **PearlHash** | Pearl (PRL) | **3%** |

*More algorithms are coming — follow the channel for updates.*

**Supported GPUs:** NVIDIA RTX 20 (Turing), RTX 30 (Ampere), RTX 40 (Ada), RTX 50 (Blackwell). *(RTX 20-series needs driver 545+.)*

---

## 🔗 Resources

- 📣 **Releases & news:** https://t.me/ForgeMiner
- 💬 **Support & chat:** https://t.me/ForgeMinerChat

---

## 💼 Developer Fee

Built-in developer fee: **3%**.

---

<p align="center"><sub>© 2026 ForgeMiner. Not affiliated with NVIDIA. Mine responsibly.</sub></p>
