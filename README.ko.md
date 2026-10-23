[English](README.md) · [简体中文](README.zh-CN.md) · [繁體中文](README.zh-TW.md) · [Русский](README.ru.md) · [Español](README.es.md) · [Português (Brasil)](README.pt-BR.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Türkçe](README.tr.md) · [Tiếng Việt](README.vi.md) · **한국어** · [日本語](README.ja.md)

> 이 번역은 편의를 위해 제공되며, 영어 README가 공식 기준입니다. *(This translation is provided for convenience; the English README is authoritative.)*

# TheNewMiner

TheNewMiner는 **TensorCash**용 GPU 채굴기입니다. **VRAM 10 GB 이상**의 NVIDIA GeForce RTX 30, 40, 50 시리즈 카드에서 동작하며
풀 **`tsc-wss.the-new.win`**에서 채굴합니다. 이 풀은 내장되어 있으므로 풀 주소를 입력할 필요가 없고,
직접 연결됩니다.

현재 버전: **v0.1.45**

## v0.1.45의 새로운 기능

- **풀이 지갑이나 비밀번호를 거부하면 명확한 메시지를 표시합니다.** 채굴기가 한 줄로 알려 주고 재시도하지 않고
  중지합니다. `--status`에서 그 이유를 볼 수 있습니다. 지갑과 비밀번호를 확인한 뒤 `-u`와 `-p`로 다시 시작하세요.
- **지갑과 비밀번호가 더 이상 `ps`에 보이지 않으며**, 다른 사용자의 프로세스 목록에도 보이지 않습니다.
- **`./thenewminer --status`**는 채굴 중인지 여부, 언제부터 채굴 중인지, 최근 해시레이트, 승인 / 거부된 셰어를 보여 줍니다.
- **HiveOS 지원 (실험적):** HiveOS 커스텀 채굴기 (custom miner) 패키지와 가져오기 가능한 플라이트 시트 (flight sheet) (아래 HiveOS 섹션 참고).
- **안전한 업그레이드:** 이전 버전이 아직 채굴 중이면 새 버전은 시작을 거부합니다 (먼저 중지하라고 안내합니다).
  `--stop` 후에는 프로그램 파일을 스스로 다시 설치하며, 설정과 모델은 그대로 유지합니다.
- **`--background`는 채굴기가 바로 종료되면 더 이상 "시작됨"으로 표시하지 않습니다.** 대신 이유를 보여 줍니다.
- **Docker 및 작은 공유 메모리:** 메모리 내 폴더가 너무 작거나 (Docker 기본값은 64 MiB) 프로그램 실행을 허용하지
  않으면, 채굴기는 대신 디스크의 `thenewminer/.miner/run/`을 사용하고 이를 알려 줍니다.
- **이제 풀 비밀번호에 눈에 보이는 모든 문자를 사용할 수 있습니다** (문장 부호와 영어 이외의 문자 포함): 6–128자,
  공백 불가.
- 더 많은 Linux 시스템 지원: 설치 프로그램이 몇몇 Ubuntu 버전만 허용하는 대신, 필요한 것 (glibc 2.34 이상)이
  있는지 확인합니다.

## 다운로드

| 파일 | 대상 | SHA-256 |
|---|---|---|
| [`TheNewMiner-v0.1.45-linux-x86_64-sealed.tar.xz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.45/TheNewMiner-v0.1.45-linux-x86_64-sealed.tar.xz) | Linux x86_64 | `6281bf489fae843551114f09ead5498d3e47af33a07e4cfdfbdc0a5d6bdbafbb` |
| [`TheNewMiner-v0.1.45-windows-wsl2-r1.zip`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.45/TheNewMiner-v0.1.45-windows-wsl2-r1.zip) | Windows 10 / 11 (WSL2를 통해 실행) | `90732441b0dc67de84809d9bf3bda4441c0bf7cd9a52fefb95d29242ad7b15e7` |
| [`SHA256SUMS`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.45/SHA256SUMS) | 모든 패키지의 체크섬 | |

Windows용 zip은 올인원입니다. Linux 채굴기가 이미 들어 있으므로 zip만 있으면 됩니다.

## 다운로드 검증

패키지와 `SHA256SUMS`를 같은 폴더에 넣은 다음, 아래를 실행하세요:

```bash
# Linux (--ignore-missing: 다운로드한 패키지만 검사합니다)
sha256sum -c --ignore-missing SHA256SUMS
# macOS
shasum -a 256 -c --ignore-missing SHA256SUMS
```

```bat
:: Windows (명령 프롬프트): 출력값을 위 표의 SHA-256과 비교하세요
certutil -hashfile TheNewMiner-v0.1.45-windows-wsl2-r1.zip SHA256
```

각 파일은 `OK`로 표시되어야 합니다 (Windows에서는 표와 동일한 64자). 일치하지 않으면 사용하지 마세요.

## 원클릭 설치 (Linux)

이 명령 끝에 있는 `<YOUR_WALLET>`, `<YOUR_POOL_PASSWORD>`, `<RIG_NAME>`을 (`<` `>` 포함) 바꾼 다음, 터미널에
붙여 넣으세요. 이 버전의 Linux 패키지와 `SHA256SUMS`를 다운로드하고 (`curl`을 사용하며, `wget`은 `curl`이 없을
때 사용), SHA-256을 확인한 뒤, 채굴기를 `~/thenewminer`에 압축 해제하고 백그라운드에서 시작합니다:

```bash
mkdir -p ~/thenewminer-download && cd ~/thenewminer-download && V=v0.1.45 && P=TheNewMiner-$V-linux-x86_64-sealed.tar.xz && U=https://github.com/ngasuka855/TheNewMiner/raw/$V && rm -f "$P" SHA256SUMS && if command -v curl >/dev/null 2>&1; then curl -fL -o "$P" "$U/$P" && curl -fL -o SHA256SUMS "$U/SHA256SUMS"; else wget -O "$P" "$U/$P" && wget -O SHA256SUMS "$U/SHA256SUMS"; fi && sha256sum -c --ignore-missing SHA256SUMS && tar -xJf "$P" -C ~ && cd ~/thenewminer && ./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w <RIG_NAME> --background
```

실패하는 첫 단계에서 멈춥니다. 다운로드 오류나 SHA-256 불일치가 있으면 아무것도 시작되지 않습니다 (이 검사는
손상된 다운로드를 막기 위한 것이며, 두 파일 모두 이 페이지에서 받습니다). 비밀번호를 감싼 작은따옴표는 그대로
두세요. 작은따옴표 안에서는 모든 문자가 안전합니다 (비밀번호 안의 `'`는 `'\''`로 씁니다). `tar`가
`xz: Cannot exec`라고 표시하면 xz를 설치하고 (`sudo apt-get install -y xz-utils`) 명령을 다시 실행하세요.

이미 `~/thenewminer`에 있는 버전을 업그레이드하려면 먼저 `~/thenewminer/thenewminer --stop`을 실행한 다음 이
명령을 실행하세요 (설정과 모델은 유지됩니다). 이전 버전이 다른 폴더에 있다면 먼저 그곳에서 중지하거나, Linux
섹션에 설명된 대로 그곳에서 업그레이드하세요 (그렇지 않으면 모델을 다시 다운로드합니다).

## Linux

```bash
tar -xJf TheNewMiner-v0.1.45-linux-x86_64-sealed.tar.xz
cd thenewminer
./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w rig-01 --background
```

| 명령 | 동작 |
|---|---|
| `./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w rig-01 --background` | 첫 실행: 필요한 것을 설치한 뒤 백그라운드에서 채굴 |
| `./thenewminer --background` | 이후 실행: 설정이 기억되어 있음 |
| `./thenewminer` | 포그라운드에서 채굴 (Ctrl+C로 중지) |
| `./thenewminer --logs` | 채굴기 상태 보기 (Ctrl+C로 보기를 종료해도 채굴은 계속됨) |
| `./thenewminer --status` | 채굴 중인지 여부, 최근 해시레이트, 승인 / 거부된 셰어 |
| `./thenewminer --stop` | 채굴 중지 |
| `./thenewminer --help` | 모든 옵션 |

- `-u`는 TensorCash 지갑 주소, `-p`는 풀 비밀번호, `-w`는 풀에 표시되는 워커 이름입니다
  (기본값: 이 머신의 호스트 이름. 영문자, 숫자, `_`, `-`, `.`로 이루어진 1–64자).
- 풀 비밀번호: 6–128자, 눈에 보이는 모든 문자 사용 가능, 공백 불가. 명령줄에서는 항상 작은따옴표로 감싸세요.
  예: `-p 'my$pass!'` (작은따옴표 안의 `'`는 `'\''`로 씁니다).
- 설정은 `thenewminer/.miner/`에 기억됩니다 (비밀번호가 담긴 파일은 본인만 읽을 수 있습니다). 지갑과 비밀번호는
  즉시 명령줄에서 제거되므로 `ps`에 보이지 않습니다 (단, 셸의 명령 기록에는 남아 있을 수 있습니다).
- 풀이 지갑이나 비밀번호를 거부하면 (또는 아직 계정을 승인하지 않았다면) 채굴기가 이를 알리고 중지합니다.
  확인한 뒤 `-u`와 `-p`로 다시 시작하세요.
- 첫 실행 시 모델 (약 8.1 GiB, 한 번만)을 다운로드하고, 카드에 맞는 GPU 엔진을 설치하고, 카드에서
  자체 점검을 실행한 뒤 채굴을 시작합니다. `[ RATE] ... N/s` 같은 줄이 보이면 채굴 중입니다.
- Ubuntu와 Debian에서는 첫 실행 시 누락된 시스템 라이브러리 몇 가지를 `apt`로 설치할 수 있습니다 (sudo 또는 root
  필요). 다른 시스템에서는 직접 설치할 수 있도록 그 이름을 알려 줍니다.
- 멀티 GPU 리그: 지원되며, 각 GPU가 채굴합니다. 이번 릴리스는 머신당 GPU 한 개로 테스트했습니다.
- `tar`가 `xz: Cannot exec`라고 표시하면 먼저 xz를 설치하세요: `sudo apt-get install -y xz-utils`.
- 공유 메모리가 적은 Docker나 기타 컨테이너에서는 채굴기가 디스크의 `thenewminer/.miner/run/`에서 프로그램
  파일을 실행하고 이를 알려 줍니다. 다른 폴더를 선택하려면
  `TNPSEAL_RUNTIME_BASE=<folder> ./thenewminer`로 시작하세요 (본인만 쓸 수 있는 폴더: `chmod 700 <folder>`).
- **업그레이드:** `./thenewminer --stop`을 실행하고, 새 패키지를 같은 위치에 압축 해제한 뒤 (`thenewminer/`에 풀리며,
  설정과 모델이 들어 있는 `thenewminer/.miner/`는 그대로 유지됩니다) 다시 시작하세요. 이전 버전이 아직 실행 중이면
  새 버전은 시작을 거부하고 먼저 이전 버전을 중지하라고 안내합니다.

## Windows (원클릭)

1. `TheNewMiner-v0.1.45-windows-wsl2-r1.zip` 전체를 압축 해제하세요 (마우스 오른쪽 버튼 클릭 › 모두 압축 풀기). 위치는 어느 폴더, 어느
   드라이브든 상관없으며, 공백이나 중국어 문자가 포함된 경로도 괜찮습니다. zip 내용 보기 화면 안에서 바로 실행하지 마세요.
2. **`TheNewMiner.bat`**을 더블 클릭하세요.
   - **처음 실행 시:** Windows, 드라이버, 디스크를 확인하고 필요하면 WSL2를 켭니다 (관리자 확인 창이 한 번 표시됩니다.
     재부팅이 한 번 필요할 수 있으며, 그 후 설치가 자동으로 계속됩니다). 데이터가 저장될 위치를 보여 주고 (Enter로
     확정), 자체 Linux (WSL) 환경을 만들고, 동봉된 채굴기를 설치한 다음, 지갑
     주소 (`tc1`로 시작), 풀 비밀번호 (두 번 입력; 6–128자, 눈에 보이는 모든 문자 사용 가능, 공백 불가), 워커 이름
     (Enter = 컴퓨터 이름)을 **한 번만** 묻고, 모델을 다운로드한 뒤 채굴을 시작합니다.
   - **그다음부터는 매번:** 더블 클릭하면 저장된 설정으로 바로 채굴을 시작합니다. 바탕 화면 바로 가기
     "TheNewMiner"도 만들어집니다. 이미 채굴 중일 때 다시 더블 클릭하면 그 사실을 알려 주고 로그를 보여 줍니다.

일상적으로 쓰는 명령 (압축을 푼 폴더에서 명령 프롬프트를 여세요):

| 명령 | 동작 |
|---|---|
| `TheNewMiner.bat` | 필요하면 설치한 뒤 이 창에서 채굴 (창을 닫으면 채굴이 중지됨) |
| `TheNewMiner.bat background` | 창 없이 채굴 |
| `TheNewMiner.bat logs` | 채굴기 상태 보기 (Ctrl+C로 보기를 종료해도 채굴은 계속됨) |
| `TheNewMiner.bat status` | 실행 중인지 여부와 최근 해시레이트 |
| `TheNewMiner.bat stop` | 채굴 중지 |
| `TheNewMiner.bat setup` | 지갑 / 비밀번호 / 워커 이름 다시 입력 |
| `TheNewMiner.bat uninstall` | 채굴기, 데이터 폴더, 바로 가기 제거 (먼저 확인함) |

- **데이터 저장 위치:** 채굴기와 모델은 첫 설치 때 선택한 데이터 폴더에 저장됩니다 (기본값은 압축을 푼
  드라이브의 루트에 있는 `TheNewMinerData`, 예: `D:\TheNewMinerData`). 압축을 푼 폴더에는 저장되지 않습니다. 그곳에 약 20 GB의
  여유 공간이 필요합니다.
- 풀이 지갑이나 비밀번호를 거부하면 창에 그 사실이 표시되고 채굴이 중지됩니다. `TheNewMiner.bat setup`을 실행해
  다시 입력하세요.
- **업그레이드:** 새 버전을 아무 곳에나 압축 해제하고 그 안의 `TheNewMiner.bat`을 더블 클릭하세요. 설치된 채굴기를 찾아
  설정과 모델을 유지한 채, 아무것도 묻지 않고 새 버전을 설치합니다. 이전 폴더는 삭제해도 됩니다.
- **제거:** `TheNewMiner.bat uninstall`.
- 화면 언어는 Windows 언어를 따릅니다 (English / 简体中文 / 繁體中文). 직접 고르려면 아무 명령에나 `-Lang en` (또는 `zh-Hans`,
  `zh-Hant`)을 추가하세요. 예: `TheNewMiner.bat -Lang en`. 해당 실행에만 적용됩니다.

## HiveOS (실험적)

HiveOS 지원은 v0.1.45에 새로 추가되었으며 **실험적** 기능입니다. 아래의 HiveOS 커스텀 채굴기 (custom miner)
패키지를 사용합니다.

| 파일 | SHA-256 |
|---|---|
| [`thenewminer-0.1.45.tar.gz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.45/thenewminer-0.1.45.tar.gz) | `90576efd388ceeca4e456b62b568f267465320913a07c9f9736f42363b1701dc` |

빠른 설정 (플라이트 시트 가져오기):

1. **Wallets** (지갑): 코인 `TSC`와 TensorCash 주소 (`tc1...`)로 지갑을 추가합니다. 이 작업은 한 번만 하면 됩니다.
   HiveOS에서는 `TSC` 지갑이 선택되어 있어야 하며, 채굴기는 플라이트 시트에 입력한 주소를 사용합니다.
2. [`hiveos/flightsheet-thenewminer.json`](hiveos/flightsheet-thenewminer.json)을 열어 내용을 복사합니다.
   `<YOUR_WALLET>`을 지갑 주소로 바꾸고 (뒤의 `.%WORKER_NAME%`은 그대로 두세요) `<YOUR_POOL_PASSWORD>`를 풀 비밀번호로
   바꿉니다 (JSON에서는 비밀번호 안의 `"`를 `\"`로, `\`를 `\\`로 씁니다). HiveOS에서 Flight Sheets › **Import from
   clipboard**를 선택해 붙여 넣고, `TSC` 지갑을 선택한 뒤 저장합니다.
3. 플라이트 시트를 리그에 적용합니다. 리그가 패키지를 다운로드하고 채굴을 시작합니다.

수동으로 설정하려면: Coin `TSC`, Wallet: `TSC` 지갑, Pool: "Configure in miner", Miner: "Custom"으로
플라이트 시트를 만들고, **Setup Miner Config**를 클릭한 뒤 다음을 입력합니다:

| 필드 | 값 |
|---|---|
| Miner name | `thenewminer` (Installation URL에서 자동으로 채워짐) |
| Installation URL | `https://github.com/ngasuka855/TheNewMiner/raw/v0.1.45/thenewminer-0.1.45.tar.gz` |
| Hash algorithm | 비워 둠 |
| Wallet and worker template | `%WAL%.%WORKER_NAME%` |
| Pool URL | `tsc-wss.the-new.win` (무시됨: 풀이 내장되어 있음) |
| Pass | 풀 비밀번호 |
| Extra config arguments | 비워 두거나 `--proxy <YOUR_PROXY>` (프록시 섹션 참고) |

- 풀에 표시되는 워커 이름은 HiveOS의 리그 이름입니다. 영문자, 숫자, `_`, `-`, `.`만 사용하세요 (공백 불가). 최대
  64자입니다. 다른 워커 이름을 쓰려면 Extra config arguments에 `-w <RIG_NAME>`을 추가하세요.
- 모델과 채굴기를 위해 HiveOS 드라이브에 약 12 GB의 여유 공간이 필요합니다 (`df -h /hive/miners`로 확인).
- 모델 (약 8.1 GiB)은 `/hive/miners/custom/.thenewminer-models`에 한 번 다운로드되며, 업그레이드해도 유지됩니다.
- 지갑과 비밀번호는 명령줄이 아니라 환경 변수를 통해 채굴기에 전달됩니다.
- 풀이 지갑이나 비밀번호를 거부하면 채굴기 로그에 그 사실이 표시되고, HiveOS가 채굴기를 다시 시작하기 전에 10분간
  대기합니다. 플라이트 시트를 수정해 적용하세요 (적용하면 채굴기가 즉시 다시 시작됩니다).
- 로그: HiveOS의 채굴기 화면 (리그 셸에서 `miner`) 또는 `/var/log/miner/custom/custom.log`.
- **업그레이드:** Installation URL의 두 버전 번호를 모두 바꾸고 (예: `.../raw/v0.1.46/thenewminer-0.1.46.tar.gz`)
  플라이트 시트를 적용하세요. HiveOS는 URL의 파일 이름이 바뀔 때만 설치합니다.
- **첫 시작은 시간이 걸립니다:** 모델 다운로드 후 RTX 30 시리즈 카드에서는 한 번만 실행되는 GPU 검사가 느린 리그
  디스크에서 시간 초과될 수 있으며, 그러면 HiveOS가 채굴기를 스스로 두세 번 다시 시작합니다 (약 8분). 그대로 두세요. 그
  후 채굴을 시작합니다.
- **이미 모델이 있나요?** 첫 시작 전에 `Qwen3-8B-Q8_0-full-v2.gguf` (약 8.1 GiB, 정확히 이 파일 이름)를 리그의
  `/home/user/`에 복사하세요. 채굴기가 이 파일을 찾아 SHA-256을 확인하고, 다시 다운로드하는 대신 이 파일을 사용합니다.

**리그가 GitHub에 접속할 수 없다면:** PC에서 `thenewminer-0.1.45.tar.gz`를 다운로드하고 (위 표에서 SHA-256을
확인하세요), 리그로 복사한 뒤 HiveOS가 사용자 지정 채굴기를 두는 위치에 압축을 풉니다. 그런 다음 평소처럼 플라이트
시트를 적용하세요. HiveOS가 "Already installed"라고 표시하고 다운로드하지 않습니다. 파일 이름은 정확히
`thenewminer-0.1.45.tar.gz`로 유지하고 (브라우저가 " (1)"을 붙일 수 있습니다), 플라이트 시트의 Installation URL은 바꾸지
마세요. HiveOS가 이 이름을 비교합니다.

```bash
# PC에서
scp thenewminer-0.1.45.tar.gz user@<RIG_IP>:/tmp/
# 리그에서 root로 (Hive Shell, 또는 SSH 접속 후 sudo -i)
mkdir -p /hive/miners/custom/downloads
mv /tmp/thenewminer-0.1.45.tar.gz /hive/miners/custom/downloads/
cd /hive/miners/custom && tar -xzf downloads/thenewminer-0.1.45.tar.gz
```

## 지갑, 풀 비밀번호 또는 워커 이름 변경

풀은 각 지급 주소를 그 주소 전용 풀 계정에 영구적으로 묶습니다. 풀 페이지 [tsc.the-new.win](https://tsc.the-new.win)은
TSC 지급 주소를 "永久绑定 · 不可修改" (영구 바인딩, 수정 불가)라고 표시하며, 바꾸려면 채굴기에서 새 주소를 직접
사용하라고 안내합니다. 따라서 다른 지갑에는 그 지갑 전용 풀 계정과 풀 비밀번호가 필요합니다 (먼저 풀 페이지에서 만들어
두세요). 풀 페이지에서 풀 비밀번호를 바꾸면 모든 채굴기에서 비밀번호를 업데이트하세요. 풀은 잊어버린 비밀번호를 복구할
수 없습니다.

**Linux:** 채굴기를 중지한 다음, 새 설정으로 한 번 시작하세요. 새 설정이 기억된 설정을 대체합니다:

```bash
./thenewminer --stop
./thenewminer -u <NEW_WALLET> -p '<PASSWORD>' -w <RIG_NAME> --background
```

`-u`와 `-p`는 반드시 함께 지정하세요. 둘 중 하나만 지정하면 기억된 지갑과 비밀번호가 바뀌지 않습니다. 워커 이름은 따로
바꿀 수 있습니다: `./thenewminer -w <RIG_NAME> --background` (`--stop` 후에).

**Windows:** `TheNewMiner.bat setup`을 실행하세요. 실행 중인 채굴기를 중지하고 지갑, 풀 비밀번호, 워커 이름을 다시
묻습니다. 그런 다음 평소처럼 시작하세요 (`TheNewMiner.bat`을 더블 클릭).

**HiveOS:** 플라이트 시트를 편집하세요. "Wallet and worker template"에 새 지갑 주소를 (`<NEW_WALLET>.%WORKER_NAME%`),
Pass에 새 풀 비밀번호를 넣은 다음 저장하고 적용하세요. 워커 이름은 HiveOS의 리그 이름입니다 (또는 Extra config
arguments의 `-w <RIG_NAME>`).

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

TLS는 항상 풀과의 사이에서 종단 간 (end to end)으로 동작하므로 프록시는 연결 내용을 읽을 수 없습니다. Windows에서는 같은 PC에서
실행 중인 프록시 앱에 WSL에서 `127.0.0.1`로 접근할 수 없는 경우가 일반적입니다 (WSL 기본 네트워킹). PC의 LAN IP를 사용하고
앱에서 LAN 연결을 허용하세요.

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

- **Ubuntu:** `sudo ubuntu-drivers install`로 권장 드라이버를 설치합니다 (설치 후 580 이상인지 확인하세요). 버전을 직접
  선택하려면 `sudo apt install nvidia-driver-580` (또는 그 이상. RTX 50 시리즈 카드에는 open 버전이 필요합니다. 예:
  `nvidia-driver-580-open`)을 실행합니다. 그런 다음 재부팅하세요. Ubuntu 드라이버 패키지와 NVIDIA `.run` 설치 프로그램 중
  하나만 사용하고, 절대 둘을 함께 사용하지 마세요.
- **Debian:** Debian 자체 드라이버 패키지는 580보다 오래된 버전입니다 (Debian 12는 535, Debian 13은 550).
  [NVIDIA Driver Installation Guide](https://docs.nvidia.com/datacenter/tesla/driver-installation-guide/debian.html) (NVIDIA 드라이버 설치 가이드)의
  설명에 따라 NVIDIA 저장소에서 더 새로운 드라이버를 설치한 다음 재부팅하세요.
- **Windows:** [NVIDIA 앱](https://www.nvidia.com/en-us/software/nvidia-app/) (이전 GeForce Experience)으로 업데이트하거나,
  [nvidia.com](https://www.nvidia.com/en-us/drivers/)에서 카드에 맞는 드라이버 (580 이상)를 다운로드하세요. WSL2는 이 Windows 드라이버를
  사용합니다. WSL 안에 Linux용 NVIDIA 드라이버를 절대 설치하지 마세요.
- **HiveOS:** root 권한으로 (Hive Shell, 또는 SSH 접속 후 `sudo -i`) `nvidia-driver-update --list`를 실행해 버전 목록을
  확인한 다음, 예를 들어 `nvidia-driver-update 595` (580 이상이면 어느 버전이든 가능)를 실행하고, 마지막으로 `sreboot`를
  실행하세요.

## 참고 해시레이트

자체 리그에서 측정한 실제 풀 기준 최고 해시레이트입니다 (2026-10-14부터 2026-10-23까지). 이는 **최대** 수치이며
보장이 아닙니다. 같은 모델의 카드라도 개별 칩, 냉각, 클럭, 전력 제한에 따라 약 5–8% 차이가 나며,
카드가 디스플레이 출력도 담당하거나 다른 프로그램이 GPU를 사용하면 해시레이트가 떨어집니다.

| GPU | 해시레이트 |
|---|---|
| RTX 5090 | 최대 ~99 N/s |
| RTX 4090 | 최대 ~70 N/s |
| RTX 3080 10 GB | 최대 ~22 N/s |

## 테스트 결과

자체 테스트 리그에서 진행한 실제 풀 테스트입니다 (머신당 GPU 한 개). A = 승인된 셰어, R = 거부된 셰어.
"안정 채굴"은 작업 전환 사이의 해시레이트 중앙값입니다.

| 날짜 | 카드 | OS | 패키지 | 해시레이트 | A / R |
|---|---|---|---|---|---|
| 2026-10-23 | RTX 4090 | Linux | v0.1.45 출시 빌드 | 65.42 N/s (안정 채굴, 16분; 작업 전환 포함 시 62.35) | 13 / 0 (무효 0) |
| 2026-10-23 | RTX 5090 | Linux | v0.1.45 출시 빌드 | 94.18 N/s (안정 채굴; 작업 전환 포함 시 92.24) | 17 / 0 (무효 0) |
| 2026-10-23 | RTX 4090 | Linux | v0.1.44 sealed-r4 | 65.58 N/s (중앙값, 16분) | 10 / 0 |
| 2026-10-23 | RTX 5090 | Linux | v0.1.44 sealed-r4 | 93.45 N/s (안정 채굴, 44분; 작업 전환 포함 시 90.39) | 31 / 0 (중지 시 stale +1) |
| 2026-10-23 | RTX 3080 10 GB | Windows (WSL2) | v0.1.44 windows-wsl2-r5 | ~22 N/s (운영자 본인 실행) | 해당 없음 |
| 2026-10-23 | RTX 3080 10 GB | Linux (Ubuntu 26.04) | v0.1.44 sealed-r4 | 21.04 N/s (23분 채굴 중 마지막 10분) | 3 / 0 |

Linux의 RTX 3080 (v0.1.44): 설치 6/6단계 성공, 유효하지 않은 윈도 없음, 정상 종료. 로그 발췌:
[`test-results/rtx3080-linux-20261023.txt`](test-results/rtx3080-linux-20261023.txt).

## 스크린샷

자체 테스트 리그에서 이전 버전으로 찍은 것입니다: RTX 4090은 v0.1.41, RTX 5090은 초기 빌드. 이는 실시간
측정값이며, 실제 수치는 카드에 따라 다릅니다.

**RTX 4090, v0.1.41:** 측정값마다 69–75 N/s, 대부분 ~70 (10분 평균 ~68), 셰어 9개 승인, 0개 거부, ~445 W.

![RTX 4090에서 채굴 중인 TheNewMiner v0.1.41](screenshots/rtx4090.png)

**RTX 5090, 초기 빌드:** 100.83 N/s (완료된 윈도 기준 실시간 평균), ~575 W.

![RTX 5090에서 채굴 중인 TheNewMiner 초기 빌드](screenshots/rtx5090.png)

## 자주 묻는 질문

**Windows에 "Windows의 PC 보호" / "알 수 없는 게시자"라고 표시됩니다.**
이 키트는 코드 서명되어 있지 않습니다. **추가 정보**를 클릭한 다음 **실행**을 클릭하세요.

**백신 프로그램이 채굴기를 탐지하거나 삭제합니다.**
Microsoft Defender 및 기타 백신 프로그램은 악성 코드가 채굴기를 숨기는 경우가 많기 때문에, 사용자가 직접 실행하기로 선택한 정상적인 채굴기라도
암호화폐 채굴기를 "잠재적으로 원치 않는 애플리케이션"으로 탐지하는 경우가 많습니다. 파일이 격리되었다면
Windows 보안 › 바이러스 및 위협 방지 › 보호 기록을 열고, TheNewMiner 항목을 선택한 뒤
**허용**을 선택하세요 (또는 압축을 푼 폴더를 제외에 추가하세요). 이 페이지에서 다운로드했고
SHA-256이 일치하는 패키지에 대해서만 이렇게 하세요.

**첫 실행이 오래 걸립니다.**
첫 실행 시 약 8.1 GiB의 모델을 다운로드하고 카드에서 GPU 자체 점검을 실행합니다. Windows에서는 WSL2 설정도
진행할 수 있습니다. 이 과정은 한 번만 일어납니다. 이후 실행에서는 훨씬 빨리 채굴이 시작됩니다.

**채굴기가 풀이 지갑 / 비밀번호를 거부했다고 표시합니다.**
지갑 주소와 풀 비밀번호를 확인하고 (풀이 계정을 승인했는지도 확인하세요), 다시 시작하세요:
Linux `./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>'`, Windows `TheNewMiner.bat setup`. 채굴기는 잘못된
인증 정보로 스스로 재시도하지 않습니다.

**채굴 중인가요? 로그는 어디에 있나요?**
Linux: `./thenewminer --status`, `./thenewminer --logs` 또는 파일 `thenewminer/.miner/thenewminer.log`.
Windows: `TheNewMiner.bat status` 및 `TheNewMiner.bat logs`.

## 보안 참고 사항

채굴기와 GPU 엔진은 패키지 안에 암호화된 상태 (ChaCha20-Poly1305)로 배포되며, 채굴기가 시작될 때 사용자의
머신에서 복호화합니다. 이렇게 하면 복사하는 비용이 높아지지만, **절대 뚫을 수 없는 것은 아닙니다**: 채굴기가 실행 중인
머신의 관리자 권한을 가진 사람이라면 누구든 충분히 노력하면 키나 복호화된 코드를 복구할 수 있습니다.
지갑 주소와 풀 비밀번호는 사용자 본인의 머신에 저장되며 풀에만 전송됩니다.
