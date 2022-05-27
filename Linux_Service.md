# [Linux] 서비스 등록하기
## 파일 생성
> vi /etc/systemd/system/logstash.service
> 
```
[Unit]
Description=Logstash

[Install]
WantedBy=multi-user.target

[Service]
User=root
Environment="JAVA_HOME=/home/ubuntu/app/jdk"
# Type=forking # 자식 프로세스 생성 후 실행이 필요할 경우 지정
ExecStart=/home/ubuntu/app/logstash/bin/logstash -f /home/ubuntu/app/logstash/config/logstash.conf
```

## 서비스 등록
> systemctl enable logstash  
> systemctl daemon-reload

## 서비스 실행
> service logstash start

## 서비스 삭제
```bath
SERVICE_NAME="logstash" \
&& systemctl stop $SERVICE_NAME \
&& systemctl disable $SERVICE_NAME \
&& rm /etc/systemd/system/$SERVICE_NAME.service \
&& systemctl daemon-reload \
&& systemctl reset-failed
```

## 명령어
### 서비스 시작
> service logstash start
### 종료
> service logstash stop
### 재시작
> service logstash restart
### 상태 확인
> service logstash status
### 로그 보기
> journalctl -u logstash -f
### 목록 보기
> systemctl list-units --type service

  
# 쉘 스크립트 서비스 등록
> 경로 : /home/ubuntu/script/start.sh
```bash
#!/bin/bash
  
nohup /home/ubuntu/app/jdk/bin/java -jar \
-Duser.timezone=Asia/Seoul \
-Dspring.profiles.active=dev \
/home/ubuntu/deploy/vote-api-deploy &

/home/ubuntu/app/nginx/sbin/nginx
```

> 경로 : /etc/systemd/system/vote-api.service
```bash
[Unit]
Description=vote-api

[Install]
WantedBy=multi-user.target

[Service]
User=root
ExecStart=/home/ubuntu/script/start.sh
Type=forking
```
