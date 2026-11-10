# robocup-2d-study
RoboCup 2D Soccer
# RoboCup 2D Soccer 실행 방법

Ubuntu 24.04에서 설치와 빌드를 완료한 후, 경기를 다시 실행하는 방법입니다.

## 설치된 프로그램

- rcssserver: 경기 시뮬레이션 서버
- rcssmonitor: 경기 화면 표시
- librcsc: 선수 AI 개발용 라이브러리
- HELIOS Base: 기본 축구팀 프로그램

작업 폴더: `~/robocup`

## 1. 경기 서버 실행 — 터미널 1

터미널을 열고 실행합니다.

```bash
mkdir -p ~/robocup/matches
cd ~/robocup/matches
rcssserver
```

`Hit CTRL-C to exit`가 표시되면 서버가 실행된 상태입니다.
이 터미널은 켜 둡니다.

## 2. 경기 모니터 실행 — 터미널 2

새 터미널을 열고 실행합니다.

```bash
rcssmonitor
```

축구장 화면이 나타납니다.
서버와 모니터는 계속 실행해 둡니다.

## 3. 두 팀 실행 — 터미널 3

새 터미널을 열고 첫 번째 팀을 실행합니다.

```bash
cd ~/robocup/helios-base/src
./start.sh -t TeamA
```

몇 초 기다린 후 같은 터미널에서 두 번째 팀을 실행합니다.

```bash
./start.sh -t TeamB
```

- 각 팀의 실행 명령은 한 번씩만 입력합니다.
- `-t`는 팀 이름을 지정하는 옵션입니다.
- 현재 두 팀은 같은 기본 AI를 사용합니다.

## 4. 경기 시작

모니터 상단 메뉴에서 **Referee → Kick Off**를 선택합니다.

선수들이 움직이고 경기 사이클 숫자가 증가하면 정상입니다.

## 5. 경기 종료

새 터미널에서 선수와 코치 프로그램을 종료합니다.

```bash
pkill -u "$(id -u)" -x sample_player
pkill -u "$(id -u)" -x sample_coach
```

위 명령은 현재 사용자 계정으로 실행 중인 모든 샘플 선수와 코치를 종료합니다.

그다음 다음 순서로 종료합니다.

1. 서버 터미널에서 `Ctrl + C`를 누릅니다.
2. 모니터 창을 닫습니다.

새 경기를 실행하려면 1~4단계를 반복합니다.

## 문제 해결

### 공유 라이브러리 오류

`error while loading shared libraries`가 나오면 실행합니다.

```bash
sudo ldconfig
```

이후 해당 프로그램을 다시 실행합니다.

### 서버 포트 사용 중

`Address already in use`가 나오면 기존 서버를 확인합니다.

```bash
pgrep -a -u "$(id -u)" -x rcssserver
```

기존 서버를 사용하거나, 기존 서버 터미널에서 `Ctrl + C`로 종료한 후 다시 실행합니다.

### 선수가 나타나지 않을 때

서버가 실행 중인지 확인하고, 팀 실행 터미널의 오류 메시지를 확인합니다.
팀 실행 명령을 반복해서 입력하지 않습니다.

## 공식 및 관련 저장소

- [rcssserver](https://github.com/rcsoccersim/rcssserver)
- [rcssmonitor](https://github.com/rcsoccersim/rcssmonitor)
- [librcsc](https://github.com/helios-base/librcsc)
- [HELIOS Base](https://github.com/helios-base/helios-base)
