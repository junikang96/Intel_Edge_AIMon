# 🧸 AIMon

## 📌 개요

**AIMon**은 어린이의 직관적인 움직임과 말로 상호작용할 수 있도록 설계되었습니다.

OpenPose 기반의 "무궁화 꽃이 피었습니다" 게임과  
YOLOv8 기반의 동물 인식 퀴즈를 통합하여,  
아이들이 실시간으로 몸을 움직이고 말하며 **AI와 자연스럽게 놀 수 있도록** 구현되었습니다.

AI(AI + 아이)와 포켓몬처럼 귀여운 느낌의 "mon"을 합쳐 AIMon이라는 이름이 붙여졌습니다.  
실시간 자세 인식, 음성 명령 제어, 이미지 인식이 모두 통합된 **AI 체험형 키즈 게임 플랫폼**입니다.

> ⚠️ 본 프로젝트는 **Intel RealSense 카메라(D400 시리즈 이상)**가 필요합니다.  
> RGB-D 카메라의 **깊이 정보**를 활용하여 전진 거리 측정 및 게임 판정에 사용됩니다.

---

## 🎮 주요 게임 기능

### ✅ 무궁화 꽃이 피었습니다 (OpenPose 기반)
- 랜덤 속도의 TTS 이후 3초간 움직임 감지
- 스켈레톤 좌표의 이동량 평균으로 판정
- 일정 거리 전진 시 승리 처리

### ✅ 동물 알려주기 게임 (YOLOv8 기반)
- 실시간 동물 탐지
- 탐지된 동물 클래스에 맞는 설명을 TTS로 출력
- 현재 고양이, 개, 코끼리, 얼룩말, 코뿔소, 버팔로 등 6종 지원

---

## 🧠 시스템 구성

| 구성 요소 | 주요 기능 | 설명 |
|-----------|-----------|------|
| **game_client.py** | 메인 클라이언트 | OpenPose 및 YOLO 기반 게임 통합 실행 |
| **voice_control_client.py** | 음성 명령 인식 | Google STT로 “무궁화”, “동물 감지” 명령을 인식하여 서버에 전송 |
| **YOLOv8 모델 (`best.pt`)** | 동물 인식 | 사전 학습된 커스텀 모델로 고양이, 개 등 탐지 |
| **OpenPose** | 사람 자세 추적 | 움직임 감지 기반 게임 인터페이스 제공 |
| **Intel RealSense** | 거리 인식 | 전진 여부 판단을 위한 실시간 깊이 센서 |
| **서버 (C)** | 명령 수신 및 제어 | 클라이언트로부터 명령을 수신하고 게임 실행을 지시 |

---

## 🧑‍💻 담당 역할

이 프로젝트는 팀 단위로 진행되었으며,  
저는 **OpenPose를 활용한 '무궁화 꽃이 피었습니다' 게임 기능을 직접 개발**하고,  
**다른 팀원이 구현한 YOLOv8 기반 동물 감지 및 음성 출력 기능을 게임 클라이언트에 통합**하는 역할을 맡았습니다.

또한 전체 클라이언트 구조를 정리하고, GitHub 문서화 및 프로젝트 배포를 주도적으로 담당했습니다.

### 💡 주요 기여 내용
- OpenPose Python API 직접 빌드 및 환경 구성
- 움직임 감지를 위한 관절 좌표 분석 및 판별 알고리즘 구현
- 클라이언트 내 OpenPose, YOLO, TTS 기능 통합 및 상태 전환 로직 설계
- 프로젝트 구조 정리 및 README 등 문서 작성

---

## ⚙️ 사용 기술

| 분류 | 기술 |
|------|------|
| 사람 자세 추적 | OpenPose (BODY_25), Python C++ 연동 |
| 객체 인식 | YOLOv8, Ultralytics |
| 음성 인식 | Google SpeechRecognition API |
| 음성 출력 | gTTS, mpg123 |
| 거리 인식 | Intel RealSense + pyrealsense2 |
| 통신 방식 | TCP 소켓 (C 서버 ↔ Python 클라이언트) |
| 개발 환경 | Python 3.8, Ubuntu 22.04, Conda 가상환경 |

---

## 🎥 시연 영상

- [👉 무궁화 꽃이 피었습니다 게임 (성공 시)](https://youtu.be/Ey0tCSUxD-c)
- [👉 무궁화 꽃이 피었습니다 게임 (실패 시)](https://youtu.be/dx5mJ6N6e1I)
- [👉 동물 인식 게임](https://youtu.be/UyEDP6XNB84)

---

## 📁 폴더 구조

```bash
INTEL_EDGE_AIMon/
├── client/
│   ├── game_client.py
│   ├── voice_control_client.py
│   └── requirements.txt
│
├── models/
│   └── yolo/
│       └── best.pt
│
├── server/
│   ├── Makefile
│   └── server.c
│
├── LICENSE
└── README.md
```

---

## 🧰 시스템 의존성 (추가 설치 필요)

```bash
# mp3 재생을 위한 CLI 오디오 플레이어
sudo apt-get install mpg123

# Intel RealSense SDK (pyrealsense2 사용을 위해 필수)
sudo apt-get install librealsense2-dev
```

> 또한, OpenPose는 pip으로 설치할 수 없으며, 직접 빌드하여 Python 바인딩(pyopenpose)을 구성해야 합니다.  

---

## 📦 OpenPose 모델 설치

OpenPose Python API를 사용하기 위해 다음 경로에 모델이 있어야 합니다:

```
models/pose/body_25/pose_iter_584000.caffemodel
```

📌 [OpenPose 모델 다운로드 안내](https://github.com/CMU-Perceptual-Computing-Lab/openpose/blob/master/doc/installation/0_index.md#models-download)

> 모델은 GitHub 용량 제한으로 인해 직접 다운로드 받아야 하며, `game_client.py`는 `models/` 기준으로 경로를 찾습니다.

---

## 🚀 실행 방법

### 1. 서버 실행

```bash
cd server
make
./server
```

### 2. 클라이언트 실행

```bash
cd client
python game_client.py         # 게임 클라이언트 직접 실행
python voice_control_client.py  # 음성 명령 기반 자동 전환
```

---

## 📜 라이선스

MIT License
