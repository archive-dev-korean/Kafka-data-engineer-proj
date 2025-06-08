# Kafka Data Pipeline Project

> 실시간 스트리밍 데이터 처리를 위해 **Apache Kafka 기반 데이터 파이프라인**을 구축한 프로젝트입니다.  
> 📽️ 시연 영상과 전체 결과물은 👉 [노션 포트폴리오 보기](https://magical-rate-172.notion.site/1556ab8db08980e5907add8e44deda2c)

---

## 🛠 사용 기술

- **Kafka** 3.7.0 (Scala 2.13)
- Python 3.11 (kafka-python, Flask, OpenCV 등)
- WSL (Ubuntu 22.04) 기반 Linux 환경
- IP Webcam 앱 (실시간 이미지 입력 소스)

---

## 💡 주요 기능

- 모바일 카메라 → Kafka Producer로 실시간 이미지 전송
- Kafka Consumer에서 프레임 단위로 수신 및 저장
- 저장된 이미지 데이터를 `.json` 형식으로 변환하여 후속 모델로 연계
- Flask API를 통해 외부에서 스트리밍 시작 요청 가능

---

## 🧑‍💻 기여한 역할

- Kafka 기반 데이터 스트리밍 구조 설계
- `Producer_ipwebcam.py`, `Consumer_ipwebcam.py` 직접 구현
- Flask API 구현 및 PowerShell/CMD 기반 호출 테스트
- Kafka 세팅 가이드 작성 및 오류 해결 경험 문서화

---

## 🖼️ 실행 결과

- 실시간 이미지 스트림 → 프레임 분할 및 `.json` 파일로 저장
- base64 이미지 전송 가능하도록 구조 설계


---
## 📎 추가 문서

→ 상세 설치 및 실행 방법은 [`README_detail.md`](README_detail.md)에서 확인할 수 있습니다.
