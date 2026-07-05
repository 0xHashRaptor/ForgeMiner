<p align="center">
  <img src="assets/forge_logo.png" width="130" alt="ForgeMiner logo">
</p>

<h1 align="center">ForgeMiner</h1>

<p align="center"><b>A fast, native NVIDIA GPU miner — Pearl (PRL), QubitCoin (QTC), KawPow (Ravencoin, Quai, Neurai) and Cryptix (CYTX)</b></p>

<p align="center">
  <a href="https://github.com/0xHashRaptor/ForgeMiner/releases"><img src="https://img.shields.io/badge/version-1.3.3-orange.svg"></a>
  <a href="#quick-start"><img src="https://img.shields.io/badge/platform-Windows%20%7C%20Linux%20%7C%20HiveOS-blue.svg"></a>
  <a href="#supported-algorithms"><img src="https://img.shields.io/badge/GPU-NVIDIA%20Pascal%20%7C%20RTX%2020%2F30%2F40%2F50%20%2B%20CMP-76b900.svg"></a>
  <a href="https://t.me/ForgeMiner"><img src="https://img.shields.io/badge/Telegram-Releases-26A5E4.svg?logo=telegram"></a>
  <a href="https://discord.gg/vxUTbb9B"><img src="https://img.shields.io/badge/Discord-Community-5865F2.svg?logo=discord&logoColor=white"></a>
</p>

---

## Overview

ForgeMiner is a high-performance, fully native NVIDIA GPU miner. It talks to the GPU directly through the CUDA Driver API — no Python, no WSL, no extra runtimes — so it starts instantly and runs lean even on low-spec rigs. It mines **Pearl (PRL)**, **QubitCoin (QTC)**, **KawPow** (Ravencoin RVN, Quai QUAI, Neurai XNA) and **Cryptix (CYTX)** from a single binary — pick the coin with one flag — and more coins are on the way.

Every algorithm ships a separate per-architecture build for each supported card, auto-selected at launch, so each GPU runs at its peak.

> ForgeMiner is closed-source. Releases are published here and announced on the [Telegram channel](https://t.me/ForgeMiner) and on [Discord](https://discord.gg/vxUTbb9B).

---

## Features

- **Multiple coins, one binary** — mine Pearl (PRL), QubitCoin (QTC), KawPow (Ravencoin / Quai / Neurai) or Cryptix (CYTX); select with `--algorithm`. More coins coming.
- **Architecture-tuned kernels** — a dedicated kernel per GPU generation (Pascal / Turing / Ampere / Ada / Blackwell), auto-selected at launch.
- **Efficient on crowded rigs** — keeps the GPUs fed even with many cards on a weak CPU, several miner instances, or slow x1 risers.
- **Native and lightweight** — direct CUDA Driver API, near-zero CPU load (blocking-sync design); runs great on weak hosts and many-GPU boxes.
- **Truly self-contained** — one executable with everything embedded and encrypted; no CUDA runtime, no NVRTC, no loose kernel or library files to manage or leak — even KawPow ships as a single `.exe`.
- **Built-in overclocking and fan control** — lock clocks, apply core/memory offsets, set a power limit and control fans straight from the miner; no third-party OC tool required.
- **Per-GPU control** — choose which cards to mine (`--gpu`) and set a different overclock per card on mixed rigs.
- **Multi-pool with fail-over** — standard Stratum pools for both coins; automatic reconnect and pool fail-over.
- **Clean live dashboard** — per-GPU hashrate, accepted/stale/rejected shares, efficiency, temperatures (incl. VRAM on Windows), clocks, fans and power at a glance.
- **HiveOS ready** — drops straight into a HiveOS custom-miner slot.

See the [Releases](https://github.com/0xHashRaptor/ForgeMiner/releases) page for version history and the latest changes.

---

## Quick start

### Windows
1. Download and unpack the Windows release.
2. Open the `.bat` for your coin/pool/region in a text editor and set your wallet and worker name. Files are named `<algo>_<pool>_<region>.bat` (`_SSL` = encrypted connection):
   - **Pearl:** `pearlhash_Baikal_Global.bat`, `pearlhash_HeroMiners_DE.bat`, `pearlhash_Kryptex_RU.bat`, `pearlhash_LuckyPool_EU.bat`, `pearlhash_AlphaPool_EU.bat`, …
   - **QubitCoin:** `qhash_LuckyPool_RU.bat`, `qhash_LuckyPool_CA.bat`, `qhash_k1pool_RU.bat`, `qhash_k1pool_EU.bat`
   - **KawPow (Ravencoin / Quai):** `kawpow_RVN_Kryptex_Global.bat`, `kawpow_RVN_HeroMiners_US.bat`, `kawpow_RVN_2Miners_EU.bat`, `kawpow_QUAI_HeroMiners_DE.bat`, `kawpow_QUAI_Kryptex_EU.bat`, …
   - **KawPow (Neurai XNA):** `kawpow_XNA_Kryptex_Global.bat`, `kawpow_XNA_Kryptex_RU.bat`, `kawpow_XNA_Kryptex_EU.bat`, `kawpow_XNA_Vipor_RU.bat`, `kawpow_XNA_Vipor_DE.bat`, `kawpow_XNA_2Miners_EU.bat`, `kawpow_XNA_2Miners_US.bat`
   - **Cryptix (CYTX):** `cryptix_Baikalmine_Global.bat`, `cryptix_CryptixNetwork_Global.bat`
3. Double-click the `.bat` to start mining. (Run as Administrator if you want the built-in overclock to apply.)

### Linux
```bash
chmod +x forge
# Pearl
FORGE_POOL=ru.pearl.herominers.com:1200 FORGE_WALLET=YOUR_PRL_WALLET FORGE_WORKER=rig01 FORGE_PROTO=stratum ./forge
# QubitCoin (qhash)
./forge --algorithm qhash --wallet YOUR_QTC_WALLET --worker rig01 --pool ru.luckypool.io:8610
# KawPow — Ravencoin (RVN) or Quai (QUAI); coin auto-detected from the pool host
./forge --algorithm kawpow --wallet YOUR_RVN_WALLET --worker rig01 --pool us.ravencoin.herominers.com:1140
# KawPow — Neurai (XNA); set the coin explicitly
./forge --algorithm kawpow --coin xna --wallet YOUR_XNA_WALLET --worker rig01 --pool xna.2miners.com:6060
# Cryptix (CYTX)
./forge --algorithm cryptix --wallet YOUR_CYTX_WALLET --worker rig01 --pool cytx.baikalmine.com:9010
```
…or use the included `start.sh` (Pearl) / `start-qhash.sh` (QubitCoin) after editing your wallet.

### HiveOS
Add a Custom miner flight sheet:

| Field | Pearl | QubitCoin (qhash) |
|---|---|---|
| Installation URL | `https://github.com/0xHashRaptor/ForgeMiner/releases/download/v1.3.3/ForgeMiner-1.3.3.tar.gz` | same URL |
| Wallet template | `%WAL%.%WORKER_NAME%` (Pearl wallet) | your QTC address `.%WORKER_NAME%` |
| Pool URL | `pearl.baikalmine.com:2010` · `ru.pearl.herominers.com:1200` · `prl-ru.kryptex.network:7048` | `ru.luckypool.io:8610` |
| Pass | `x` | `x` |
| Extra config | *empty for Stratum* · `FORGE_PROTO=alpha` for AlphaPool · OC e.g. `FORGE_CCLK=2505` | **`FORGE_ALGO=qhash`** · OC e.g. `FORGE_CCLK=2505` |

For **KawPow** set `FORGE_ALGO=kawpow` in *Extra config* and use a Ravencoin, Quai or Neurai pool — e.g. `us.ravencoin.herominers.com:1140` (RVN), `de.quai.herominers.com:1185` (QUAI) or `xna.2miners.com:6060` (XNA). The coin is auto-detected from the pool host; override with `FORGE_COIN=rvn` / `FORGE_COIN=quai` / `FORGE_COIN=xna` if needed (Neurai/Vipor pools need it set explicitly).

For **Cryptix (CYTX)** set `FORGE_ALGO=cryptix` in *Extra config*, use your `cryptix:…` address as the wallet and a Cryptix pool — e.g. `cytx.baikalmine.com:9010`.

Apply → the dashboard shows per-GPU hashrate, temperatures, fans and shares. Run one miner per rig.

---

## Options

Options can be passed as command-line flags (`--flag value`) or as environment variables (`FORGE_FLAG`) — handy for HiveOS and `.bat` files.

| Flag | Env variable | Description |
|------|--------------|-------------|
| `--algorithm` | `FORGE_ALGO` | Algorithm to mine: `pearl`, `qhash` (QubitCoin), `kawpow` (Ravencoin / Quai / Neurai) or `cryptix` (CYTX). |
| `--coin` | `FORGE_COIN` | KawPow coin: `rvn`, `quai` or `xna` (auto-detected from the pool host if omitted; set explicitly for Neurai / Vipor pools). *(KawPow only.)* |
| `--pool` | `FORGE_POOL` | Pool address as `host:port`. |
| `--wallet` | `FORGE_WALLET` | Your payout wallet address. |
| `--worker` | `FORGE_WORKER` | Worker / rig name shown on the pool. |
| `--password` | `FORGE_PASS` | Pool password (usually `x`). |
| `--proto` | `FORGE_PROTO` | Pearl pool dialect: `stratum` (HeroMiners, LuckyPool, Kryptex) or `alpha` (AlphaPool). *(Pearl only.)* |
| `--efficient` | `FORGE_PEARL_EFFICIENT` | Pearl power-efficient mode (RTX 40 / Ada): noticeably lower power for nearly the same hashrate. Off by default (default targets maximum hashrate); on other GPUs it falls back to the default. *(Pearl only.)* |
| `--gpu` | `FORGE_GPU` | Mine only these GPU indices, e.g. `--gpu 0,1,2,6` (default: all GPUs). Indices match `nvidia-smi` order. |
| — | `FORGE_LOWVRAM` | Low-VRAM mode for 8 GB cards (Pearl). `1` force on, `0` force off. Auto-detected by default. |

### Overclocking and fans

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
| `--fan` | `FORGE_FAN` | Fixed fan speed (%). Use `--fan-curve t:p,t:p,...` for a custom temperature-to-speed curve. |

### Fan control

Control the GPU fans yourself — a fixed speed, or a temperature curve. With no fan flag, ForgeMiner leaves the
fans on the driver's automatic control. Setting fans requires Administrator (Windows) or root (Linux/HiveOS).
On exit the driver's automatic fan control is restored.

| Flag | Env variable | Description |
|------|--------------|-------------|
| `--fan` | `FORGE_FAN` | Fixed fan speed in % (one value = all cards, or a comma list mapped to `--gpu`). |
| `--fan-curve` | `FORGE_FANCURVE` | Temperature → speed curve: `temp:percent` points, linear-interpolated. |

**Fixed speed**

```
--fan 70                 every GPU at 70%
--fan 60,70,80,100       per-GPU (maps to --gpu order: GPU0=60%, GPU1=70%, ...)
```

**Temperature curve** — the recommended way. Give `temp°C:fan%` points; ForgeMiner reads each card's live core
temperature and sets its fan to the matching percent, interpolating linearly between the points:

```
--fan-curve 45:30,60:55,70:75,80:100
```

reads as: ≤45 °C → 30% · 60 °C → 55% · 70 °C → 75% · ≥80 °C → 100% · in-between (e.g. 65 °C) → ~65%.
Use as many points as you like (they're sorted automatically). One curve applies to all selected GPUs. A fixed
`--fan` for a card overrides the curve for that card.

> **Note:** GeForce cards have a ~30% hardware fan floor — a request below that just lands at 30%.

**HiveOS / Linux** — put it in the flight-sheet *Extra config arguments*, or use the env form:

```
--fan-curve 50:40,65:60,75:85,83:100
FORGE_FANCURVE=50:40,65:60,75:85,83:100      (env equivalent; FORGE_FAN=70 for a fixed speed)
```

---

## Monitoring API

ForgeMiner has a built-in **read-only** monitoring API and web dashboard. It is **off by default**, and it only ever reports stats — it never lets anyone control or reconfigure the miner.

**Enable it**

```
--api                      on 127.0.0.1:7777 (this machine only)
--api-bind 0.0.0.0:7777    on all interfaces (watch from your phone or another PC on the LAN)
--api-bind 127.0.0.1:9000  any address/port you like
```

On HiveOS / mining OS, set it in *Extra config*: `FORGE_API=127.0.0.1:7777` (or `0.0.0.0:7777`). Everything below is served from that one port.

**Web dashboard** — open `http://<ip>:7777` in a browser

A live panel that refreshes on its own: total hashrate, 1h / 24h averages, power, efficiency, accept rate and shares; a live hashrate graph with a scale on the left; and a per-GPU table — card name, hashrate, temperature (and VRAM temp), core clock, memory clock, fan, power, efficiency, accepted / stale / rejected, with a **Total** row — plus pool, worker and wallet. It has a **dark / light theme** toggle and a **selectable refresh rate** (1s … 30s), both remembered by your browser. Nothing to install — it is built into the miner.

**Endpoints**

| Endpoint | Format | Use it with |
|---|---|---|
| `GET /` | HTML | The web dashboard (alias `/dashboard`). |
| `GET /summary` (or `/stats`) | JSON | Grafana, custom dashboards, bots and scripts. Everything: total + per-GPU hashrate, temps, core/memory clocks, fans, power, efficiency, accepted/stale/rejected, accept rate, shares/min, difficulty, uptime, pool/worker/wallet, and a short hashrate history for graphs. |
| `GET /metrics` | Prometheus | Grafana dashboards and alerts (`forge_hashrate_hs`, `forge_gpu_temperature_celsius`, `forge_gpu_fan_percent`, `forge_gpu_power_watts`, `forge_shares_total`, …). |
| `miner_getstat1` (JSON-RPC, same port) | Claymore | **Awesome Miner, mmpOS** and the wider monitoring ecosystem — add forge as a custom / managed miner with the **Claymore / Ethminer** API on port **7777**. It shows up out of the box. |

HiveOS is already covered by the built-in stats integration and needs none of this.

> **Security:** the API is read-only and exposes only public info (hashrate, temperatures, pool, wallet). Keep the default `127.0.0.1` for local-only access; use `0.0.0.0` only inside your own network, behind a router / firewall / VPN.

---

## Supported algorithms

| Algorithm | Coin | Dev fee |
|-----------|------|:-------:|
| PearlHash | Pearl (PRL) | 2% |
| qhash | QubitCoin (QTC) | 1% |
| KawPow | Ravencoin (RVN), Quai (QUAI), Neurai (XNA) | 0.7% |
| Cryptix | Cryptix (CYTX) | 2% |

*More algorithms are coming — follow the channel for updates.*

Supported GPUs: NVIDIA Pascal P104-100 8 GB (Pearl, KawPow and Cryptix), RTX 20 (Turing), RTX 30 (Ampere), RTX 40 (Ada), RTX 50 (Blackwell), and NVIDIA CMP mining cards (e.g. CMP 50HX). *(RTX 20-series and CMP cards need driver 545+.)*

---

## Resources

- Releases and news: https://t.me/ForgeMiner
- Support and chat: https://t.me/ForgeMinerChat
- Discord community: https://discord.gg/vxUTbb9B

---

<p align="center"><sub>© 2026 ForgeMiner. Not affiliated with NVIDIA. Mine responsibly.</sub></p>
