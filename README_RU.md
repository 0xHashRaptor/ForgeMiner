<p align="center">
  <img src="assets/forge_logo.png" width="130" alt="ForgeMiner logo">
</p>

<h1 align="center">ForgeMiner</h1>

<p align="center"><b>Быстрый нативный майнер для NVIDIA — Pearl (PRL), QubitCoin (QTC), KawPow (Ravencoin, Quai, Neurai), Cryptix (CYTX) и BTX (btx.dev)</b></p>

<p align="center">
  <a href="https://github.com/0xHashRaptor/ForgeMiner/releases"><img src="https://img.shields.io/badge/version-1.4.1-orange.svg"></a>
  <a href="#быстрый-старт"><img src="https://img.shields.io/badge/platform-Windows%20%7C%20Linux%20%7C%20HiveOS-blue.svg"></a>
  <a href="#поддерживаемые-алгоритмы"><img src="https://img.shields.io/badge/GPU-NVIDIA%20Pascal%20%7C%20RTX%2020%2F30%2F40%2F50%20%2B%20CMP-76b900.svg"></a>
  <a href="https://t.me/ForgeMiner"><img src="https://img.shields.io/badge/Telegram-Releases-26A5E4.svg?logo=telegram"></a>
  <a href="https://discord.gg/vxUTbb9B"><img src="https://img.shields.io/badge/Discord-Community-5865F2.svg?logo=discord&logoColor=white"></a>
</p>

---

## Обзор

ForgeMiner — высокопроизводительный, полностью нативный майнер для GPU NVIDIA. Он работает с видеокартой напрямую через CUDA Driver API — без Python, без WSL, без лишних рантаймов — поэтому запускается мгновенно и потребляет минимум ресурсов даже на слабых ригах. Майнит **Pearl (PRL)**, **QubitCoin (QTC)**, **KawPow** (Ravencoin RVN, Quai QUAI, Neurai XNA), **Cryptix (CYTX)** и **BTX (btx.dev)** из одного бинарника — монета выбирается одним флагом — и скоро добавятся другие монеты.

Каждый алгоритм поставляется с отдельной сборкой под каждую поддерживаемую архитектуру карты; нужная выбирается автоматически при запуске — так каждый GPU работает на пике.

> ForgeMiner — с закрытым исходным кодом. Релизы публикуются здесь и анонсируются в [Telegram-канале](https://t.me/ForgeMiner) и в [Discord](https://discord.gg/vxUTbb9B).

---

## Возможности

- **Несколько монет, один бинарник** — майнинг Pearl (PRL), QubitCoin (QTC), KawPow (Ravencoin / Quai / Neurai), Cryptix (CYTX) или BTX (btx.dev); выбор через `--algorithm`. Скоро больше монет.
- **Ядра под архитектуру** — отдельное ядро под каждое поколение GPU (Pascal / Turing / Ampere / Ada / Blackwell), выбирается автоматически при запуске.
- **Ниже потребление на 30/40-й серии (Pearl)** — ядра Pearl для Ampere/Ada потребляют значительно меньше энергии при том же хешрейте, автоматически.
- **Эффективен на плотных ригах** — держит GPU загруженными даже при множестве карт на слабом CPU, нескольких экземплярах майнера или медленных x1-райзерах.
- **Нативный и лёгкий** — прямой CUDA Driver API, почти нулевая нагрузка на CPU (blocking-sync); отлично работает на слабых хостах и многокарточных сборках.
- **По-настоящему самодостаточный** — один исполняемый файл со всем встроенным и зашифрованным; никакого CUDA-рантайма, никакого NVRTC, никаких отдельных файлов ядер или библиотек, которые надо хранить или которые могут утечь — даже KawPow поставляется одним `.exe`.
- **Встроенный разгон и управление вентиляторами** — фиксация частот, оффсеты ядра/памяти, лимит мощности и управление вентиляторами прямо из майнера; сторонний OC-софт не нужен.
- **Управление по каждому GPU** — выбор карт для майнинга (`--gpu`) и свой разгон для каждой карты на смешанных ригах.
- **Мультипул с фейловером** — стандартные Stratum-пулы для всех монет; автопереподключение и переключение пулов.
- **Чистый живой дашборд** — хешрейт по каждому GPU, accepted/stale/rejected шары, эффективность, температуры (включая VRAM на Windows), частоты, вентиляторы и потребление с одного взгляда.
- **Готов к HiveOS** — ставится прямо в слот кастомного майнера HiveOS.

История версий и последние изменения — на странице [Releases](https://github.com/0xHashRaptor/ForgeMiner/releases).

---

## Быстрый старт

### Windows
1. Скачайте и распакуйте Windows-релиз.
2. Откройте `.bat` для вашей монеты/пула/региона в текстовом редакторе и впишите кошелёк и имя воркера. Файлы названы `<алго>_<пул>_<регион>.bat` (`_SSL` — шифрованное подключение):
   - **Pearl:** `pearlhash_Baikal_Global.bat`, `pearlhash_HeroMiners_DE.bat`, `pearlhash_Kryptex_RU.bat`, `pearlhash_LuckyPool_EU.bat`, `pearlhash_AlphaPool_EU.bat`, …
   - **QubitCoin:** `qhash_LuckyPool_RU.bat`, `qhash_LuckyPool_CA.bat`, `qhash_k1pool_RU.bat`, `qhash_k1pool_EU.bat`
   - **KawPow (Ravencoin / Quai):** `kawpow_RVN_Kryptex_Global.bat`, `kawpow_RVN_HeroMiners_US.bat`, `kawpow_RVN_2Miners_EU.bat`, `kawpow_QUAI_HeroMiners_DE.bat`, `kawpow_QUAI_Kryptex_EU.bat`, …
   - **KawPow (Neurai XNA):** `kawpow_XNA_Kryptex_Global.bat`, `kawpow_XNA_Kryptex_RU.bat`, `kawpow_XNA_Kryptex_EU.bat`, `kawpow_XNA_Vipor_RU.bat`, `kawpow_XNA_Vipor_DE.bat`, `kawpow_XNA_2Miners_EU.bat`, `kawpow_XNA_2Miners_US.bat`
   - **Cryptix (CYTX):** `cryptix_Baikalmine_Global.bat`, `cryptix_CryptixNetwork_Global.bat`
   - **BTX (btx.dev):** `btx_lproute_EU.bat`, `btx_lproute_RU.bat`
3. Запустите `.bat` двойным кликом. (Запуск от имени администратора — если хотите, чтобы применялся встроенный разгон.)

### Linux
```bash
chmod +x forge
# Pearl
FORGE_POOL=ru.pearl.herominers.com:1200 FORGE_WALLET=ВАШ_PRL_КОШЕЛЁК FORGE_WORKER=rig01 FORGE_PROTO=stratum ./forge
# QubitCoin (qhash)
./forge --algorithm qhash --wallet ВАШ_QTC_КОШЕЛЁК --worker rig01 --pool ru.luckypool.io:8610
# KawPow — Ravencoin (RVN) или Quai (QUAI); монета определяется по адресу пула
./forge --algorithm kawpow --wallet ВАШ_RVN_КОШЕЛЁК --worker rig01 --pool us.ravencoin.herominers.com:1140
# KawPow — Neurai (XNA); монету задаём явно
./forge --algorithm kawpow --coin xna --wallet ВАШ_XNA_КОШЕЛЁК --worker rig01 --pool xna.2miners.com:6060
# Cryptix (CYTX)
./forge --algorithm cryptix --wallet ВАШ_CYTX_КОШЕЛЁК --worker rig01 --pool cytx.baikalmine.com:9010
# BTX (btx.dev)
./forge --algorithm btx --wallet ВАШ_BTX_КОШЕЛЁК --worker rig01 --pool btx-eu.lproute.com:8660
```
…или используйте готовые `start.sh` (Pearl) / `start-qhash.sh` (QubitCoin), вписав свой кошелёк.

### HiveOS
Добавьте флайт-лист Custom miner:

| Поле | Pearl | QubitCoin (qhash) |
|---|---|---|
| Installation URL | `https://github.com/0xHashRaptor/ForgeMiner/releases/download/v1.4.1/ForgeMiner-1.4.1.tar.gz` | тот же URL |
| Wallet template | `%WAL%.%WORKER_NAME%` (кошелёк Pearl) | ваш QTC-адрес `.%WORKER_NAME%` |
| Pool URL | `pearl.baikalmine.com:2010` · `ru.pearl.herominers.com:1200` · `prl-ru.kryptex.network:7048` | `ru.luckypool.io:8610` |
| Pass | `x` | `x` |
| Extra config | *пусто для Stratum* · `FORGE_PROTO=alpha` для AlphaPool · разгон, напр. `FORGE_CCLK=2505` | **`FORGE_ALGO=qhash`** · разгон, напр. `FORGE_CCLK=2505` |

Для **KawPow** укажите `FORGE_ALGO=kawpow` в *Extra config* и пул Ravencoin, Quai или Neurai — напр. `us.ravencoin.herominers.com:1140` (RVN), `de.quai.herominers.com:1185` (QUAI) или `xna.2miners.com:6060` (XNA). Монета определяется по адресу пула; при необходимости переопределите через `FORGE_COIN=rvn` / `FORGE_COIN=quai` / `FORGE_COIN=xna` (для Neurai/Vipor задайте явно).

Для **Cryptix (CYTX)** укажите `FORGE_ALGO=cryptix` в *Extra config*, в качестве кошелька — ваш адрес `cryptix:…`, и пул Cryptix — напр. `cytx.baikalmine.com:9010`.

Для **BTX** укажите `FORGE_ALGO=btx` в *Extra config*, в качестве кошелька — ваш адрес `btx1…`, и пул BTX — напр. `btx-eu.lproute.com:8660`.

Apply → дашборд покажет хешрейт по каждому GPU, температуры, вентиляторы и шары. Запускайте один майнер на риг.

---

## Параметры

Параметры задаются флагами командной строки (`--flag value`) или переменными окружения (`FORGE_FLAG`) — удобно для HiveOS и `.bat`-файлов.

| Флаг | Переменная | Описание |
|------|--------------|-------------|
| `--algorithm` | `FORGE_ALGO` | Алгоритм: `pearl`, `qhash` (QubitCoin), `kawpow` (Ravencoin / Quai / Neurai), `cryptix` (CYTX) или `btx` (btx.dev). |
| `--coin` | `FORGE_COIN` | Монета KawPow: `rvn`, `quai` или `xna` (если не задано — определяется по адресу пула; для Neurai / Vipor задайте явно). *(Только KawPow.)* |
| `--pool` | `FORGE_POOL` | Адрес пула в виде `host:port`. |
| `--wallet` | `FORGE_WALLET` | Адрес кошелька для выплат. |
| `--worker` | `FORGE_WORKER` | Имя воркера / рига на пуле. |
| `--password` | `FORGE_PASS` | Пароль пула (обычно `x`). |
| `--proto` | `FORGE_PROTO` | Диалект пула Pearl: `stratum` (HeroMiners, LuckyPool, Kryptex) или `alpha` (AlphaPool). *(Только Pearl.)* |
| `--gpu` | `FORGE_GPU` | Майнить только указанные индексы GPU, напр. `--gpu 0,1,2,6` (по умолчанию все). Индексы — как в `nvidia-smi`. |
| — | `FORGE_LOWVRAM` | Режим низкой видеопамяти для карт 8 ГБ (Pearl). `1` — принудительно вкл, `0` — выкл. По умолчанию автоопределение. |

### Разгон и вентиляторы

> **Совет:** ForgeMiner упирается в частоту ядра и нетребователен к памяти — для лучшего хешрейта поднимайте ядро, память можно держать низко. Разгон требует root (Linux/HiveOS) или администратора (Windows).
>
> **Разгон по каждому GPU:** каждый флаг принимает одно значение (на все карты) или список через запятую, соответствующий `--gpu`:
> `--gpu 0,1,2,6 --coff 300,250,300,200 --plimit 280,280,300,260`

| Флаг | Переменная | Описание |
|------|--------------|-------------|
| `--cclk` | `FORGE_CCLK` | Фиксация частоты ядра GPU (МГц). |
| `--coff` | `FORGE_COFF` | Оффсет частоты ядра (МГц, `+`/`-`). |
| `--mclk` | `FORGE_MCLK` | Фиксация частоты памяти (МГц). |
| `--moff` | `FORGE_MOFF` | Оффсет частоты памяти (МГц, `+`/`-`). |
| `--plimit` | `FORGE_PLIMIT` | Лимит мощности (Вт). |
| `--fan` | `FORGE_FAN` | Фиксированная скорость вентилятора (%). Для своей кривой «температура → скорость» — `--fan-curve t:p,t:p,...`. |

### Управление вентиляторами

Задайте обороты вентиляторов сами — фиксированную скорость или кривую по температуре. Без флага вентиляторов
ForgeMiner оставляет автоматику драйвера. Установка оборотов требует Администратора (Windows) или root (Linux/HiveOS).
При выходе автоматика драйвера восстанавливается.

| Флаг | Переменная | Описание |
|------|------------|----------|
| `--fan` | `FORGE_FAN` | Фиксированная скорость в % (одно значение — на все карты, или список через запятую по `--gpu`). |
| `--fan-curve` | `FORGE_FANCURVE` | Кривая температура → скорость: точки `темп:процент`, линейная интерполяция. |

**Фиксированная скорость**

```
--fan 70                 все карты на 70%
--fan 60,70,80,100       по картам (в порядке --gpu: GPU0=60%, GPU1=70%, ...)
```

**Кривая по температуре** — рекомендуемый способ. Задайте точки `темп°C:вентилятор%`; ForgeMiner читает живую
температуру ядра каждой карты и ставит вентилятор на соответствующий процент, линейно интерполируя между точками:

```
--fan-curve 45:30,60:55,70:75,80:100
```

читается так: ≤45 °C → 30% · 60 °C → 55% · 70 °C → 75% · ≥80 °C → 100% · между точками (напр. 65 °C) → ~65%.
Точек можно сколько угодно (сортируются автоматически). Одна кривая применяется ко всем выбранным картам.
Фиксированный `--fan` для карты переопределяет для неё кривую.

> **Примечание:** у карт GeForce аппаратный минимум вентилятора ~30% — запрос ниже просто упрётся в 30%.

**HiveOS / Linux** — впишите в *Доп. параметры* полётника, либо используйте env-форму:

```
--fan-curve 50:40,65:60,75:85,83:100
FORGE_FANCURVE=50:40,65:60,75:85,83:100      (эквивалент через env; FORGE_FAN=70 для фикс. скорости)
```

---

## API мониторинга

У ForgeMiner есть встроенный **read-only** API мониторинга и веб-дашборд. По умолчанию **выключен** и только отдаёт статистику — управлять или перенастраивать майнер через него нельзя.

**Как включить**

```
--api                      на 127.0.0.1:7777 (только эта машина)
--api-bind 0.0.0.0:7777    на всех интерфейсах (смотреть с телефона или другого ПК по локалке)
--api-bind 127.0.0.1:9000  любой адрес/порт на ваш выбор
```

На HiveOS / майнинг-ОС задайте в *Extra config*: `FORGE_API=127.0.0.1:7777` (или `0.0.0.0:7777`). Всё ниже отдаётся с одного порта.

**Веб-дашборд** — откройте `http://<ip>:7777` в браузере

Живая панель, обновляется сама: общий хешрейт, средние за 1ч / 24ч, потребление, эффективность, accept rate и шары; живой график хешрейта со шкалой слева; и таблица по картам — название, хешрейт, температура (и температура памяти), частота ядра, частота памяти, вентилятор, потребление, эффективность, accepted / stale / rejected, со строкой **Total** — плюс пул, воркер и кошелёк. Есть переключатель **тёмной / светлой темы** и **выбор частоты обновления** (1с … 30с), оба запоминаются браузером. Ничего ставить не надо — встроено в майнер.

**Эндпоинты**

| Эндпоинт | Формат | С чем использовать |
|---|---|---|
| `GET /` | HTML | Веб-дашборд (алиас `/dashboard`). |
| `GET /summary` (или `/stats`) | JSON | Grafana, кастомные дашборды, боты и скрипты. Всё: общий + по-GPU хешрейт, температуры, частоты ядра/памяти, вентиляторы, потребление, эффективность, accepted/stale/rejected, accept rate, шары/мин, сложность, аптайм, пул/воркер/кошелёк и короткая история хешрейта для графиков. |
| `GET /metrics` | Prometheus | Дашборды и алерты в Grafana (`forge_hashrate_hs`, `forge_gpu_temperature_celsius`, `forge_gpu_fan_percent`, `forge_gpu_power_watts`, `forge_shares_total`, …). |
| `miner_getstat1` (JSON-RPC, тот же порт) | Claymore | **Awesome Miner, mmpOS** и остальная экосистема — добавьте форж как custom / managed-майнер с API **Claymore / Ethminer** на порту **7777**. Появляется из коробки. |

HiveOS уже покрыт встроенной интеграцией статистики и в этом не нуждается.

> **Безопасность:** API только на чтение и отдаёт лишь публичные данные (хешрейт, температуры, пул, кошелёк). Для доступа только с этой машины оставьте `127.0.0.1`; `0.0.0.0` используйте только внутри своей сети — за роутером / файрволом / VPN.

---

## Поддерживаемые алгоритмы

| Алгоритм | Монета | Комиссия |
|-----------|------|:-------:|
| PearlHash | Pearl (PRL) | 2% |
| qhash | QubitCoin (QTC) | 1% |
| KawPow | Ravencoin (RVN), Quai (QUAI), Neurai (XNA) | 0.7% |
| Cryptix | Cryptix (CYTX) | 2% |
| BTX | BTX (btx.dev) | 1% |

*Скоро новые алгоритмы — следите за каналом.*

Поддерживаемые GPU: NVIDIA Pascal P104-100 8 ГБ (Pearl, KawPow и Cryptix), RTX 20 (Turing), RTX 30 (Ampere), RTX 40 (Ada), RTX 50 (Blackwell) и майнинговые карты NVIDIA CMP (напр. CMP 50HX). *(RTX 20-й серии и картам CMP нужен драйвер 545+.)*

---

## Ресурсы

- Релизы и новости: https://t.me/ForgeMiner
- Поддержка и чат: https://t.me/ForgeMinerChat
- Discord-сообщество: https://discord.gg/vxUTbb9B

---

<p align="center"><sub>© 2026 ForgeMiner. Не аффилирован с NVIDIA. Майньте ответственно.</sub></p>
