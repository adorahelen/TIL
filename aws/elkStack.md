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


