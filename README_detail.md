# Kafka-data-engineer-proj
ApacheKafka를 활용한 데이터 파이프라인 프로젝트</br>

전체 시연 영상 및 결과물은 👉 [노션 포트폴리오](https://magical-rate-172.notion.site/1556ab8db08980e5907add8e44deda2c)에서 확인하실 수 있습니다.

실시간 데이터 스트리밍을 위해 Apache Kafka 사용.</br> 
Ipwebcam 어플리케이션을 설치해서 앱과 연결</br>
실시간 이미지 처리를 위한 프레임별 이미지 저장</br>


<h1>Apache Kafka 로컬에서 실행</h1>
선행 조건 : 태블릿pc 또는 모바일 기기에 ipwebcam apk 다운로드, 리눅스 설치


apk 설치 후 앱이 백그라운드에서 실행 가능하도록 설정.

![99999999999999999999](https://github.com/user-attachments/assets/03403095-1f93-4abc-9fc6-f5342014c6e8)

이 부분 확인해서 apk의 로컬 주소 확인(위 이미지는 예시 이미지임)
</br>
</br>

리눅스 설치 방법 
  
 - Windows에서 cmd와 Powershell을 관리자 권한으로 실행.</br>
- ` wsl --install Ubuntu-22.04` 입력 (본 프로젝트에서는 22.04.4 버전을 사용)</br>

--------------------------

><h2>Apahce Kafka 설치</br>
https://kafka.apache.org/downloads</br>
3.7.0, Scala 2.13 버전 설치</br>
압축 해제</br>

><h2>리눅스로 Kafka 설치하는 법</h2>
설치된 wsl 실행 후</br>
원하는 버전의 다운로드 링크 주소 복사 후</br>
![0000](https://github.com/user-attachments/assets/0324ad39-7ce8-48d2-8bfb-1154f39c133c)</br>
 `wget https://downloads.apache.org/kafka/3.7.0/kafka_2.13-3.7.0.tgz` 실행해서 다운로드</br>
 `tar xzvf kafka_2.12-3.7.0.tgz` 실행해서 압축 해제</br> 

><h2>실행법</h2>
압축 해제된 폴더로 이동</br>
- `./bin/zookeeper-server-start.sh ./config/zookeeper.properties` 입력해서 zookeeper 실행</br>
- `./bin/kafka-server-start.sh ./config/server.properties` 입력해서 브로커(서버) 실행</br>
- 각각 다른 wsl로 실행. 즉, 2개의 wsl에 각각 zookeeper, 브로커 실행</br>
- 다시 wsl실행해서 `./bin/kafka-topics.sh --create --topic video_stream --bootstrap-server localhost:9092 --partitions 1 --replication-factor 1
` 실행해서 video-stream이라는 topic 생성.</br>
><h3>실행 환경</h3>
- Kafka-python 모듈을 다운로드 터미널에 `pip install kafk-python==1.4.3` 입력해서 다운로드.
- requests 모듈 다운로드 터미널에 `pip install requests==2.31.0` 입력해서 다운로드.
- Flask 모듈 다운로드 `pip install Flask==3.0.3` 입력해서 다운로드.
- cv2 모듈 다운로드 `pip install opencv-python==4.9.0.80` 다운로드.
- **Python 버전 3.11.5**에서 코드가 작성되고 실행되었음.
><h2>실행 - API호출 및 데이터 스트리밍</h2>
- Producer_ipwebcam.py 코드 실행
- Powershell 에서 api 호출시에는 ` $headers = @{
     "Content-Type" = "application/json"
 }
 $body = @{
     video_id = "원하는 video_id 지정"
     max_frames = 원하는 최종프레임값 지정(초당 20프레임 데이터 전송 구성됨)
     ip_address = "10.41.0.154(ipwebcam애플리케이션 ip주소)" 
 } | ConvertTo-Json
 Invoke-RestMethod -Uri http://localhost:5000/start_stream -Method Post -Headers $headers -Body $body`입력</br>
 - 윈도우 CMD에서 api 호출 시에는 `curl -X POST http://localhost:5000/start_stream ^
     -H "Content-Type: application/json" ^
     -d "{\"video_id\": \"원하는 video_id 지정\", \"max_frames\": 원하는 최종프레임값 지정, \"ip_address\": \"ipwebcam 앱 로컬 주소\"}"
` 입력

 
 원하는 값을 할당 후에 호출, Consumer_ipwebcam.py 실행하면 Kafka가 데이터 스트리밍을 수행함.
 
------
<h2>⚠️만약 실행이 되지 않는다면⚠️</h2>


wsl 에서 `vim ./config/server.properties` 실행해서 변수를 직접 설정해야함.</br>
또는 직접 경로에 있는 파일을 클릭해서 수정하는 방법도 있음.</br>
주석으로 처리되어 있는 **listeners=PLAINTEXT://** 을 listeners=PLAINTEXT://0.0.0.0:9092 로 수정</br>
**num.partitions**를 num.partitions=1로 수정</br>
**advertised.listeners=PLAINTEXT:** 를 advertised.listeners=PLAINTEXT://localhost:9092로 수정이 이미 되어 있다면 주석 해제만 하면 됨.</br>

<h2> 결과 </h2>

라이브 동영상이 프레임 단위로 나뉘어짐.</br>
현재 경로에 .json 폴더가 생김.</br>
폴더 안에 프레임 단위로 나뉜 프레임 이미지가 json형식에 맞게 저장됨.</br>
json형식의 파일의 데이터가 gaze,blink,emotion 모델로 전송됨.(이후의 모델에서 base64 형태로 저장된 이미지를 디코딩하면서 활용할 것임.)
