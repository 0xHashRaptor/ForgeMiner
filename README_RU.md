<p align="center">
  <img src="assets/forge_logo.png" width="130" alt="ForgeMiner logo">
</p>

<h1 align="center">ForgeMiner</h1>

<p align="center"><b>Быстрый нативный майнер для NVIDIA — Pearl (PRL) и QubitCoin (QTC), скоро другие</b></p>

<p align="center">
  <a href="https://github.com/0xHashRaptor/ForgeMiner/releases"><img src="https://img.shields.io/badge/version-1.1.1-orange.svg"></a>
  <a href="#быстрый-старт"><img src="https://img.shields.io/badge/platform-Windows%20%7C%20Linux%20%7C%20HiveOS-blue.svg"></a>
  <a href="#поддерживаемые-алгоритмы"><img src="https://img.shields.io/badge/GPU-NVIDIA%20RTX%2020%2F30%2F40%2F50%20%2B%20CMP-76b900.svg"></a>
  <a href="https://t.me/ForgeMiner"><img src="https://img.shields.io/badge/Telegram-Releases-26A5E4.svg?logo=telegram"></a>
</p>

> Это русский перевод. Релизный README — на английском ([README.md](README.md)).

---

## Обзор

ForgeMiner — высокопроизводительный полностью нативный майнер для видеокарт NVIDIA. Он работает с видеокартой напрямую через CUDA Driver API — без Python, без WSL, без лишних рантаймов — поэтому запускается мгновенно и потребляет минимум ресурсов даже на слабых ригах. Он майнит **Pearl (PRL)** и **QubitCoin (QTC)** из одного бинаря — монета выбирается одним флагом — и скоро будут другие.

Под каждый алгоритм идёт отдельная сборка под каждое поколение карт, выбирается автоматически при запуске, чтобы каждый GPU работал на максимуме.

> ForgeMiner — закрытый (исходники не публикуются). Релизы выходят здесь и анонсируются в [Telegram-канале](https://t.me/ForgeMiner).

---

## Возможности

- **Две монеты, один бинарь** — майните Pearl (PRL) или QubitCoin (QTC); выбор через `--algorithm`. Скоро другие монеты.
- **Ядра под архитектуру** — отдельное ядро под каждое поколение (Turing / Ampere / Ada / Blackwell), выбирается автоматически при запуске.
- **Эффективен на загруженных ригах** — держит карты «накормленными» даже при множестве карт на слабом CPU, нескольких инстансах или медленных x1-райзерах.
- **Нативный и лёгкий** — прямой CUDA Driver API, почти нулевая нагрузка на CPU (blocking-sync); отлично работает на слабых хостах и многокарточных ригах.
- **Самодостаточный и защищённый** — единый бинарь, всё вшито и зашифровано; никаких отдельных файлов-ядер.
- **Встроенный разгон** — лок частот, оффсеты ядра/памяти и лимит мощности прямо из майнера; сторонние OC-утилиты не нужны.
- **Управление по картам** — выбор карт (`--gpu`) и разный разгон на каждую карту для смешанных ригов.
- **Несколько пулов с переключением** — стандартные Stratum-пулы для обеих монет; автопереподключение и переключение между пулами при отвале.
- **Наглядная панель** — по каждой карте: хешрейт, шары, эффективность, температуры, частоты, вентиляторы и мощность.
- **Готов к HiveOS** — ставится в кастомный слот HiveOS.

История версий и последние изменения — на странице [Releases](https://github.com/0xHashRaptor/ForgeMiner/releases).

---

## Быстрый старт

### Windows
1. Скачайте и распакуйте релиз для Windows.
2. Откройте `.bat` своей монеты/пула в текстовом редакторе и впишите кошелёк и имя воркера:
   - **Pearl:** `Baikal.bat`, `HeroMiners.bat`, `LuckyPool.bat`, `AlphaPool.bat`
   - **QubitCoin:** `qhash_LuckyPool_RU.bat`, `qhash_LuckyPool_CA.bat`, `qhash_k1pool_RU.bat`, `qhash_k1pool_EU.bat`
3. Запустите `.bat` двойным кликом. (От имени Администратора — если нужен встроенный разгон.)

### Linux
```bash
chmod +x forge
# Pearl
FORGE_POOL=ru.pearl.herominers.com:1200 FORGE_WALLET=ВАШ_PRL_КОШЕЛЁК FORGE_WORKER=rig01 FORGE_PROTO=stratum ./forge
# QubitCoin (qhash)
./forge --algorithm qhash --wallet ВАШ_QTC_КОШЕЛЁК --worker rig01 --pool ru.luckypool.io:8610
```
…или используйте вложенный `start.sh` (Pearl) / `start-qhash.sh` (QubitCoin), отредактировав кошелёк.

### HiveOS
Создайте полётный лист Custom miner:

| Поле | Pearl | QubitCoin (qhash) |
|---|---|---|
| Установочный URL | `https://github.com/0xHashRaptor/ForgeMiner/releases/download/v1.1.1/ForgeMiner-1.1.1.tar.gz` | тот же URL |
| Шаблон кошелька | `%WAL%.%WORKER_NAME%` (Pearl-кошелёк) | ваш QTC-адрес `.%WORKER_NAME%` |
| Адрес пула | `pearl.baikalmine.com:2010` · `ru.pearl.herominers.com:1200` · `prl-ru.kryptex.network:7048` | `ru.luckypool.io:8610` |
| Пароль | `x` | `x` |
| Доп. параметры | *пусто для Stratum* · `FORGE_PROTO=alpha` для AlphaPool · разгон, напр. `FORGE_CCLK=2505` | **`FORGE_ALGO=qhash`** · разгон, напр. `FORGE_CCLK=2505` |

Применить → в дашборде появятся хешрейт, температуры, кулеры и шары по картам. Один майнер на риг.

---

## Параметры

Параметры можно задавать как флаги командной строки (`--флаг значение`) или как переменные окружения (`FORGE_ФЛАГ`).

| Флаг | Переменная | Описание |
|------|------------|----------|
| `--algorithm` | `FORGE_ALGO` | Алгоритм: `pearl` или `qhash` (QubitCoin). |
| `--pool` | `FORGE_POOL` | Адрес пула в виде `host:port`. |
| `--wallet` | `FORGE_WALLET` | Кошелёк для выплат. |
| `--worker` | `FORGE_WORKER` | Имя воркера/рига на пуле. |
| `--password` | `FORGE_PASS` | Пароль пула (обычно `x`). |
| `--proto` | `FORGE_PROTO` | Диалект Pearl-пула: `stratum` (HeroMiners, LuckyPool, Kryptex) или `alpha` (AlphaPool). *(только Pearl.)* |
| `--gpu` | `FORGE_GPU` | Майнить только на этих индексах карт, напр. `--gpu 0,1,2,6` (по умолчанию — все). Индексы как в `nvidia-smi`. |
| — | `FORGE_LOWVRAM` | Режим экономии видеопамяти для карт на 8 ГБ (Pearl). `1` — вкл, `0` — выкл. По умолчанию авто. |

### Разгон

> **Совет:** ForgeMiner упирается в частоту ядра и почти не зависит от памяти — для максимального хешрейта ставьте высокое ядро; память может оставаться низкой. Разгон требует root (Linux/HiveOS) или Администратора (Windows).
>
> **Разный разгон на карту:** каждый флаг принимает одно значение (на все карты) или список через запятую, соответствующий `--gpu`:
> `--gpu 0,1,2,6 --coff 300,250,300,200 --plimit 280,280,300,260`

| Флаг | Переменная | Описание |
|------|------------|----------|
| `--cclk` | `FORGE_CCLK` | Лок частоты ядра (МГц). |
| `--coff` | `FORGE_COFF` | Оффсет частоты ядра (МГц, `+`/`-`). |
| `--mclk` | `FORGE_MCLK` | Лок частоты памяти (МГц). |
| `--moff` | `FORGE_MOFF` | Оффсет частоты памяти (МГц, `+`/`-`). |
| `--plimit` | `FORGE_PLIMIT` | Лимит мощности (Вт). |

---

## Поддерживаемые алгоритмы

| Алгоритм | Монета | Комиссия |
|----------|--------|:--------:|
| PearlHash | Pearl (PRL) | 3% |
| qhash | QubitCoin (QTC) | 2.5% |

*Скоро будут другие алгоритмы — следите за каналом.*

Поддерживаемые GPU: NVIDIA RTX 20 (Turing), RTX 30 (Ampere), RTX 40 (Ada), RTX 50 (Blackwell), и майнинговые карты NVIDIA CMP (например CMP 50HX). *(RTX 20-й серии и картам CMP нужен драйвер 545+.)*

---

## Ресурсы

- Релизы и новости: https://t.me/ForgeMiner
- Поддержка и чат: https://t.me/ForgeMinerChat

---

<p align="center"><sub>© 2026 ForgeMiner. Не аффилирован с NVIDIA. Майньте ответственно.</sub></p>
