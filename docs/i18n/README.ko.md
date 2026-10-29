# TheNewMiner

TheNewMiner는 VRAM 10 GB 이상의 NVIDIA GeForce RTX 30, 40, 50 시리즈 카드를 위한 **TensorCash**용 GPU 채굴기이며, 풀 **`tsc-wss.the-new.win`**이 내장되어 있습니다.

[English](../../README.md) · [简体中文](README.zh-CN.md) · [繁體中文](README.zh-TW.md) · [Русский](README.ru.md) · [Español](README.es.md) · [Português (Brasil)](README.pt-BR.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Türkçe](README.tr.md) · [Tiếng Việt](README.vi.md) · **한국어** · [日本語](README.ja.md)

> 이 번역은 편의를 위해 제공되며, 영어 README가 공식 기준입니다. *(This translation is provided for convenience; the English README is authoritative.)*

## 다운로드

현재 버전: **v0.1.47** · 풀 웹사이트: [tsc.the-new.win](https://tsc.the-new.win/#/home)

| 시스템 | 다운로드 | 다음 단계 |
|---|---|---|
| Linux x86_64 | [`TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz) | 또는 아래의 원클릭 설치 사용 |
| Windows 10 / 11 | [`TheNewMiner-v0.1.47-windows-wsl2-r3.zip`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/TheNewMiner-v0.1.47-windows-wsl2-r3.zip) | 압축을 풀고 `TheNewMiner.bat`을 더블 클릭 |
| HiveOS (실험적) | [`hiveos/flightsheet-thenewminer.json`](../../hiveos/flightsheet-thenewminer.json) | 플라이트 시트 가져오기 (아래 HiveOS 참고) |

체크섬: [`SHA256SUMS`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/SHA256SUMS). 확인 방법은 [가이드](GUIDE.ko.md)에 있습니다. 파일이 일치하지 않으면 사용하지 마세요.

## 시작하기 전에: 풀 계정 만들기

채굴하려면 풀 계정이 필요합니다. 즉 TSC 수령 주소 (지갑)와 풀 웹사이트에서 직접 정하는 풀 비밀번호입니다. 사이트에는 영어 버전이 있습니다 (사이트의 언어 메뉴를 사용하세요).

1. [tsc.the-new.win](https://tsc.the-new.win/#/home)을 열고 **Start mining** (开始挖矿)으로 이동합니다.
2. **1. Bind Wallet** 아래에 **TSC payout address**를 입력하고 (`tc1...`; 신중하게 확인하세요: 잘못된 주소로 보내진 채굴 수입은 되찾을 수 없습니다), **Pool Password** (6–128자)를 정한 다음 **Confirm Password**에 다시 입력합니다.
3. **Apply to join pool**을 클릭합니다. 페이지에 지갑이 연결되었다고 표시되며, 계정은 바로 사용할 수 있습니다.
4. 채굴기에서도 같은 지갑과 비밀번호를 사용하세요. Linux에서는 `-u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>'`, Windows에서는 `TheNewMiner.bat`의 계정 입력 창, HiveOS에서는 플라이트 시트에 입력합니다. 채굴을 시작하고 약 3–5분이 지나면 풀 웹사이트의 **My Miners**에 리그가 표시됩니다.

풀은 잊어버린 풀 비밀번호를 복구할 수 없으며, 수령 주소는 나중에 변경할 수 없습니다. 풀 비밀번호는 지갑의 개인 키가 아닙니다. 개인 키는 절대 누구에게도 알려 주지 마세요.

## 원클릭 설치 (Linux)

이 명령 끝에 있는 `<YOUR_WALLET>`, `<YOUR_POOL_PASSWORD>`, `<RIG_NAME>`을 (`<` `>` 포함) 각각 자신의 지갑, 풀 비밀번호, 이 리그의 이름으로 바꾼 다음, 터미널에 붙여 넣으세요. Linux 패키지와 `SHA256SUMS`를 다운로드하고, SHA-256을 확인한 뒤, 채굴기를 `~/thenewminer`에 압축 해제하고 백그라운드에서 시작합니다:

```bash
mkdir -p ~/thenewminer-download && cd ~/thenewminer-download && V=v0.1.47 && P=TheNewMiner-$V-linux-x86_64-sealed.tar.xz && U=https://github.com/ngasuka855/TheNewMiner/raw/$V && rm -f "$P" SHA256SUMS && if command -v curl >/dev/null 2>&1; then curl -fL -o "$P" "$U/$P" && curl -fL -o SHA256SUMS "$U/SHA256SUMS"; else wget -O "$P" "$U/$P" && wget -O SHA256SUMS "$U/SHA256SUMS"; fi && sha256sum -c --ignore-missing SHA256SUMS && tar -xJf "$P" -C ~ && cd ~/thenewminer && ./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w <RIG_NAME> --background
```

다운로드 오류나 SHA-256 불일치가 있으면 아무것도 시작되지 않습니다. 비밀번호를 감싼 작은따옴표는 그대로 두세요 (비밀번호 안의 `'`는 `'\''`로 씁니다). GPU가 하나인 머신에서는 채굴기가 모델 (약 8.1 GiB, 한 번만)을 다운로드하기 전에 지갑과 비밀번호를 풀에서 확인합니다. 그 후 `./thenewminer --logs`로 채굴하는 모습을 볼 수 있고, `./thenewminer --status`는 실행 중인지 또는 왜 중지되었는지 알려 줍니다. 업그레이드하려면 먼저 `~/thenewminer/thenewminer --stop`을 실행한 다음 같은 명령을 실행하세요. 설정과 모델은 유지됩니다. 그 밖의 모든 내용 (일상적인 명령, 업그레이드, 자동 시작, 채굴되지 않을 때의 조치)은 [가이드](GUIDE.ko.md)에 있습니다.

## Windows

1. `TheNewMiner-v0.1.47-windows-wsl2-r3.zip` 전체를 아무 곳에나 압축 해제하세요 (마우스 오른쪽 버튼 클릭 › 모두 압축 풀기).
2. **`TheNewMiner.bat`**을 더블 클릭하세요. 처음에는 창 하나가 풀 계정 (지갑, 풀 비밀번호, 워커 이름)을 묻습니다. 그 후에는 아무것도 묻지 않습니다. WSL2와 Ubuntu를 설정하고, 동봉된 채굴기를 설치하고, 모델을 다운로드한 뒤 채굴을 시작합니다. 이후에는 더블 클릭하면 바로 채굴을 시작합니다. 처음에 Windows에 "Windows의 PC 보호"라고 표시될 수 있습니다 (이 키트는 코드 서명되어 있지 않습니다). **추가 정보**를 클릭한 다음 **실행**을 클릭하세요. WSL을 켤 때 관리자 권한을 한 번 요청하며 다시 시작이 한 번 필요할 수 있습니다. 그 후 설정은 자동으로 계속됩니다.
3. 이전 버전이 있으면 이를 찾아 설정과 모델을 유지한 채 그 자리에서 업그레이드합니다.

중지되거나 채굴이 느리면 [Windows 문제 해결](WINDOWS-TROUBLESHOOTING.ko.md)을 참고하세요 (DNS, Windows 드라이브, 브라우저와 Docker Desktop이 사용하는 VRAM, 시계).

## HiveOS (실험적)

1. **Wallets** (지갑): 코인 `TSC`와 TensorCash 주소 (`tc1...`)로 지갑을 추가합니다.
2. [`hiveos/flightsheet-thenewminer.json`](../../hiveos/flightsheet-thenewminer.json)을 열어 내용을 복사하고, `<YOUR_WALLET>` (뒤의 `.%WORKER_NAME%`은 그대로 두세요)과 `<YOUR_POOL_PASSWORD>`를 바꿉니다. HiveOS에서 Flight Sheets › **Import from clipboard**를 선택해 붙여 넣고, `TSC` 지갑을 선택한 뒤 저장합니다.
3. 플라이트 시트를 리그에 적용합니다. `thenewminer-0.1.47.tar.gz`가 설치되고 채굴이 시작됩니다.

수동 설정, 업그레이드, GitHub에 접속할 수 없는 리그: [가이드](GUIDE.ko.md)를 참고하세요.

## v0.1.47의 새로운 기능

- **더 쉬운 설치:** GPU가 하나인 머신에서는 채굴기가 8.1 GiB 모델을 다운로드하기 **전에** 지갑과 풀 비밀번호를 풀에서 확인하므로, 오타를 몇 초 만에 찾아냅니다. 오타가 있는 지갑 주소 (체크섬이 일치하지 않음)나 예시 주소는 무엇이 잘못되었는지와 함께 거부됩니다.
- **Windows: 인증 정보는 한 번만 묻습니다.** 시간이 오래 걸리는 작업 전에, 처음에 창 하나에서 묻습니다. 이전 버전으로 설치한 것은 다른 WSL 배포판에 있더라도 찾아내 **그 자리에서 업그레이드**합니다 (설정과 모델 유지). Microsoft에서 Ubuntu를 다운로드할 수 없으면, 설치 과정에서 중국에 있는 미러에서 Ubuntu 공식 WSL 이미지를 받아 SHA-256이 일치할 때만 사용합니다.
- **기본 Ubuntu에서는 apt 불필요:** Ubuntu 22.04 / 24.04 (WSL에 포함된 것 포함)에는 채굴기에 필요한 모든 것이 이미 있으므로 아무것도 설치하지 않습니다. 빠진 것이 있으면 첫 실행 시 설치하기 전에 그 목록을 보여 줍니다.
- **Windows 드라이브에 있는 패키지** (WSL의 `/mnt/d` 등)는 즉시 거부되며, Linux 홈 폴더로 복사해 그곳에서 시작하는, 바로 붙여 넣을 수 있는 한 줄을 보여 줍니다.
- **명확한 네트워크 메시지:** WSL 안에서 DNS가 작동하지 않으면 설치가 그 단계에서 멈추고 해결 방법을 알려 줍니다. 인터넷이나 풀에 연결할 수 없는 채굴기는 무엇을 확인해야 하는지 알려 줍니다.
- **모델 재사용:** 새 폴더로 업그레이드하면 이전 설치의 모델 (같은 크기와 SHA-256)을 찾아 다시 다운로드하지 않습니다.
- **`--status`가 이유를 알려 줍니다:** 채굴기가 중지된 이유와 해결 방법을 알려 주며, `--background`로 시작한 후에도 마찬가지입니다.
- **HiveOS:** 이제 모든 실패에서 대기 시간을 점점 늘려 가며 (일 분에서 십 분까지) 기다렸다가 다시 시도하고, 그 이유를 로그와 대시보드에 표시합니다.

전체 릴리스 노트: [`docs/releases/RELEASE-NOTES-v0.1.47.md`](../releases/RELEASE-NOTES-v0.1.47.md).

## 알려진 문제

- **Windows의 10 GB 카드는 최대 속도를 내려면 약 9.3 GB의 여유 VRAM이 필요합니다.** 그보다 적으면 채굴기가 더 느린 레이아웃을 사용합니다 (RTX 3080 10 GB에서 21–22에서 약 9 N/s로 떨어졌습니다). 하드웨어 가속을 사용하는 브라우저나 Docker Desktop이 그 메모리를 차지할 수 있습니다. 이를 닫은 다음 (또는 브라우저의 하드웨어 가속을 끈 다음) 채굴을 다시 시작하세요.
- **Windows 시계:** 채굴기가 시계 오류를 보고하면 Windows 설정 › 시간 및 언어 › 날짜 및 시간을 열고 "자동으로 표준 시간대 설정"과 "자동으로 시간 설정"을 켠 다음 **지금 동기화**를 클릭하고, `wsl --shutdown`을 실행한 뒤 다시 시작하세요.

## 해시레이트

자체 리그에서 측정한 실제 풀 기준 최고 해시레이트입니다. 이는 **최대** 수치이며 보장이 아닙니다. 같은 모델의 카드라도 약 5–8% 차이가 나며, 카드가 디스플레이 출력도 담당하거나 다른 프로그램이 GPU를 사용하면 해시레이트가 떨어집니다.

| GPU | 해시레이트 |
|---|---|
| RTX 5090 | 최대 ~99 N/s |
| RTX 4090 | 최대 ~70 N/s |
| RTX 3080 10 GB | 최대 ~22 N/s |

버전별 날짜가 기록된 측정 결과: [테스트 결과](../TEST-RESULTS.md). 프로젝트의 주요 이정표: [개발 기록](../DEVELOPMENT-RECORD.md).

## 문제 신고

[GitHub Issues](https://github.com/ngasuka855/TheNewMiner/issues/new/choose)에서 이슈를 열고 버그 신고 양식을 작성하세요: TheNewMiner 버전, 시스템, GPU 모델과 드라이버, `./thenewminer --status` 출력, 마지막 로그 줄. 이슈는 공개됩니다. **풀 비밀번호나 지갑의 개인 키는 절대 게시하지 마세요**. 붙여 넣는 내용에서는 비밀번호를 (원한다면 지갑 주소도) 제거하세요.

## 보안 참고 사항

채굴기와 GPU 엔진은 패키지 안에 암호화된 상태 (ChaCha20-Poly1305)로 배포되며, 채굴기가 시작될 때 사용자의 머신에서 복호화합니다. 이렇게 하면 복사하는 비용이 높아지지만, **절대 뚫을 수 없는 것은 아닙니다**: 채굴기가 실행 중인 머신의 관리자 권한을 가진 사람이라면 누구든 충분히 노력하면 키나 복호화된 코드를 복구할 수 있습니다. 지갑 주소와 풀 비밀번호는 사용자 본인의 머신에 저장되며 풀에만 전송됩니다.
