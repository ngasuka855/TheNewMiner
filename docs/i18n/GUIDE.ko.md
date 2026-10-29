[English](../GUIDE.md) · [简体中文](GUIDE.zh-CN.md) · [繁體中文](GUIDE.zh-TW.md) · [Русский](GUIDE.ru.md) · [Español](GUIDE.es.md) · [Português (Brasil)](GUIDE.pt-BR.md) · [Français](GUIDE.fr.md) · [Deutsch](GUIDE.de.md) · [Türkçe](GUIDE.tr.md) · [Tiếng Việt](GUIDE.vi.md) · **한국어** · [日本語](GUIDE.ja.md)

> 이 번역은 편의를 위해 제공되며, 영어 README가 공식 기준입니다. *(This translation is provided for convenience; the English README is authoritative.)*

# TheNewMiner 가이드

첫 실행 이후의 모든 내용: 다운로드 검증, Linux·Windows·HiveOS에서의 일상적인 사용, 업그레이드, 채굴되지 않을 때의 조치, 자주 묻는 질문. 다운로드와 빠른 시작은 [메인 페이지](README.ko.md)에 있으며, Windows 문제는 별도 페이지가 있습니다: [Windows 문제 해결](WINDOWS-TROUBLESHOOTING.ko.md).

## 다운로드 검증

| 파일 | SHA-256 |
|---|---|
| [`TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz) | `86c215dc9401209b8d2a2346252ed5cc1901527b6a51520372e3128583c63656` |
| [`TheNewMiner-v0.1.47-windows-wsl2-r3.zip`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/TheNewMiner-v0.1.47-windows-wsl2-r3.zip) | `45a459646713bef9f8285848486c0a4bd66dee65e8c63090c8d3345c998b2659` |
| [`thenewminer-0.1.47.tar.gz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/thenewminer-0.1.47.tar.gz) (HiveOS) | `6b2515c5bf23f60daa236ff4a8b165ce5e55265d7eecac9dd408896e1e31b784` |

Windows용 zip은 올인원입니다. Linux 채굴기가 이미 들어 있으므로 zip만 있으면 됩니다. 패키지와 [`SHA256SUMS`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/SHA256SUMS)를 같은 폴더에 넣은 다음, 아래를 실행하세요:

```bash
# Linux (--ignore-missing: 다운로드한 패키지만 검사합니다)
sha256sum -c --ignore-missing SHA256SUMS
# macOS
shasum -a 256 -c --ignore-missing SHA256SUMS
```

```bat
:: Windows (명령 프롬프트): 출력값을 위 표의 SHA-256과 비교하세요
certutil -hashfile TheNewMiner-v0.1.47-windows-wsl2-r3.zip SHA256
```

각 파일은 `OK`로 표시되어야 합니다 (Windows에서는 표와 동일한 64자). 일치하지 않으면 사용하지 마세요.

## Linux

```bash
tar -xJf TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz
cd thenewminer
./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w rig-01 --background
```

| 명령 | 동작 |
|---|---|
| `./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w rig-01 --background` | 첫 실행: 필요한 것을 설치한 뒤 백그라운드에서 채굴 |
| `./thenewminer --background` | 이후 실행: 설정이 기억되어 있음 |
| `./thenewminer` | 포그라운드에서 채굴 (Ctrl+C로 중지) |
| `./thenewminer --logs` | 채굴기 상태 보기 (Ctrl+C로 보기를 종료해도 채굴은 계속됨) |
| `./thenewminer --status` | 채굴 중인지 여부, 최근 해시레이트, 승인 / 거부된 셰어. 중지된 후에는 중지된 이유와 해결 방법 |
| `./thenewminer --stop` | 채굴 중지 |
| `./thenewminer --help` | 모든 옵션 |

- `-u`는 TensorCash 지갑 주소, `-p`는 풀 비밀번호, `-w`는 풀에 표시되는 워커 이름입니다 (기본값: 이 머신의 호스트 이름. 영문자, 숫자, `_`, `-`, `.`로 이루어진 1–64자).
- 지갑 주소는 무엇이든 시작하기 전에 확인됩니다. 오타가 있는 주소 (체크섬이 일치하지 않음)나 예시 주소는 무엇이 잘못되었는지와 함께 거부됩니다.
- 풀 비밀번호: 6–128자, 눈에 보이는 모든 문자 사용 가능, 공백 불가. 명령줄에서는 항상 작은따옴표로 감싸세요. 예: `-p 'my$pass!'` (작은따옴표 안의 `'`는 `'\''`로 씁니다).
- 설정은 `thenewminer/.miner/`에 기억됩니다 (비밀번호가 담긴 파일은 본인만 읽을 수 있습니다). 지갑과 비밀번호는 즉시 명령줄에서 제거되므로 `ps`에 보이지 않습니다 (단, 셸의 명령 기록에는 남아 있을 수 있습니다).
- GPU가 하나인 머신의 첫 실행에서는 채굴기가 모델을 다운로드하기 **전에** 지갑과 비밀번호를 풀에서 확인하므로, 오타를 몇 초 만에 찾아냅니다 (멀티 GPU 리그는 채굴이 시작될 때 찾아냅니다). 풀이 이를 거부하면 채굴기가 이를 알리고 중지합니다. 확인한 뒤 `-u`와 `-p`로 다시 시작하세요.
- 첫 실행 시 모델 (약 8.1 GiB, 한 번만)을 다운로드하고, 카드에 맞는 GPU 엔진을 설치하고, 카드에서 자체 점검을 실행한 뒤 채굴을 시작합니다. `[ RATE] ... N/s` 같은 줄이 보이면 채굴 중입니다. 로그는 `thenewminer/.miner/thenewminer.log`입니다.
- 기본 Ubuntu 22.04 / 24.04 (WSL에 포함된 것 포함)에는 채굴기에 필요한 모든 라이브러리가 이미 있으므로 아무것도 설치하지 않습니다. 빠진 것이 있으면 (Docker 이미지, 최소 구성 시스템) Ubuntu, Debian, HiveOS에서는 첫 실행 시 그 목록을 보여 주고 `apt`로 설치합니다 (sudo 또는 root 필요). 다른 시스템에서는 직접 설치할 수 있도록 그 이름을 알려 줍니다.
- 멀티 GPU 리그: 지원되며, 각 GPU가 채굴합니다. 이번 릴리스는 머신당 GPU 한 개로 테스트했습니다.
- 공유 메모리가 적은 Docker나 기타 컨테이너에서는 채굴기가 디스크의 `thenewminer/.miner/run/`에서 프로그램 파일을 실행하고 이를 알려 줍니다. 다른 폴더를 선택하려면 `TNPSEAL_RUNTIME_BASE=<folder> ./thenewminer`로 시작하세요 (본인만 쓸 수 있는 폴더: `chmod 700 <folder>`).
- **업그레이드:** `./thenewminer --stop`을 실행하고, 새 패키지를 같은 위치에 압축 해제한 뒤 (`thenewminer/`에 풀리며, 설정과 모델이 들어 있는 `thenewminer/.miner/`는 그대로 유지됩니다) 다시 시작하세요. 이전 버전이 아직 실행 중이면 새 버전은 시작을 거부하고 먼저 이전 버전을 중지하라고 안내합니다. 대신 새 폴더에 압축을 풀면 이전 버전의 모델 (같은 크기와 SHA-256)을 찾아 재사용하므로 8.1 GiB를 다시 다운로드하지 않습니다. 그곳에서 `-u -p -w`를 다시 지정하세요.

### 채굴되지 않을 때

`./thenewminer --status`가 채굴기가 중지된 이유와 해결 방법을 알려 줍니다. 가장 흔한 메시지:

| 표시되는 내용 | 해야 할 일 |
|---|---|
| `bash: ./thenewminer: Permission denied` | 이 폴더에서는 프로그램을 실행할 수 없습니다 (`noexec`로 마운트된 곳 (일부 `/tmp` 등), 또는 USB 메모리). 홈 폴더에 압축을 푸세요: `cd ~ && tar -xJf <the package>.tar.xz && cd thenewminer` |
| `this package is on a FAT (vfat) drive` (또는 exFAT / NTFS, 또는 `Windows (DrvFs, 9p)`: WSL의 `/mnt/d` 같은 Windows 드라이브) | 이런 드라이브에서는 채굴기의 비공개 설정 (모드 600)을 유지할 수 없습니다. 표시되는 `mkdir -p ~/thenewminer && cp -a ...` 줄을 붙여 넣으세요. 폴더를 Linux 홈 폴더로 복사하고 그곳에서 시작합니다 |
| `this system has no xz` | `sudo apt-get install -y xz-utils`를 실행한 다음 (root라면 `sudo` 없이) 다시 시작하세요 |
| `This system is missing what TheNewMiner needs` | 표시되는 `sudo apt-get install -y ...` 줄을 실행한 다음 다시 시작하세요 |
| `No NVIDIA GPU found on this machine` | NVIDIA 카드가 인식되지 않습니다. 카드가 제대로 장착되고 전원이 연결되어 있는지 확인하세요. 클라우드 머신이라면 NVIDIA GPU가 있는 것을 선택하세요 |
| `NVIDIA driver ... is too old` | 안내된 드라이버 (580 이상)를 설치하고 재부팅한 다음 다시 시작하세요 (아래 "NVIDIA 드라이버 업데이트" 참고) |
| `the pool rejected the wallet / password` | `-u`와 `-p`, 그리고 풀 웹사이트에 계정이 있는지 확인한 다음 `./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' --background`를 실행하세요 |
| `no download source answered` | 모델 다운로드가 인터넷에 연결할 수 없습니다. 프록시를 사용하는 환경이라면: `https_proxy=http://<HOST>:<PORT> ./thenewminer --background`. 또는 모델을 복사하세요 (아래 "이미 모델이 있나요?" 참고) |
| `cannot reach the pool` / `pool disconnected` | 인터넷 연결, DNS, 그리고 방화벽이 아웃바운드 TCP 443을 허용하는지 확인하세요. 네트워크가 풀을 차단한다면 `--proxy`를 사용하세요 (아래 "프록시" 참고). 연결은 자동으로 다시 이루어집니다 |

### 재부팅 후 시작 (선택 사항)

채굴기는 재부팅 후 자동으로 시작되지 않습니다 (HiveOS는 자동으로 해 줍니다). Linux에서는 `crontab`에 한 줄을 추가하거나:

```bash
crontab -e      # 그런 다음 이 줄을 추가합니다 (자신의 폴더로):
@reboot cd /home/<YOU>/thenewminer && ./thenewminer --background
```

systemd 사용자 서비스를 사용하세요 (로그인 없이 시작되도록 `loginctl enable-linger <YOU>`를 한 번 실행):

```ini
# ~/.config/systemd/user/thenewminer.service, 그런 다음: systemctl --user enable --now thenewminer
[Unit]
Description=TheNewMiner
After=network-online.target
[Service]
WorkingDirectory=%h/thenewminer
ExecStart=%h/thenewminer/thenewminer
ExecStop=%h/thenewminer/thenewminer --stop
Restart=on-failure
RestartSec=60
RestartPreventExitStatus=77
[Install]
WantedBy=default.target
```

종료 상태 77은 풀이 지갑 / 비밀번호를 거부했다는 뜻입니다. 다시 시작해도 해결되지 않으므로 서비스는 그 자리에서 멈춥니다.

## Windows

1. `TheNewMiner-v0.1.47-windows-wsl2-r3.zip` 전체를 압축 해제하세요 (마우스 오른쪽 버튼 클릭 › 모두 압축 풀기). 위치는 어느 폴더, 어느 드라이브든 상관없으며, 공백이나 중국어 문자가 포함된 경로도 괜찮습니다. zip 내용 보기 화면 안에서 바로 실행하지 마세요.
2. **`TheNewMiner.bat`**을 더블 클릭하세요.
   - **처음 실행 시:** 창 하나가 먼저 풀 계정을 묻습니다. 지갑 주소 (`tc1`로 시작하며, 체크섬을 확인합니다. 표시된 예시는 실제 지갑이 아닙니다), 풀 비밀번호 (6–128자, 눈에 보이는 모든 문자 사용 가능, 공백 불가), 워커 이름 (컴퓨터 이름이 미리 채워짐)입니다. 세 가지가 모두 올바를 때까지 확인 버튼은 회색으로 비활성화됩니다. 확인과 예를 누른 후에는 더 이상 아무것도 묻지 않습니다. Windows, 드라이버, 디스크를 확인하고 필요하면 WSL2를 켜고 (관리자 확인 창이 한 번 표시됩니다. 재부팅이 한 번 필요할 수 있으며, 그 후 설치가 자동으로 계속됩니다), 데이터가 저장될 위치를 보여 주고, 자체 Linux (WSL) 환경을 만들고, 동봉된 채굴기를 설치하고, 모델을 다운로드한 뒤 채굴을 시작합니다. 데스크톱이 없는 환경 (SSH 접속)에서는 같은 질문이 콘솔에 표시됩니다.
   - **그다음부터는 매번:** 더블 클릭하면 저장된 설정으로 바로 채굴을 시작합니다. 바탕 화면 바로 가기 "TheNewMiner"도 만들어집니다. 이미 채굴 중일 때 다시 더블 클릭하면 그 사실을 알려 주고 로그를 보여 줍니다.
   - **설치가 멈추면** 창에 어느 단계에서 멈췄는지와 무엇을 해야 하는지가 표시되며, `TheNewMiner.bat`을 실행하면 다시 보여 줍니다. [Windows 문제 해결](WINDOWS-TROUBLESHOOTING.ko.md)을 참고하세요.

일상적으로 쓰는 명령 (압축을 푼 폴더에서 명령 프롬프트를 여세요):

| 명령 | 동작 |
|---|---|
| `TheNewMiner.bat` | 필요하면 설치한 뒤 이 창에서 채굴 (창을 닫으면 채굴이 중지됨) |
| `TheNewMiner.bat background` | 창 없이 채굴 |
| `TheNewMiner.bat logs` | 채굴기 상태 보기 (Ctrl+C로 보기를 종료해도 채굴은 계속됨) |
| `TheNewMiner.bat status` | 실행 중인지 여부와 최근 해시레이트 |
| `TheNewMiner.bat stop` | 채굴 중지 |
| `TheNewMiner.bat setup` | 지갑 / 비밀번호 / 워커 이름 다시 입력 (취소하면 기존 값 유지) |
| `TheNewMiner.bat uninstall` | 채굴기, 데이터 폴더, 바로 가기 제거 (먼저 확인함) |

- **데이터 저장 위치:** 채굴기와 모델은 첫 설치 때 선택한 데이터 폴더에 저장됩니다 (기본값은 압축을 푼 드라이브의 루트에 있는 `TheNewMinerData`, 예: `D:\TheNewMinerData`). 압축을 푼 폴더에는 저장되지 않습니다. 그곳에 약 20 GB의 여유 공간이 필요합니다. 다른 폴더를 선택하려면: `TheNewMiner.bat setup -DataDir <folder>`.
- **Ubuntu:** 설치 과정에서 `wsl --install`로 Ubuntu 24.04를 설치합니다. 그 다운로드가 연결되지 않으면 중국의 공개 미러 (칭화대 TUNA, 그다음 USTC, 그다음 Aliyun)에서 Ubuntu 공식 WSL 이미지 `ubuntu-24.04.5-wsl-amd64.wsl`을 다운로드하고, SHA-256이 정확히 `bb415d824822c4b878125729af451a5d18fb13d1cf5cbed9a7393ad64ac6039e`일 때만 사용합니다.
- 풀이 지갑이나 비밀번호를 거부하면 창에 그 사실이 표시되고 채굴이 중지됩니다. `TheNewMiner.bat setup`을 실행해 다시 입력하세요.
- **업그레이드:** 새 버전을 아무 곳에나 압축 해제하고 그 안의 `TheNewMiner.bat`을 더블 클릭하세요. 설치된 채굴기를 찾아 설정과 모델을 유지한 채, 아무것도 묻지 않고 새 버전을 설치합니다. 이전 버전이 설치했거나 다른 WSL 배포판 (예: `Ubuntu-24.04`) 안에 수동으로 설치한 것도 찾아내 그 자리에서 업그레이드합니다. 두 번째 설치도, 새 Ubuntu도, 모델 재다운로드도 없습니다. 그 자리에서 업그레이드할 수 없으면 설치가 멈추고 이유를 알려 줍니다. 이전 폴더는 삭제해도 됩니다.
- **제거:** `TheNewMiner.bat uninstall`. 사용자가 직접 만든 WSL 배포판은 절대 제거하지 않습니다.
- Windows 키트는 영어로 표시됩니다.

## HiveOS (실험적)

HiveOS 지원은 **실험적** 기능입니다 (v0.1.45부터 제공, v0.1.45는 실제 HiveOS 리그에서 실행되었지만 v0.1.46 이후의 변경 사항은 아직 HiveOS 리그에서 실행되지 않음). HiveOS 커스텀 채굴기 (custom miner) 패키지 `thenewminer-0.1.47.tar.gz`를 사용합니다 (SHA-256은 "다운로드 검증"의 표에 있습니다).

빠른 설정 (플라이트 시트 가져오기):

1. **Wallets** (지갑): 코인 `TSC`와 TensorCash 주소 (`tc1...`)로 지갑을 추가합니다. 이 작업은 한 번만 하면 됩니다. HiveOS에서는 `TSC` 지갑이 선택되어 있어야 하며, 채굴기는 플라이트 시트에 입력한 주소를 사용합니다.
2. [`hiveos/flightsheet-thenewminer.json`](../../hiveos/flightsheet-thenewminer.json)을 열어 내용을 복사합니다. `<YOUR_WALLET>`을 지갑 주소로 바꾸고 (뒤의 `.%WORKER_NAME%`은 그대로 두세요) `<YOUR_POOL_PASSWORD>`를 풀 비밀번호로 바꿉니다 (JSON에서는 비밀번호 안의 `"`를 `\"`로, `\`를 `\\`로 씁니다). HiveOS에서 Flight Sheets › **Import from clipboard**를 선택해 붙여 넣고, `TSC` 지갑을 선택한 뒤 저장합니다.
3. 플라이트 시트를 리그에 적용합니다. 리그가 패키지를 다운로드하고 채굴을 시작합니다.

수동으로 설정하려면: Coin `TSC`, Wallet: `TSC` 지갑, Pool: "Configure in miner", Miner: "Custom"으로 플라이트 시트를 만들고, **Setup Miner Config**를 클릭한 뒤 다음을 입력합니다:

| 필드 | 값 |
|---|---|
| Miner name | `thenewminer` (Installation URL에서 자동으로 채워짐) |
| Installation URL | `https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/thenewminer-0.1.47.tar.gz` |
| Hash algorithm | 비워 둠 |
| Wallet and worker template | `%WAL%.%WORKER_NAME%` |
| Pool URL | `tsc-wss.the-new.win` (무시됨: 풀이 내장되어 있음) |
| Pass | 풀 비밀번호 |
| Extra config arguments | 비워 두거나 `--proxy <YOUR_PROXY>` (프록시 섹션 참고) |

- 풀에 표시되는 워커 이름은 HiveOS의 리그 이름입니다. 영문자, 숫자, `_`, `-`, `.`만 사용하세요 (공백 불가). 최대 64자입니다. 다른 워커 이름을 쓰려면 Extra config arguments에 `-w <RIG_NAME>`을 추가하세요.
- 모델과 채굴기를 위해 HiveOS 드라이브에 약 12 GB의 여유 공간이 필요합니다 (`df -h /hive/miners`로 확인).
- 모델 (약 8.1 GiB)은 `/hive/miners/custom/.thenewminer-models`에 한 번 다운로드되며, 업그레이드해도 유지됩니다.
- 지갑과 비밀번호는 명령줄이 아니라 환경 변수를 통해 채굴기에 전달됩니다.
- **문제가 생기면** (예를 들어 풀이 지갑이나 비밀번호를 거부하면) 채굴기 로그와 HiveOS 대시보드에 이유가 표시되고, 리그는 대기 시간을 점점 늘려 가며 (일 분에서 십 분까지) 다시 시도합니다. 플라이트 시트를 수정해 적용하세요. 적용하면 채굴기가 즉시 다시 시작됩니다.
- 로그: HiveOS의 채굴기 화면 (리그 셸에서 `miner`) 또는 `/var/log/miner/custom/custom.log`.
- **업그레이드:** Installation URL의 두 버전 번호를 모두 바꾸고 (예: `.../raw/vX.Y.Z/thenewminer-X.Y.Z.tar.gz`) 플라이트 시트를 적용하세요. HiveOS는 URL의 파일 이름이 바뀔 때만 설치합니다.
- **첫 시작** 시 모델을 다운로드하고 GPU 엔진을 설치하므로 몇 분이 걸립니다.
- **이미 모델이 있나요?** 첫 시작 전에 리그의 `/home/user/`에 복사하세요 (다음 섹션 참고).

**리그가 GitHub에 접속할 수 없다면:** PC에서 `thenewminer-0.1.47.tar.gz`를 다운로드하고 (SHA-256을 확인하세요), 리그로 복사한 뒤 HiveOS가 사용자 지정 채굴기를 두는 위치에 압축을 풉니다. 그런 다음 평소처럼 플라이트 시트를 적용하세요. HiveOS가 "Already installed"라고 표시하고 다운로드하지 않습니다. 파일 이름은 정확히 `thenewminer-0.1.47.tar.gz`로 유지하고 (브라우저가 " (1)"을 붙일 수 있습니다), 플라이트 시트의 Installation URL은 바꾸지 마세요. HiveOS가 이 이름을 비교합니다.

```bash
# PC에서
scp thenewminer-0.1.47.tar.gz user@<RIG_IP>:/tmp/
# 리그에서 root로 (Hive Shell, 또는 SSH 접속 후 sudo -i)
mkdir -p /hive/miners/custom/downloads
mv /tmp/thenewminer-0.1.47.tar.gz /hive/miners/custom/downloads/
cd /hive/miners/custom && tar -xzf downloads/thenewminer-0.1.47.tar.gz
```

## 이미 모델이 있나요? 복사하세요

채굴기에는 8.1 GiB 모델 파일 하나가 필요합니다: `Qwen3-8B-Q8_0-full-v2.gguf` (`8709518112` 바이트, SHA-256 `408b955510e196121c1c375201744783b5c9a43c7956d73fc78df54c66e883d6`). 이 컴퓨터나 네트워크의 다른 컴퓨터에 이미 있다면 다시 다운로드하지 말고 복사하세요.

채굴기는 무엇이든 다운로드하기 전에 정확한 이름과 크기로 파일을 찾습니다. 찾는 위치는 자체 모델 폴더, 이전 TheNewMiner 폴더, 홈 폴더 (와 그 아래 한두 단계의 폴더, 또는 그 안의 `models` 폴더), `/home` 아래의 홈 폴더들, `/data`, `/mnt/*`, `/media/*`, `/opt`, `/srv`, `/workspace`, `/var/lib`, 그리고 Hugging Face / ModelScope 캐시입니다. 그곳에서 찾지 못하면 같은 디스크에 있는 그 폴더들에서 크기가 정확히 같은 `.gguf` 파일을 최대 20초 동안 검색합니다 (마운트된 다른 디스크는 이름으로만 확인합니다). 찾은 파일은 **SHA-256이 일치할 때만** 사용되며, 채굴기는 그 파일을 자체 모델 폴더에 링크하거나 (같은 디스크인 경우) 복사하므로 이후의 모든 버전이 바로 찾을 수 있습니다. 파일을 직접 지정할 수도 있습니다: `./thenewminer --model /path/to/Qwen3-8B-Q8_0-full-v2.gguf`.

**Linux:** 홈 폴더에 복사한 다음 평소처럼 채굴기를 시작하세요. 같은 네트워크의 Mac이나 PC에서 복사하려면 (Windows 10과 11의 PowerShell에는 `scp`가 있습니다), `user`와 `<MINER_IP>`를 채굴기의 로그인 이름과 주소로 바꾸세요:

```bash
# 채굴기 컴퓨터에서 직접
cp /path/to/Qwen3-8B-Q8_0-full-v2.gguf ~/
# 또는 다른 컴퓨터에서
scp Qwen3-8B-Q8_0-full-v2.gguf user@<MINER_IP>:~/
```

**HiveOS:** 리그의 `/home/user/`에 복사한 다음 (HiveOS의 SSH 로그인은 `user`), 플라이트 시트를 적용하세요. 채굴기가 그곳에서 파일을 찾아 확인한 뒤 패키지 옆 (`/hive/miners/custom/.thenewminer-models`)에 보관하므로, 새 버전이 다시 다운로드하는 일은 없습니다. HiveOS의 디스크는 작습니다. 먼저 `disk-expand`를 실행하세요 (`df -h /hive/miners`에서 여유 공간이 12 GB 미만으로 표시되는 경우).

```bash
scp Qwen3-8B-Q8_0-full-v2.gguf user@<RIG_IP>:/home/user/
```

**Windows:** 첫 `setup`이 TheNewMiner의 WSL 배포판을 만든 후, 명령 프롬프트에서 파일을 그 안으로 복사하세요 (`D:\Models`는 복사본이 있는 위치로 바꾸세요). 다운로드가 이미 시작되었다면 창을 닫고 이 명령을 실행한 다음 TheNewMiner를 다시 시작하세요. 복사본을 찾아 다운로드를 건너뜁니다. 복사에는 몇 분이 걸리며 진행 상황은 표시되지 않습니다.

```bat
wsl -d TheNewMiner -u root -- mkdir -p ~/thenewminer/models
wsl -d TheNewMiner -u root -- cp /mnt/d/Models/Qwen3-8B-Q8_0-full-v2.gguf ~/thenewminer/models/
```

## 지갑, 풀 비밀번호 또는 워커 이름 변경

풀은 각 지급 주소를 그 주소 전용 풀 계정에 영구적으로 묶습니다. 풀 페이지 [tsc.the-new.win](https://tsc.the-new.win)은 TSC 지급 주소를 "永久绑定 · 不可修改" (영구 바인딩, 수정 불가)라고 표시하며, 바꾸려면 채굴기에서 새 주소를 직접 사용하라고 안내합니다. 따라서 다른 지갑에는 그 지갑 전용 풀 계정과 풀 비밀번호가 필요합니다 (먼저 풀 페이지에서 만들어 두세요). 풀 페이지에서 풀 비밀번호를 바꾸면 모든 채굴기에서 비밀번호를 업데이트하세요. 풀은 잊어버린 비밀번호를 복구할 수 없습니다.

**Linux:** 채굴기를 중지한 다음, 새 설정으로 한 번 시작하세요. 새 설정이 기억된 설정을 대체합니다:

```bash
./thenewminer --stop
./thenewminer -u <NEW_WALLET> -p '<PASSWORD>' -w <RIG_NAME> --background
```

`-u`와 `-p`는 반드시 함께 지정하세요. 둘 중 하나만 지정하면 기억된 지갑과 비밀번호가 바뀌지 않습니다. 워커 이름은 따로 바꿀 수 있습니다: `./thenewminer -w <RIG_NAME> --background` (`--stop` 후에).

**Windows:** `TheNewMiner.bat setup`을 실행하세요. 워커 이름이 채워진 상태로 계정 입력 창이 열립니다. 지갑과 풀 비밀번호를 입력한 다음 평소처럼 시작하세요 (`TheNewMiner.bat`을 더블 클릭).

**HiveOS:** 플라이트 시트를 편집하세요. "Wallet and worker template"에 새 지갑 주소를 (`<NEW_WALLET>.%WORKER_NAME%`), Pass에 새 풀 비밀번호를 넣은 다음 저장하고 적용하세요. 워커 이름은 HiveOS의 리그 이름입니다 (또는 Extra config arguments의 `-w <RIG_NAME>`).

## 프록시 (선택 사항)

채굴기는 풀에 직접 연결합니다. 네트워크에 프록시가 필요하다면:

```bash
# Linux (변경할 때까지 기억됨)
./thenewminer --proxy <IP>[:<PORT>]            # TCP 릴레이 (생략 시 포트 443)
./thenewminer --proxy socks5://<HOST>:<PORT>   # SOCKS5, socks5://<USER>:<PASS>@<HOST>:<PORT> 형식도 가능
./thenewminer --proxy http://<HOST>:<PORT>     # HTTP CONNECT 프록시
./thenewminer --proxy none                     # 직접 연결로 되돌리기
```

이 명령들은 채굴기도 함께 시작합니다. 먼저 채굴기를 중지하고, 백그라운드에서 계속 실행하려면 `--background`를 추가하세요.

```bat
:: Windows
TheNewMiner.bat setup -Proxy <IP>[:<PORT>]
TheNewMiner.bat setup -Proxy socks5://<HOST>:<PORT>
TheNewMiner.bat setup -Proxy http://<HOST>:<PORT>
TheNewMiner.bat setup -Proxy none
```

TLS는 항상 풀과의 사이에서 종단 간 (end to end)으로 동작하므로 프록시는 연결 내용을 읽을 수 없습니다. Windows에서는 같은 PC에서 실행 중인 프록시 앱에 WSL에서 `127.0.0.1`로 접근할 수 없는 경우가 일반적입니다 (WSL 기본 네트워킹). PC의 LAN IP를 사용하고 앱에서 LAN 연결을 허용하세요.

## 요구 사항

| 항목 | 요구 사항 |
|---|---|
| GPU | NVIDIA GeForce RTX 30 / 40 / 50 시리즈, VRAM 10 GB 이상. 멀티 GPU 리그: 지원되며 각 GPU가 채굴함. 이번 릴리스는 머신당 GPU 한 개로 테스트함 |
| 드라이버 | NVIDIA **580 이상** (`nvidia-smi`가 작동해야 함). Windows에서는 Windows에 설치하고, 절대 WSL 안에 설치하지 마세요 |
| Linux | **glibc 2.34 이상**의 x86_64. 예: Ubuntu 22.04 이상 또는 Debian 12 이상. 설치 프로그램이 필요한 것을 확인하고 누락된 항목의 이름을 알려 줌 |
| Windows | Windows 10 21H2 이상 또는 Windows 11, 64비트. BIOS에서 가상화 (Intel VT-x / AMD SVM) 켜기 |
| 디스크 | 약 20 GB 여유 공간 |
| RAM | 4 GB 이상 |
| 모델 | 약 8.1 GiB, 첫 실행 시 한 번 다운로드 (병렬, 다중 미러)하며 이후 재사용 |

## NVIDIA 드라이버 업데이트 (580 이상)

채굴기에는 NVIDIA 드라이버 **580 이상**이 필요합니다. 현재 버전을 확인하세요:

```bash
nvidia-smi    # 헤더에 "Driver Version: ..."이 표시되며, 580 이상이어야 합니다
```

- **Ubuntu:** `sudo ubuntu-drivers install`로 권장 드라이버를 설치합니다 (설치 후 580 이상인지 확인하세요). 버전을 직접 선택하려면 `sudo apt install nvidia-driver-580` (또는 그 이상. RTX 50 시리즈 카드에는 open 버전이 필요합니다. 예: `nvidia-driver-580-open`)을 실행합니다. 그런 다음 재부팅하세요. Ubuntu 드라이버 패키지와 NVIDIA `.run` 설치 프로그램 중 하나만 사용하고, 절대 둘을 함께 사용하지 마세요.
- **Debian:** Debian 자체 드라이버 패키지는 580보다 오래된 버전입니다 (Debian 12는 535, Debian 13은 550). [NVIDIA Driver Installation Guide](https://docs.nvidia.com/datacenter/tesla/driver-installation-guide/debian.html) (NVIDIA 드라이버 설치 가이드)의 설명에 따라 NVIDIA 저장소에서 더 새로운 드라이버를 설치한 다음 재부팅하세요.
- **Windows:** [NVIDIA 앱](https://www.nvidia.com/en-us/software/nvidia-app/) (이전 GeForce Experience)으로 업데이트하거나, [nvidia.com](https://www.nvidia.com/en-us/drivers/)에서 카드에 맞는 드라이버 (580 이상)를 다운로드하세요. WSL2는 이 Windows 드라이버를 사용합니다. WSL 안에 Linux용 NVIDIA 드라이버를 절대 설치하지 마세요.
- **HiveOS:** root 권한으로 (Hive Shell, 또는 SSH 접속 후 `sudo -i`) `nvidia-driver-update --list`를 실행해 버전 목록을 확인한 다음, 예를 들어 `nvidia-driver-update 595` (580 이상이면 어느 버전이든 가능)를 실행하고, 마지막으로 `sreboot`를 실행하세요.

## 자주 묻는 질문

**Windows에 "Windows의 PC 보호" / "알 수 없는 게시자"라고 표시됩니다.**
이 키트는 코드 서명되어 있지 않습니다. **추가 정보**를 클릭한 다음 **실행**을 클릭하세요.

**백신 프로그램이 채굴기를 탐지하거나 삭제합니다.**
Microsoft Defender 및 기타 백신 프로그램은 악성 코드가 채굴기를 숨기는 경우가 많기 때문에, 사용자가 직접 실행하기로 선택한 정상적인 채굴기라도 암호화폐 채굴기를 "잠재적으로 원치 않는 애플리케이션"으로 탐지하는 경우가 많습니다. 파일이 격리되었다면 Windows 보안 › 바이러스 및 위협 방지 › 보호 기록을 열고, TheNewMiner 항목을 선택한 뒤 **허용**을 선택하세요 (또는 압축을 푼 폴더를 제외에 추가하세요). 이 페이지에서 다운로드했고 SHA-256이 일치하는 패키지에 대해서만 이렇게 하세요.

**첫 실행이 오래 걸립니다.**
첫 실행 시 약 8.1 GiB의 모델을 다운로드하고 카드에서 GPU 자체 점검을 실행합니다. Windows에서는 WSL2 설정도 진행할 수 있습니다. 이 과정은 한 번만 일어납니다. 이후 실행에서는 훨씬 빨리 채굴이 시작됩니다.

**채굴기가 풀이 지갑 / 비밀번호를 거부했다고 표시합니다.**
지갑 주소와 풀 비밀번호를 확인하고 (계정이 풀 웹사이트에 있어야 합니다), 다시 시작하세요: Linux `./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>'`, Windows `TheNewMiner.bat setup`. 채굴기는 잘못된 인증 정보로 스스로 재시도하지 않습니다.

**채굴 중인가요? 로그는 어디에 있나요?**
Linux: `./thenewminer --status`, `./thenewminer --logs` 또는 파일 `thenewminer/.miner/thenewminer.log`.
Windows: `TheNewMiner.bat status` 및 `TheNewMiner.bat logs`.

**제 RTX 3080 (10 GB)이 Windows에서 Linux보다 훨씬 느리게 채굴합니다.**
Windows에서는 다른 프로그램이 VRAM을 함께 사용합니다. [Windows 문제 해결](WINDOWS-TROUBLESHOOTING.ko.md)을 참고하여 하드웨어 가속을 사용하는 브라우저와 Docker Desktop을 닫으세요.
