<p align="center">
  <img src="assets/forge_logo.png" width="130" alt="ForgeMiner logo">
</p>

<h1 align="center">ForgeMiner</h1>

<p align="center"><b>A fast, native NVIDIA GPU miner — Pearl (PRL), QubitCoin (QTC), KawPow (Ravencoin, Quai, Neurai), Cryptix (CYTX), BTX (btx.dev), Xelis (XEL) and Conflux (CFX)</b></p>

<p align="center">
  <a href="https://github.com/0xHashRaptor/ForgeMiner/releases"><img src="assets/badge_version.svg" alt="version 1.6.0"></a>
  <a href="#download"><img src="assets/badge_platform.svg" alt="platform: Windows | Linux | HiveOS | Docker"></a>
  <a href="#supported-gpus"><img src="assets/badge_gpu.svg" alt="GPU: NVIDIA Pascal | RTX 20/30/40/50 + CMP"></a>
</p>
<p align="center">
  <a href="https://forgeminer.org"><img src="assets/badge_site.svg" alt="site: forgeminer.org"></a>
  <a href="https://t.me/ForgeMiner"><img src="assets/badge_telegram.svg" alt="Telegram: Releases"></a>
  <a href="https://discord.gg/CyU6ASQWSy"><img src="assets/badge_discord.svg" alt="Discord: Community"></a>
</p>

---

## Contents

- [Overview](#overview)
- [Download](#download)
- [Quick start](#quick-start)
- [Coins & dev fee](#coins--dev-fee)
- [Features](#features)
- [Options](#options)
- [Monitoring API](#monitoring-api)
- [CMP hardware unlock (Linux)](#cmp-hardware-unlock-linux)
- [Supported GPUs](#supported-gpus)
- [Resources](#resources)

---

## Overview

ForgeMiner is a high-performance, fully native NVIDIA GPU miner. It talks to the GPU directly through the CUDA Driver API — no Python, no WSL, no extra runtimes — so it starts instantly and runs lean even on low-spec rigs. It mines **Pearl (PRL)**, **QubitCoin (QTC)**, **KawPow** (Ravencoin RVN, Quai QUAI, Neurai XNA), **Cryptix (CYTX)**, **BTX (btx.dev)** and **Xelis (XEL)** from a single binary — pick the coin with one flag — and more coins are on the way.

Every algorithm ships a separate per-architecture build for each supported card, auto-selected at launch, so each GPU runs at its peak. Rigs with CMP 40HX, 50HX, 70HX or 90HX get built-in hardware unlock on Linux via one command, on any kernel — see [CMP hardware unlock](#cmp-hardware-unlock-linux).

Website: **[forgeminer.org](https://forgeminer.org)** · ForgeMiner is closed-source; releases are published here and announced on [Telegram](https://t.me/ForgeMiner) and [Discord](https://discord.gg/CyU6ASQWSy).

<!-- Tip: drop a dashboard screenshot here for instant credibility, e.g. -->
<!-- <p align="center"><img src="assets/dashboard.png" width="720" alt="ForgeMiner live dashboard"></p> -->

---

## Download

Grab the latest build from the [**Releases**](https://github.com/0xHashRaptor/ForgeMiner/releases) page:

| Platform | Package |
|---|---|
| Windows | `ForgeMiner-<version>-windows.zip` |
| Linux | `ForgeMiner-<version>-linux.tar.gz` (glibc 2.17+) |
| HiveOS | `ForgeMiner-<version>.tar.gz` · flight sheets install `ForgeMiner.tar.gz`, whose URL never changes |
| Docker | `docker pull hashraptor/forge` (tags `:latest` and the version) |

---

## Quick start

### Windows
1. Download and unpack the Windows release.
2. Open the `.bat` for your coin/pool/region and set your wallet and worker. There is one folder per coin, 97 ready-made launchers in all (`-SSL` = encrypted connection):
   - `Pearl (PRL)\` — 20: Kryptex, LuckyPool, HeroMiners, BaikalMine, 2Miners. AlphaPool sits in its own folder, `Pearl (PRL) - AlphaPool\`, because it speaks a different dialect.
   - `Ravencoin (RVN)\` — 18 · `Quai (QUAI)\` — 14 · `Neurai (XNA)\` — 12
   - `Conflux (CFX)\` and `Xelis (XEL)\` — 10 each
   - `QubitCoin (QTC)\` — 6 · `BTX\` — 3 · `Cryptix (CYTX)\` — 2
3. Double-click to start. Run as Administrator to apply the built-in overclock.

### Linux
```bash
chmod +x forge
# Pearl
./forge --algorithm pearlhash --wallet YOUR_PRL_WALLET --pool prl.kryptex.network:7048 --worker rig01
# QubitCoin
./forge --algorithm qhash --wallet YOUR_QTC_WALLET --pool ru.luckypool.io:8610 --worker rig01
# KawPow — RVN / QUAI (coin auto-detected from the pool) or XNA (set --coin xna)
./forge --algorithm kawpow --wallet YOUR_RVN_WALLET --pool rvn.kryptex.network:7031 --worker rig01
# Cryptix
./forge --algorithm cryptix --wallet YOUR_CYTX_WALLET --pool cytx.baikalmine.com:9010 --worker rig01
# BTX
./forge --algorithm btx --wallet YOUR_BTX_WALLET --pool ssl://btx-eu.lproute.com:8665 --worker rig01
# Xelis
./forge --algorithm xelis --wallet YOUR_XEL_WALLET --pool xel.kryptex.network:7019 --worker rig01
# Conflux (needs a 12 GB card; see the note under the table below)
./forge --algorithm cfx --wallet YOUR_CFX_WALLET --pool cfx.kryptex.network:7027 --worker rig01
```

The Linux binary is built against **glibc 2.17**, so it runs on anything from CentOS 7 / Ubuntu 14.04 upwards — no CUDA toolkit, no extra libraries, just the NVIDIA driver. Add `--api-bind 0.0.0.0:7777` to any of the commands above and open `http://<rig>:7777` for the built-in dashboard.

### Docker
```bash
docker pull hashraptor/forge
docker run --rm --gpus all hashraptor/forge \
  --algorithm pearlhash --wallet YOUR_PRL_WALLET --worker rig01 --pool prl.kryptex.network:7048
# dashboard: add -p 7777:7777 and --api-bind 0.0.0.0:7777
```

### HiveOS
Custom miner flight sheet — installation URL `.../ForgeMiner.tar.gz`, wallet template `%WAL%.%WORKER_NAME%`. That URL always points at the current release, so an upgrade needs no editing; the versioned `ForgeMiner-<version>.tar.gz` on the Releases page is the same package pinned to one version.

*Extra config* accepts **both** forms, one per line, and you can mix them:

```
FORGE_ALGO=xelis            # or pearlhash / qhash / kawpow / cryptix / btx / cfx
FORGE_COIN=xna              # KawPow only: rvn | quai | xna
--gpu 0,1,3                 # mine only these cards
--cclk 1500 --moff 1000     # overclock: also --coff / --mclk / --plimit
--fan 70                    # or --fan-curve 45:30,60:55,70:75,80:100
--cmp-install               # unlock any CMP 40HX/50HX/70HX/90HX on the rig (safe to leave in permanently)
--temp-limit 80 --temp-resume 70   # pause a card on overheat, resume once it cools
```

Hashrate, temperatures and shares appear in the HiveOS dashboard on their own — no extra flag needed. Add `--api-bind 0.0.0.0:7777` only if you also want the miner's own web dashboard.

Ready-made flight sheets: **[forgeminer.org/#flightsheets](https://forgeminer.org/#flightsheets)**.

---

## Coins & dev fee

| Coin | `--algorithm` | Pools (ready-made `.bat` in the release) | Dev fee |
|------|---------------|-------------------------------------------|:------:|
| Pearl (PRL) | `pearlhash` | Kryptex · BaikalMine · HeroMiners · LuckyPool · 2Miners · AlphaPool | 2% |
| Cryptix (CYTX) | `cryptix` | BaikalMine · CryptixNetwork | 2% |
| QubitCoin (QTC) | `qhash` | LuckyPool · k1pool | 1% |
| BTX (btx.dev) | `btx` | LuckyPool (lproute) | 1% |
| Xelis (XEL) | `xelis` | Kryptex · HeroMiners | 1% |
| Conflux (CFX) | `cfx` | Kryptex · HeroMiners | 1.5% |
| Ravencoin (RVN) | `kawpow` | Kryptex · HeroMiners · 2Miners · RavenMiner · k1pool | 0.7% |
| Quai (QUAI) | `kawpow` `--coin quai` | Kryptex · HeroMiners · k1pool | 0.7% |
| Neurai (XNA) | `kawpow` `--coin xna` | Kryptex · 2Miners · Vipor | 0.7% |

Conflux needs about **8.6 GB of video memory**, and that requirement grows slowly as the chain
advances: 8 GB cards cannot mine it, 10 GB is tight, 12 GB and up is comfortable. The miner checks
your card at startup and says so with the actual numbers rather than failing partway through.

The dev fee is interleaved (no graph dips) and verifiable on your pool. No hidden second fee. *More algorithms are on the way.*

---

## Features

- **Multiple coins, one binary** — Pearl, QubitCoin, KawPow (RVN / QUAI / XNA), Cryptix, BTX or Xelis; select with `--algorithm`.
- **Architecture-tuned kernels** — a dedicated kernel per GPU generation (Pascal / Volta / Turing / Ampere / Ada / Blackwell), auto-selected at launch.
- **CMP 40HX / 50HX / 70HX / 90HX hardware unlock (Linux)** — one embedded command unlocks any of these cards from the stock throttled hashrate to full speed; no external scripts, no exact kernel requirement (driver 610.43.03 still required).
- **Native and lightweight** — direct CUDA Driver API, near-zero CPU load; no Python, WSL or extra runtimes. Starts in a second, runs on weak hosts and many-GPU boxes.
- **Efficient on crowded rigs** — keeps the GPUs fed even with many cards on a weak CPU, several miner instances or slow x1 risers.
- **One self-contained binary** — everything embedded; no CUDA runtime or loose kernel files to manage. Even KawPow ships as a single executable.
- **Built-in overclocking & fan control** — lock clocks, apply offsets, set a power limit and drive fans straight from the miner — a different OC per card on mixed rigs. No third-party tool.
- **Temperature protection** — `--temp-limit` / `--temp-resume` pause a card automatically if it overheats, and resume once it cools back down.
- **Multi-pool with fail-over** — standard Stratum for every coin, SSL/TLS pools, automatic reconnect and pool fail-over.
- **Live dashboard & read-only API** — the per-card table stays pinned at the top with the log scrolling underneath (press `L` for the classic view). Per-GPU hashrate, temps (incl. VRAM on Windows), clocks, fans, power and shares, plus a delivery figure showing how much of the hashrate the pool actually credits — and JSON, Prometheus and Claymore-compatible endpoints.
- **HiveOS ready** — drops straight into a custom-miner slot.

---

## Options

Anything you pass on the command line has a `FORGE_*` environment-variable twin — handy for HiveOS *Extra config* and `.bat` files.

| Flag | Env | Description |
|------|-----|-------------|
| `--algorithm` | `FORGE_ALGO` | `pearlhash`, `qhash`, `kawpow`, `cryptix`, `btx` or `xelis`. |
| `--coin` | `FORGE_COIN` | KawPow coin: `rvn`, `quai` or `xna` (auto-detected from the pool; set explicitly for Neurai / Vipor). |
| `--pool` | `FORGE_POOL` | Pool `host:port`. SSL/TLS supported; list several for fail-over. |
| `--wallet` | `FORGE_WALLET` | Payout wallet address. |
| `--worker` | `FORGE_WORKER` | Worker / rig name. |
| `--password` | `FORGE_PASS` | Pool password (usually `x`). |
| `--proto` | `FORGE_PROTO` | Pearl dialect: `stratum` or `alpha` (AlphaPool). |
| `--gpu` | `FORGE_GPU` | Mine only these indices, e.g. `0,1,2,6` (`nvidia-smi` order). |
| `--temp-limit` | `FORGE_TEMP_LIMIT` | Pause a card automatically once it hits this temperature (°C). |
| `--temp-resume` | `FORGE_TEMP_RESUME` | Resume a paused card once it cools to this temperature (°C). |
| `--oc-delay` | `FORGE_OC_DELAY` | Delay applying overclock by N seconds after startup. |
| `--log` | `FORGE_LOG` | Also write the scrolling output to a file (tables included, colours stripped). Off by default; appends. |
| `--v1-tui` | — | Start in the classic scrolling view instead of the pinned dashboard. `L` switches between them at any time. |
| `--cmp-install` | — | Linux: install hardware unlock for CMP 40HX / 50HX / 70HX / 90HX (see [below](#cmp-hardware-unlock-linux)). |
| `--cmp-verify` | — | Linux: print live unlock status per CMP card. |
| `--cmp-rollback` | — | Linux: undo the unlock, restore the previous driver. |
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

Off by default and **read-only** — it only reports stats; it can never control or reconfigure the miner.

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

## CMP hardware unlock (Linux)

CMP 40HX, 50HX, 70HX and 90HX ship with a hardware throttle that keeps their hashrate at a small fraction of what the chip can actually do. Forge can install a hardware-unlock helper directly on the rig — one command, one reboot, the card then runs at full speed. The helper is embedded in the miner binary; no external download, no manual steps.

Builds from source for whatever kernel the rig is actually running — no exact kernel version required (NVIDIA driver 610.43.03 is still required), tested clean across 10 different kernel versions. Only the detected CMP cards are touched; every other GPU on a mixed rig runs untouched. CMP 30HX is detected but never attempted — it's a permanent hardware limitation on that chip, not a bug or an oversight.

| Command | What it does |
|---|---|
| `sudo forge --cmp-install` | install unlock helper, reboot with 10-second cancel window |
| `sudo forge --cmp-install --no-reboot` | install only, you reboot the rig yourself |
| `sudo forge --cmp-verify` | print live unlock status per CMP card |
| `sudo forge --cmp-rollback` | restore the previous driver from backup, reboot |

Safe to leave `--cmp-install` at the end of the flight-sheet command line permanently — if the unlock is already active, forge prints one line and drops straight into mining. Full guide (verification, FAQ): **[wiki · CMP hardware unlock](https://github.com/0xHashRaptor/ForgeMiner/wiki/CMP-unlock)**.

---

## Supported GPUs

Kernels are tuned per architecture, so a whole generation is covered — desktop and laptop alike.

| Generation | Cards |
|---|---|
| **Blackwell** (RTX 50) | 5090 · 5080 · 5070 Ti · 5070 · 5060 Ti · 5060 · 50-series Laptop *(driver 580+)* |
| **Ada** (RTX 40) | 4090 · 4080 (S) · 4070 Ti (S) · 4070 (S) · 4060 Ti · 4060 · 40-series Laptop |
| **Ampere** (RTX 30) | 3090 Ti · 3090 · 3080 Ti · 3080 · 3070 Ti · 3070 · 3060 Ti · 3060 · 30-series Laptop |
| **Turing** (RTX 20) | 2080 Ti · 2080 (S) · 2070 (S) · 2060 (S) · 20-series Laptop *(driver 545+)* |
| **Volta** | Tesla V100 |
| **Pascal** | GTX 10-series · P104-100 · P106 · P108 (8 GB mining cards) |
| **CMP** | 170HX · 90HX · 70HX · 50HX · 40HX · 30HX *(driver 545+)*. 40HX/50HX/70HX/90HX get built-in [hardware unlock](#cmp-hardware-unlock-linux) on Linux; 30HX is a permanent hardware limitation, never supported; 170HX needs a separate tool. |

*All coins run on every listed generation.*

---

## Resources

- **Website:** [forgeminer.org](https://forgeminer.org)
- **Releases & news:** [t.me/ForgeMiner](https://t.me/ForgeMiner)
- **Support & chat:** [t.me/ForgeMinerChat](https://t.me/ForgeMinerChat)
- **Discord:** [discord.gg/CyU6ASQWSy](https://discord.gg/CyU6ASQWSy)
- **CMP hardware unlock guide:** [wiki · CMP hardware unlock](https://github.com/0xHashRaptor/ForgeMiner/wiki/CMP-unlock)

---

<p align="center"><sub>© 2026 ForgeMiner. Not affiliated with NVIDIA. Mine responsibly.</sub></p>
