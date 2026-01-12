# 🚗 Driver Helper App (운전 도우미 시스템)

![Python](https://img.shields.io/badge/Python-3.8+-3776AB?style=flat&logo=python&logoColor=white)
![YOLOv5](https://img.shields.io/badge/YOLOv5-Object_Detection-00FFFF?style=flat)
![RaspberryPi](https://img.shields.io/badge/Raspberry_Pi_4-C51A4A?style=flat&logo=raspberrypi&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-Express-339933?style=flat&logo=nodedotjs&logoColor=white)
![React Native](https://img.shields.io/badge/React_Native-Expo-61DAFB?style=flat&logo=react&logoColor=black)

## 📖 Project Overview
**운전 도우미(Driver Helper)**는 운전자의 시야 사각지대를 보완하고, 보행자 사고를 예방하기 위해 개발된 **실시간 객체 인식 및 음성 안내 시스템**입니다.

최근 빈번하게 발생하는 **우회전 시 보행자 사고**와 **무단 횡단 사고**를 줄이기 위해, 차량 전방 영상을 실시간으로 분석하여 위험 요소를 감지하고 스마트폰 앱을 통해 운전자에게 **음성(TTS)**으로 경고합니다. 또한 버스 전용 차로 정보를 안내하여 운전 편의성을 높입니다.

---

## 🛠️ System Architecture
이 시스템은 크게 **하드웨어(수집), AI 클라이언트(분석), 웹 서버(중계), 모바일 앱(출력)**의 4단계로 구성됩니다.

1.  **Video Streaming:** `Raspberry Pi 4`와 `PiCamera 2`를 사용하여 차량 전방 영상을 실시간으로 촬영 및 송출합니다.
2.  **Object Detection:** 송출된 영상을 Client PC에서 수신하여 `YOLOv5` 알고리즘으로 객체(사람, 신호등, 횡단보도 등)를 탐지합니다.
3.  **Data Processing:** 탐지된 정보를 HTTP 프로토콜을 통해 `Node.js (Express)` 기반의 웹 서버로 전송합니다.
4.  **User Interface:** 사용자는 `Expo (React Native)`로 개발된 앱을 통해 실시간 분석 화면을 확인하고, 위험 상황 시 **TTS(Text-to-Speech)** 음성 안내를 받습니다.

---

## ✨ Key Features
### 1. 🚶 보행자 및 무단 횡단 감지
- 도로 위의 보행자를 실시간으로 인식하여 운전자에게 알림을 제공합니다.
- 무단 횡단 시 즉각적인 경고를 통해 사고를 예방합니다.

### 2. ↪️ 우회전 시 사고 예방
- 우회전 시 사각지대에 있는 횡단보도와 보행자 유무를 파악하여, "보행자 주의" 등의 음성 안내를 송출합니다.

### 3. 🚌 버스 전용 차로 안내
- 버스 전용 차로 표지판(전일제, 시간제)을 인식하여 현재 이용 가능한 차로인지 안내합니다.

### 4. 🔊 실시간 TTS 음성 안내
- 시각적인 정보뿐만 아니라 청각적인 알림(TTS)을 통해 운전자가 전방 주시에 집중할 수 있도록 돕습니다.

---

## 🧰 Tech Stack

| Category | Technology | Description |
| :--- | :--- | :--- |
| **Hardware** | Raspberry Pi 4, PiCamera 2 | 영상 수집 및 스트리밍 하드웨어 |
| **AI / Model** | YOLOv5, PyTorch, OpenCV | 실시간 객체 탐지 및 영상 처리 |
| **Backend** | Node.js, Express | 데이터 처리 및 앱 통신 서버 |
| **Frontend (App)** | React Native, Expo | 운전자용 스마트폰 애플리케이션 (WebView) |
| **Dataset** | Roboflow, AI Hub | 학습 데이터 수집 및 라벨링 |
| **OS / Tools** | Debian Linux, VS Code | 개발 환경 |

---

## 📂 Directory Structure
📦 Driver-Helper-Project 
<br/>├── 📂 f1,p,r 그래프 # 모델 성능 분석 지표 (F1 Score, Precision, Recall) 
<br/>├── 📂 raspberrypi # 라즈베리파이 스트리밍 및 설정 소스코드 
<br/>├── 📂 yolov5-master # YOLOv5 모델 학습 및 추론용 코어 디렉토리 
<br/>├── 📂 우회전,무단횡단,버스전용... # 학습 및 테스트에 사용된 원본 영상 데이터 
  <br/>├── 📄 detect.py # 객체 탐지 실행 스크립트 
  <br/>├── 📄 project.pt # 학습된 Custom YOLOv5 가중치 모델 파일 
  <br/>└── 📄 read_me.txt # 프로젝트 기본 설명 파일



---

## 📊 Dataset & Training
- **Data Source:** AI Hub 및 직접 촬영 영상, Youtube 등
- **Classes:**
  - `Person` (보행자)
  - `Crosswalk` (횡단보도)
  - `Traffic Light` (신호등)
  - `Bus Lane Sign` (버스전용차로 표지판)
- **Tool:** Roboflow를 사용하여 데이터 라벨링 및 전처리를 수행하였습니다.

---

## 📝 Result & Future Work
### 성과
- 라즈베리파이의 하드웨어 성능 한계를 극복하기 위해 **포트 포워딩 및 외부 컴퓨팅 장치(Client)**를 활용한 스트리밍 분석 구조를 설계하여 FPS를 확보했습니다.
- 스마트폰 앱과 연동하여 실제 주행 환경에서 활용 가능한 프로토타입을 구현했습니다.

### 향후 과제
- 다양한 주행 환경(야간, 우천 시)에서의 데이터셋 추가 확보를 통한 정확도 개선
- 스트리밍 지연 시간(Latency) 최소화 및 보안성 강화
- 음성 인식 기능을 추가하여 운전자가 음성으로 앱을 제어하는 기능 개발

---

## 📜 License
This project is for educational/portfolio purposes.
