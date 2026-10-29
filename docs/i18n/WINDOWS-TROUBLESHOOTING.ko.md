[English](../WINDOWS-TROUBLESHOOTING.md) · [简体中文](WINDOWS-TROUBLESHOOTING.zh-CN.md) · [繁體中文](WINDOWS-TROUBLESHOOTING.zh-TW.md) · [Русский](WINDOWS-TROUBLESHOOTING.ru.md) · [Español](WINDOWS-TROUBLESHOOTING.es.md) · [Português (Brasil)](WINDOWS-TROUBLESHOOTING.pt-BR.md) · [Français](WINDOWS-TROUBLESHOOTING.fr.md) · [Deutsch](WINDOWS-TROUBLESHOOTING.de.md) · [Türkçe](WINDOWS-TROUBLESHOOTING.tr.md) · [Tiếng Việt](WINDOWS-TROUBLESHOOTING.vi.md) · **한국어** · [日本語](WINDOWS-TROUBLESHOOTING.ja.md)

> 이 번역은 편의를 위해 제공되며, 영어 README가 공식 기준입니다. *(This translation is provided for convenience; the English README is authoritative.)*

# Windows 문제 해결

Windows의 TheNewMiner는 WSL2 안에서 Linux 채굴기를 실행합니다. 설치가 멈추면 창에 어느 단계에서 멈췄는지와 무엇을 해야 하는지가 표시되며, `TheNewMiner.bat`을 실행하면 다시 보여 줍니다 (`%LOCALAPPDATA%\TheNewMiner\last-setup-failure.txt`에도 저장됩니다). 이 페이지에서는 흔한 경우를 설명합니다. 설치와 일상적인 명령은 [가이드](GUIDE.ko.md)에 있습니다.

## WSL 안에서 DNS가 작동하지 않음

설치 과정은 단계 4에서 WSL이 풀과 모델 다운로드 주소를 확인 (resolve)할 수 있는지 검사합니다. 확인할 수 없으면 설치가 그 자리에서 멈추며, 스스로는 아무것도 바꾸지 않습니다. 메모장에서 `%UserProfile%\.wslconfig`를 열고 (파일이 없으면 만드세요) 다음 두 줄을 추가하세요 (Windows 11 22H2 이상에서는 `networkingMode=mirrored`를 `[wsl2]` 아래에 넣어도 됩니다):

```ini
[wsl2]
dnsTunneling=true
```

그런 다음 명령 프롬프트에서 `wsl --shutdown`을 실행하고 (모든 WSL 배포판이 중지되며, Docker Desktop의 배포판도 포함됩니다) `TheNewMiner.bat`을 다시 더블 클릭하세요.

WSL을 차단하는 VPN이나 방화벽 때문에 같은 메시지가 나타날 수도 있습니다. WSL을 허용하거나, `TheNewMiner.bat setup -Proxy ...`로 프록시를 설정하세요 ([가이드](GUIDE.ko.md)의 "프록시" 참고).

## Ubuntu를 다운로드할 수 없거나 가상화가 꺼져 있음

- **네트워크:** 설치 과정에서 `wsl --install`로 Ubuntu 24.04를 설치합니다. 그 다운로드가 연결되지 않는 곳 (예: GitHub에 접속할 수 없는 곳)에서는 대신 중국의 공개 미러에서 Ubuntu 공식 WSL 이미지를 다운로드하고, SHA-256이 일치할 때만 사용합니다. 어느 미러도 응답하지 않으면 인터넷 연결을 확인하고 나중에 다시 시도하세요.
- **가상화:** 가상화에 관한 메시지 (예: `0x80370102`)는 가상화가 꺼져 있다는 뜻입니다. BIOS에서 Intel VT-x / AMD SVM을 켠 다음 `TheNewMiner.bat`을 다시 더블 클릭하세요.

## 패키지가 Windows 드라이브에 있음

Linux 패키지를 직접 압축 해제하고 WSL 안에서 `./thenewminer`를 Windows 드라이브 (예: `/mnt/d/...`)에서 시작했다면, 채굴기는 시작을 거부합니다. Windows 드라이브에서는 채굴기의 비공개 설정 파일을 유지할 수 없기 때문입니다. 채굴기는 폴더를 Linux 홈 폴더로 복사하고 그곳에서 시작하는, 바로 붙여 넣을 수 있는 `mkdir -p ~/thenewminer && cp -a ...` 줄을 출력합니다. 또는 대신 Windows용 zip을 사용하세요. `TheNewMiner.bat`이 모든 것을 알아서 올바른 위치에 둡니다.

## 10 GB 카드에서 느림 (브라우저, Docker Desktop)

10 GB 카드는 채굴이 시작될 때 약 9.3 GB (9472 MiB)의 VRAM이 비어 있어야만 빠른 레이아웃을 사용합니다. Windows에서는 다른 프로그램이 카드를 함께 사용합니다. 하드웨어 가속을 사용하는 브라우저나 Docker Desktop이 VRAM을 많이 차지하면 채굴기가 더 느린 레이아웃으로 전환됩니다. RTX 3080 10 GB에서는 21–22 N/s가 아니라 약 9 N/s였습니다.

1. 브라우저를 닫거나 하드웨어 가속을 끄고 (Chrome과 Edge: 설정 › 시스템 › "가능한 경우 그래픽 가속 사용", Firefox: 설정 › 일반 › 성능), Docker Desktop을 종료하세요.
2. 명령 프롬프트에서 여유 VRAM을 확인하세요: `nvidia-smi --query-gpu=memory.used,memory.total --format=csv` (total에서 used를 뺀 값이 9472 MiB 이상이어야 합니다).
3. 채굴을 다시 시작하세요: `TheNewMiner.bat stop`을 실행한 다음 `TheNewMiner.bat`을 더블 클릭합니다.

12 GB 카드도 같은 9472 MiB의 여유 VRAM이 필요합니다. 16 GB 이상인 카드는 영향을 받지 않습니다.

## 시계 오류

풀은 시계가 약 30초 어긋난 채굴기를 중지시킵니다. WSL2의 시계는 예를 들어 PC가 절전 모드에 들어갔다 나온 후 어긋날 수 있으며, 어긋나 있으면 설치 과정에서 경고합니다. 채굴기가 시계 오류를 보고하면:

1. Windows 설정 › 시간 및 언어 › 날짜 및 시간을 열고 "자동으로 표준 시간대 설정"과 "자동으로 시간 설정"을 켠 다음 **지금 동기화**를 클릭하세요.
2. 명령 프롬프트에서 `wsl --shutdown`을 실행하세요.
3. 채굴을 다시 시작하세요.

## Windows 보안 경고

SmartScreen ("Windows의 PC 보호")과 채굴기를 탐지하는 백신 프로그램: [가이드](GUIDE.ko.md)의 자주 묻는 질문을 참고하세요.

## 그래도 해결되지 않나요?

[GitHub Issues](https://github.com/ngasuka855/TheNewMiner/issues/new/choose)에서 이슈를 열고 `TheNewMiner.bat status`의 출력, 실패한 단계, `last-setup-failure.txt`의 내용을 첨부하세요. 풀 비밀번호나 지갑의 개인 키는 절대 게시하지 마세요.
