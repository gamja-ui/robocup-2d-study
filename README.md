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
## 최초 설치 방법

아래 과정은 Ubuntu 24.04에서 처음 한 번만 진행합니다.
이미 설치한 PC에서는 아래의 경기 실행 방법부터 진행하면 됩니다.

각 명령어는 순서대로 실행합니다.
중간에 오류가 발생하면 다음 단계로 넘어가지 않고 먼저 오류를 해결합니다.

### 1. 개발 도구 및 라이브러리 설치

```bash
sudo apt update
sudo apt install build-essential git cmake automake autoconf libtool flex bison libboost-all-dev
```

모니터에 필요한 라이브러리도 설치합니다.

```bash
sudo apt install qtbase5-dev qt5-qmake libfontconfig1-dev libaudio-dev libxt-dev libglib2.0-dev libxi-dev libxrender-dev
```

설치한 개발 도구의 버전을 확인합니다.

```bash
g++ --version
cmake --version
git --version
```

### 2. 작업 폴더 생성

```bash
mkdir -p ~/robocup
cd ~/robocup
```

### 3. 경기 서버 설치 — rcssserver

공식 저장소에서 소스 코드를 다운로드합니다.

```bash
cd ~/robocup
git clone https://github.com/rcsoccersim/rcssserver.git
cd rcssserver
```

빌드 설정과 빌드를 진행합니다.

```bash
cmake -S . -B build -DCMAKE_BUILD_TYPE=Release
cmake --build build -j2
```

빌드가 성공하면 설치하고 공유 라이브러리 정보를 갱신합니다.

```bash
sudo cmake --install build
sudo ldconfig
```

### 4. 경기 모니터 설치 — rcssmonitor

```bash
cd ~/robocup
git clone https://github.com/rcsoccersim/rcssmonitor.git
cd rcssmonitor
```

```bash
cmake -S . -B build -DCMAKE_BUILD_TYPE=Release
cmake --build build -j2
```

```bash
sudo cmake --install build
sudo ldconfig
```

모니터 설치 후에도 `sudo ldconfig`를 실행합니다.
생략하면 실행 시 `librcssrcg.so.18`을 찾지 못하는 오류가 발생할 수 있습니다.

### 5. 선수 AI 라이브러리 설치 — librcsc

```bash
cd ~/robocup
git clone https://github.com/helios-base/librcsc.git
cd librcsc
```

빌드 준비와 빌드를 진행합니다.

```bash
./bootstrap
./configure --disable-unit-test
make -j2
```

빌드가 성공하면 설치합니다.

```bash
sudo make install
sudo ldconfig
```

공유 라이브러리 등록 여부를 확인합니다.

```bash
ldconfig -p | grep librcsc
```

오늘 설치한 환경에서는 다음 경로가 확인되었습니다.

```text
/usr/local/lib/librcsc.so.20
/usr/local/lib/librcsc.so
```

라이브러리 버전에 따라 숫자는 달라질 수 있습니다.

### 6. 기본 축구팀 설치 — HELIOS Base

```bash
cd ~/robocup
git clone https://github.com/helios-base/helios-base.git
cd helios-base
```

설치한 librcsc의 위치를 지정하고 빌드합니다.

```bash
./bootstrap
./configure --with-librcsc=/usr/local
make -j2
```

실행 파일과 시작 스크립트를 확인합니다.

```bash
ls -l src/sample_player src/sample_coach src/start.sh
```

이 안내에서는 별도의 `sudo make install` 없이
`~/robocup/helios-base/src`에서 팀을 실행합니다.

### 7. 설치 완료 후 확인

아래의 경기 실행 방법에 따라 다음을 순서대로 실행합니다.

1. 경기 서버
2. 경기 모니터
3. TeamA와 TeamB
4. 모니터에서 Referee → Kick Off

두 팀의 선수들이 움직이면 기본 환경 구성이 완료된 것입니다.

### 설치 기록

2026년 11월 10일, 동아리 PC에서 확인한 환경입니다.

| 항목 | 확인 내용 |
|---|---|
| 운영체제 | Ubuntu 24.04 |
| C++ 컴파일러 | GCC 13.3.0 |
| CMake | 3.28.3 |
| Git | 2.43.0 |
| 경기 서버 | rcssserver 19.0.0 |
| 경기 모니터 | rcssmonitor 19.0.1 |
| 선수 라이브러리 | librcsc 설치 및 등록 확인 |
| 기본 팀 | HELIOS Base 빌드 완료 |
| 동작 확인 | TeamA와 TeamB의 경기 실행 성공 |

위 명령어는 다운로드 시점의 기본 브랜치를 사용합니다.
나중에 저장소가 업데이트되면 버전이나 빌드 결과가 달라질 수 있습니다.

---

## 설치 후 경기 실행

이후부터는 설치를 반복하지 않고 아래 순서로 실행합니다.

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
