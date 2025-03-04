# for ELK stack, use Docker

#### Amazon Linux 2023에서 Docker를 설치하는 방법

> sudo dnf install -y docker

- 도커 설치 전에 패키지 업데이트
```angular2html
sudo dnf update -y
```

-  Docker 서비스 시작 및 부팅 시 자동 실행 설정
```angular2html
sudo systemctl start docker
sudo systemctl enable docker
```

-  현재 사용자(ec2-user)를 Docker 그룹에 추가
```angular2html

Docker 명령을 실행할 때 매번 sudo를 입력하지 않도록 ec2-user를 docker 그룹에 추가

     sudo usermod -aG docker $USER

적용을 위해 로그아웃 후 다시 로그인하거나 다음 명령어 실행:

    newgrp docker
```

- Docker Compose 설치 -> 문제 발생
```angular2html

> Amazon Linux 2023에서 docker compose 명령어가 동작하지 않는다면, Docker Compose v2가 포함되지 않은 경우

1. Docker Compose 최신 버전 수동 설치
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-linux-x86_64" -o /usr/local/bin/docker-compose

(2) 실행 권한 부여
sudo chmod +x /usr/local/bin/docker-compose

(3) 설치 확인
docker-compose --version
```
## ELK 트러블 슈팅

### 01. 엘라스틱 서치 권한 문제로 디렉토리 x 

> sudo chown -R 1000:1000 ./elasticsearch
> 
> sudo chmod -R 755 ./elasticsearch

### 02. 정리
 - 접속 URL: http://<EC2-Public-IP>:5601 (Kibana 대시보드)
 - 보안 그룹: 필요한 포트(9200, 5601, 6000)를 열어야 외부에서 접속 가능합니다.
 - Docker 네트워크: 내부 서비스는 Docker 네트워크(elk)를 통해 서로 연결(신경x). 외부에서는 EC2 퍼블릭 IP로 접근.
    * docker-compose down (멈추고 삭제)
    *  docker-compose up -d (클린 재시작)


### 03. 비밀번호 
- (elastic / elastic123!@#) 고정 - > 바꾸니 인증 x 

### 04. 방화벽 
- EC2 인스턴스에서 방화벽 규칙에 따라 포트가 차단될 수 있습니다. 
- iptables 또는 **firewalld**가 활성화되어 있다면, 해당 포트를 허용
```angular2html
sudo iptables -A INPUT -p tcp --dport 9200 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 5601 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 6000 -j ACCEPT
```

### 05. 기타 
- 보안 강화: AWS에서 SSL 인증서를 설정하여 Kibana 및 Elasticsearch의 보안을 강화
- 로그 데이터 용량: ELK 스택을 사용할 때 로그 데이터 용량을 관리하는 것이 중요
  * 데이터가 급증할 경우 디스크 용량 부족 문제를 예방하기 위해 엘라스틱서치 인덱스 관리를 잘 설정