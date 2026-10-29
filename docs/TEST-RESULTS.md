# Test results

Real-pool tests on our test rigs (one GPU per machine). A = accepted shares, R = rejected shares. "Steady mining" is
the median rate between job changes. Rates depend on the individual card, its cooling, clocks and power limit, so
compare rows of the same date and card; they are measurements, not a promise.

| Date | Card | OS | Package | Hashrate | A / R |
|---|---|---|---|---|---|
| 2026-10-28 | RTX 4090 | Linux | v0.1.47 release build | 69.28 N/s (10 min; v0.1.46 69.45 N/s in the same session) | 10 / 0 |
| 2026-10-28 | RTX 5090 | Linux | v0.1.47 release build | 93.57 N/s (10 min; v0.1.46 93.10 N/s in the same session) | 7 / 0 |
| 2026-10-28 | RTX 3080 10 GB | Windows (WSL2) | v0.1.47 windows-wsl2-r3 | 21–22 N/s (operator's own run; about 9 N/s while a browser with hardware acceleration or Docker Desktop held VRAM) | n/a |
| 2026-10-23 | RTX 5090 | Linux | v0.1.46 release build | 95.41 N/s (steady mining, 12 min; 91.70 including job changes) | 10 / 0 (+1 stale; 0 invalid) |
| 2026-10-23 | RTX 4090 | Linux | v0.1.46 release build | 66.23 N/s (median, 10 min) | 2 / 0 |
| 2026-10-23 | RTX 4090 | Linux | v0.1.45 release build | 65.42 N/s (steady mining, 16 min; 62.35 including job changes) | 13 / 0 (0 invalid) |
| 2026-10-23 | RTX 5090 | Linux | v0.1.45 release build | 94.18 N/s (steady mining; 92.24 including job changes) | 17 / 0 (0 invalid) |
| 2026-10-23 | RTX 4090 | Linux | v0.1.44 sealed-r4 | 65.58 N/s (median, 16 min) | 10 / 0 |
| 2026-10-23 | RTX 5090 | Linux | v0.1.44 sealed-r4 | 93.45 N/s (steady mining, 44 min; 90.39 including job changes) | 31 / 0 (+1 stale at stop) |
| 2026-10-23 | RTX 3080 10 GB | Windows (WSL2) | v0.1.44 windows-wsl2-r5 | ~22 N/s (operator's own run) | n/a |
| 2026-10-23 | RTX 3080 10 GB | Linux (Ubuntu 26.04) | v0.1.44 sealed-r4 | 21.04 N/s (last 10 min of 23 min mining) | 3 / 0 |

The v0.1.47 Linux package was also checked on the RTX 4090 for a clean install that reuses the model, a wrong
password (stops after 5 seconds, before any download), an upgrade from v0.1.46 in place and into a new folder (model
reused, settings kept), and a full disk (`--status` names the reason). The Windows zip passed 144 automated checks on a
real Windows runner (first install, second double-click, upgrade from v0.1.46, the console fallback, the failure paths).

RTX 3080 on Linux (v0.1.44): install 6/6 steps OK, no invalid windows, clean stop. Log excerpt:
[`test-results/rtx3080-linux-20261023.txt`](test-results/rtx3080-linux-20261023.txt).

## Screenshots

From our test rigs, with earlier versions: v0.1.41 on the RTX 4090 and an early build on the RTX 5090. These are live
readings; your numbers depend on the card.

**RTX 4090, v0.1.41:** 69–75 N/s per reading, mostly ~70 (10-minute average ~68), 9 shares accepted, 0 rejected, ~445 W.

![TheNewMiner v0.1.41 mining on an RTX 4090](screenshots/rtx4090.png)

**RTX 5090, early build:** 100.83 N/s (live average over completed windows), ~575 W.

![An early TheNewMiner build mining on an RTX 5090](screenshots/rtx5090.png)

A 4× RTX 5090 rig is shown in the [development record](DEVELOPMENT-RECORD.md).
