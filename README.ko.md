[English](README.md) · [简体中文](README.zh-CN.md) · [繁體中文](README.zh-TW.md) · [Русский](README.ru.md) · [Español](README.es.md) · [Português (Brasil)](README.pt-BR.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Türkçe](README.tr.md) · [Tiếng Việt](README.vi.md) · **한국어** · [日本語](README.ja.md)

> 이 번역은 편의를 위해 제공되며, 영어 README가 공식 기준입니다. *(This translation is provided for convenience; the English README is authoritative.)*

# TheNewMiner

TheNewMiner는 **TensorCash**용 GPU 채굴기입니다. **VRAM 10 GB 이상**의 NVIDIA GeForce RTX 30, 40, 50 시리즈 카드에서 동작하며
풀 **`tsc-wss.the-new.win`**에서 채굴합니다. 이 풀은 내장되어 있으므로 풀 주소를 입력할 필요가 없고,
직접 연결됩니다.

현재 버전: **v0.1.44**

## 다운로드

| 파일 | 대상 | SHA-256 |
|---|---|---|
| [`TheNewMiner-v0.1.44-linux-x86_64-sealed-r4.tar.xz`](https://github.com/ngasuka855/TheNewMiner/raw/main/TheNewMiner-v0.1.44-linux-x86_64-sealed-r4.tar.xz) | Linux x86_64 (Ubuntu) | `99a8246e242c7d09f1c4b6335d974de0d163362d4dc634fff4188ab58a151ca6` |
| [`TheNewMiner-v0.1.44-windows-wsl2-r5.zip`](https://github.com/ngasuka855/TheNewMiner/raw/main/TheNewMiner-v0.1.44-windows-wsl2-r5.zip) | Windows 10 / 11 (WSL2를 통해 실행) | `264025136cf01d87d25ca2bb0554fe88efe01833850a697247cd657027215506` |
| [`SHA256SUMS`](https://github.com/ngasuka855/TheNewMiner/raw/main/SHA256SUMS) | 두 패키지의 체크섬 | |

Windows용 zip은 올인원입니다. Linux 채굴기가 이미 들어 있으므로 zip만 있으면 됩니다.

## 다운로드 검증

패키지와 `SHA256SUMS`를 같은 폴더에 넣은 다음, 아래를 실행하세요:

```bash
# Linux (패키지를 하나만 다운로드했다면 --ignore-missing 추가)
sha256sum -c SHA256SUMS
# macOS
shasum -a 256 -c SHA256SUMS
```

```bat
:: Windows (명령 프롬프트): 출력값을 위 표의 SHA-256과 비교하세요
certutil -hashfile TheNewMiner-v0.1.44-windows-wsl2-r5.zip SHA256
```

각 파일은 `OK`로 표시되어야 합니다 (Windows에서는 표와 동일한 64자). 일치하지 않으면 사용하지 마세요.

## Linux

```bash
tar -xJf TheNewMiner-v0.1.44-linux-x86_64-sealed-r4.tar.xz
cd thenewminer
./thenewminer -u <YOUR_WALLET> -p <YOUR_POOL_PASSWORD> -w rig-01 --background
```

| 명령 | 동작 |
|---|---|
| `./thenewminer -u <YOUR_WALLET> -p <YOUR_POOL_PASSWORD> -w rig-01 --background` | 첫 실행: 필요한 것을 설치한 뒤 백그라운드에서 채굴 |
| `./thenewminer --background` | 이후 실행: 설정이 기억되어 있음 |
| `./thenewminer` | 포그라운드에서 채굴 |
| `./thenewminer --logs` | 채굴기 상태 보기 (Ctrl+C로 보기를 종료해도 채굴은 계속됨) |
| `./thenewminer --stop` | 채굴 중지 |

- `-u`는 TensorCash 지갑 주소, `-p`는 풀 비밀번호, `-w`는 풀에 표시되는 워커 이름입니다
  (기본값: 이 머신의 호스트 이름).
- 설정은 `thenewminer/.miner/`에 기억됩니다 (비밀번호가 담긴 파일은 본인만 읽을 수 있습니다).
- 첫 실행 시 모델 (약 8.1 GiB, 한 번만)을 다운로드하고, 카드에 맞는 GPU 엔진을 설치하고, 카드에서
  자체 점검을 실행한 뒤 채굴을 시작합니다. `[ RATE] ... N/s` 같은 줄이 보이면 채굴 중입니다.
- 첫 실행 시 `apt`로 몇 가지 Ubuntu 패키지를 설치할 수 있습니다 (sudo 또는 root 필요).
- 이번 릴리스에서는 머신당 NVIDIA GPU가 정확히 한 개여야 합니다 (멀티 GPU 리그는 아직 지원되지 않습니다).
- `tar`가 `xz: Cannot exec`라고 표시하면 먼저 xz를 설치하세요: `sudo apt-get install -y xz-utils`.
- 패키지 안의 `README.md`는 예전 내부 메모입니다. 이 페이지를 따르세요 (`--proxy`는 필요 없습니다).
- **업그레이드:** `./thenewminer --stop`을 실행하고, 새 패키지를 같은 위치에 압축 해제한 뒤 (`thenewminer/`에 풀리며,
  설정과 모델이 들어 있는 `thenewminer/.miner/`는 그대로 유지됩니다) 다시 시작하세요.

## Windows (원클릭)

1. `TheNewMiner-v0.1.44-windows-wsl2-r5.zip` 전체를 압축 해제하세요 (마우스 오른쪽 버튼 클릭 › 모두 압축 풀기). 위치는 어느 폴더, 어느
   드라이브든 상관없으며, 공백이나 중국어 문자가 포함된 경로도 괜찮습니다. zip 내용 보기 화면 안에서 바로 실행하지 마세요.
2. **`TheNewMiner.bat`**을 더블 클릭하세요.
   - **처음 실행 시:** Windows, 드라이버, 디스크를 확인하고 필요하면 WSL2를 켭니다 (관리자 확인 창이 한 번 표시됩니다.
     재부팅이 한 번 필요할 수 있으며, 그 후 설치가 자동으로 계속됩니다). 데이터가 저장될 위치를 보여 주고 (Enter로
     확정), 자체 Linux (WSL) 환경을 만들고, 동봉된 채굴기를 설치한 다음, 지갑
     주소 (`tc1`로 시작), 풀 비밀번호 (두 번 입력; 6–128자의 영문자, 숫자, `_` 또는 `-`), 워커 이름
     (Enter = 컴퓨터 이름)을 **한 번만** 묻고, 모델을 다운로드한 뒤 채굴을 시작합니다.
   - **그다음부터는 매번:** 더블 클릭하면 저장된 설정으로 바로 채굴을 시작합니다. 바탕 화면 바로 가기
     "TheNewMiner"도 만들어집니다.

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
- **업그레이드:** 새 버전을 아무 곳에나 압축 해제하고 그 안의 `TheNewMiner.bat`을 더블 클릭하세요. 설치된 채굴기를 찾아
  설정과 모델을 유지한 채, 아무것도 묻지 않고 새 버전을 설치합니다. 이전 폴더는 삭제해도 됩니다.
- **제거:** `TheNewMiner.bat uninstall`.
- 화면 언어는 Windows 언어를 따릅니다 (English / 简体中文 / 繁體中文). 직접 고르려면 아무 명령에나 `-Lang en` (또는 `zh-Hans`,
  `zh-Hant`)을 추가하세요. 예: `TheNewMiner.bat -Lang en`. 해당 실행에만 적용됩니다.

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
| GPU | NVIDIA GeForce RTX 30 / 40 / 50 시리즈 카드 정확히 한 개, VRAM 10 GB 이상 |
| 드라이버 | NVIDIA **580 이상** (`nvidia-smi`가 작동해야 함). Windows에서는 Windows에 설치하고, 절대 WSL 안에 설치하지 마세요 |
| Linux | x86_64 Ubuntu 22.04, 24.04 또는 26.04 (다른 배포판은 아직 지원되지 않음) |
| Windows | Windows 10 21H2 이상 또는 Windows 11, 64비트. BIOS에서 가상화 (Intel VT-x / AMD SVM) 켜기 |
| 디스크 | 약 20 GB 여유 공간 |
| RAM | 4 GB 이상 |
| 모델 | 약 8.1 GiB, 첫 실행 시 한 번 다운로드 (병렬, 다중 미러)하며 이후 재사용 |

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

2026-10-23에 자체 테스트 리그에서 진행한 v0.1.44 실제 풀 테스트입니다 (머신당 GPU 한 개). A = 승인된 셰어, R = 거부된
셰어.

| 날짜 | 카드 | OS | 패키지 | 해시레이트 | A / R |
|---|---|---|---|---|---|
| 2026-10-23 | RTX 4090 | Linux | sealed-r4 | 65.58 N/s (중앙값, 16분) | 10 / 0 |
| 2026-10-23 | RTX 5090 | Linux | sealed-r4 | 93.45 N/s (안정 채굴, 44분; 작업 전환 포함 시 90.39) | 31 / 0 (중지 시 stale +1) |
| 2026-10-23 | RTX 3080 10 GB | Windows (WSL2) | windows-wsl2-r5 | ~22 N/s (운영자 본인 실행) | 해당 없음 |
| 2026-10-23 | RTX 3080 10 GB | Linux (Ubuntu 26.04) | sealed-r4 | 21.04 N/s (23분 채굴 중 마지막 10분) | 3 / 0 |

Linux의 RTX 3080: 설치 6/6단계 성공, 유효하지 않은 윈도 없음, 정상 종료. 로그 발췌:
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

**로그는 어디에 있나요?**
Linux: `./thenewminer --logs` 또는 파일 `thenewminer/.miner/thenewminer.log`.
Windows: `TheNewMiner.bat logs` (최근 해시레이트는 `TheNewMiner.bat status`).

**HiveOS는요?**
HiveOS용 패키지는 **곧 출시**됩니다. 아직 테스트 중입니다.

## 보안 참고 사항

채굴기와 GPU 엔진은 패키지 안에 암호화된 상태 (ChaCha20-Poly1305)로 배포되며, 채굴기가 시작될 때 사용자의
머신에서 복호화합니다. 이렇게 하면 복사하는 비용이 높아지지만, **절대 뚫을 수 없는 것은 아닙니다**: 채굴기가 실행 중인
머신의 관리자 권한을 가진 사람이라면 누구든 충분히 노력하면 키나 복호화된 코드를 복구할 수 있습니다.
지갑 주소와 풀 비밀번호는 사용자 본인의 머신에 저장되며 풀에만 전송됩니다.
