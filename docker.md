# 도커 (Docker)

도커(Docker)는 애플리케이션을 **컨테이너(Container)**라는 가상화된 환경에서 실행할 수 있도록 도와주는 플랫폼입니다. 도커는 애플리케이션과 그 실행에 필요한 모든 것을 하나의 패키지로 묶어 컨테이너로 배포합니다. 이를 통해 개발, 테스트, 배포 환경 간의 차이로 인해 발생하는 문제를 최소화할 수 있습니다.

## 주요 개념

1. **컨테이너(Container)**
    - 애플리케이션과 그 실행에 필요한 라이브러리, 설정 파일 등을 포함하는 독립적인 실행 환경입니다.
    - 경량화된 가상화 기술로, 하나의 OS 커널을 공유하기 때문에 성능 오버헤드가 적습니다.

2. **이미지(Image)**
    - 컨테이너를 생성하기 위한 템플릿입니다.
    - 애플리케이션, 설정, 의존성을 포함한 불변의 파일 시스템 스냅샷입니다.
    - 도커 허브(Docker Hub)에서 공개 이미지를 가져오거나 직접 빌드할 수 있습니다.

3. **도커 파일(Dockerfile)**
    - 이미지를 빌드하기 위한 설정 파일입니다.
    - 애플리케이션을 실행하기 위한 명령어, 환경 설정 등이 포함되어 있습니다.

4. **도커 엔진(Docker Engine)**
    - 도커의 핵심으로, 컨테이너를 생성, 실행, 관리하는 역할을 합니다.

5. **도커 허브(Docker Hub)**
    - 도커 이미지를 공유하는 클라우드 기반 저장소입니다.

## 도커의 장점

1. **이식성(Portability)**
    - 어디서나 동일한 환경에서 실행 가능(개발 환경과 배포 환경의 차이 제거).

2. **경량성(Lightweight)**
    - 기존의 가상 머신(VM)보다 자원을 적게 사용하며 빠르게 실행.

3. **효율성(Efficiency)**
    - 애플리케이션 배포와 확장, 테스트 과정이 단순화.

4. **확장성(Scalability)**
    - 여러 컨테이너를 손쉽게 배포하고 관리 가능.

## 도커의 사용 사례

1. **개발 환경 구축**
    - 동일한 환경을 여러 개발자에게 제공.

2. **테스트 자동화**
    - 테스트 환경을 빠르게 설정하고 제거.

3. **마이크로서비스 아키텍처**
    - 각 서비스별로 독립적인 컨테이너를 실행.

4. **CI/CD 파이프라인**
    - 애플리케이션의 지속적인 배포와 통합.

도커는 특히 클라우드 기반의 애플리케이션 배포와 마이크로서비스 환경에서 강력한 도구로 자리 잡고 있습니다.

## 도커에서 자주 사용되는 주요 명령어

1. **도커 버전 및 정보 확인**
    - `docker --version` : 도커 버전 확인.
    - `docker info` : 도커 엔진 정보 및 설정 확인.

2. **이미지 관련 명령어**
    - `docker images` : 로컬에 저장된 도커 이미지 목록 확인.
    - `docker pull <이미지 이름>` : 도커 허브에서 이미지를 다운로드.
    - `docker build -t <이미지 이름> .` : Dockerfile을 기반으로 이미지를 빌드.
    - `docker rmi <이미지 ID>` : 특정 이미지를 삭제.

3. **컨테이너 관련 명령어**
    - `docker ps` : 실행 중인 컨테이너 목록 확인.
    - `docker ps -a` : 모든 컨테이너(중지된 컨테이너 포함) 목록 확인.
    - `docker run <이미지 이름>` : 컨테이너 실행.
    - `docker run -d <이미지 이름>` : 백그라운드에서 컨테이너 실행.
    - `docker run -it <이미지 이름> /bin/bash` : 컨테이너에 접속하여 터미널 실행.
    - `docker stop <컨테이너 ID>` : 실행 중인 컨테이너 중지.
    - `docker start <컨테이너 ID>` : 중지된 컨테이너 다시 실행.
    - `docker rm <컨테이너 ID>` : 컨테이너 삭제.
    - `docker logs <컨테이너 ID>` : 특정 컨테이너의 로그 확인.

4. **네트워크 관련 명령어**
    - `docker network ls` : 도커 네트워크 목록 확인.
    - `docker network create <네트워크 이름>` : 새로운 네트워크 생성.
    - `docker network connect <네트워크 이름> <컨테이너 이름>` : 특정 컨테이너를 네트워크에 연결.

5. **볼륨 관련 명령어**
    - `docker volume ls` : 도커 볼륨 목록 확인.
    - `docker volume create <볼륨 이름>` : 새로운 볼륨 생성.
    - `docker volume rm <볼륨 이름>` : 특정 볼륨 삭제.

6. **컨테이너 관리 및 디버깅**
    - `docker exec -it <컨테이너 ID> /bin/bash` : 실행 중인 컨테이너에 접속하여 명령 실행.
    - `docker inspect <컨테이너 ID>` : 컨테이너의 상세 정보 확인.
    - `docker stats` : 실행 중인 컨테이너의 리소스 사용량 확인.

7. **도커 컴포즈(Docker Compose)**
    - `docker-compose up` : Docker Compose 파일을 기반으로 모든 서비스 시작.
    - `docker-compose down` : 실행 중인 모든 Compose 서비스 중지 및 삭제.
    - `docker-compose ps` : Compose로 실행된 서비스 상태 확인.

## 도커의 핵심 개념

### 도커 컴포즈 (Docker Compose)
도커 컴포즈는 여러 컨테이너를 정의하고 관리하기 위한 도구입니다.
- 사용 목적: 복잡한 애플리케이션(예: 마이크로서비스)에서 여러 컨테이너를 하나의 파일로 정의하고 실행.
- 구성 파일: `docker-compose.yml` 파일을 사용해 컨테이너의 네트워크, 볼륨, 이미지 등을 정의.

```yaml
version: '3'
services:
  web:
    image: nginx
    ports:
      - "8080:80"
  db:
    image: mysql
    environment:
      MYSQL_ROOT_PASSWORD: example

	•	docker-compose up 명령으로 정의된 모든 서비스를 시작.
	•	docker-compose down 명령으로 모든 서비스를 종료 및 삭제.

볼륨(Volume)

도커 컨테이너에서 데이터를 영구적으로 저장하거나 공유할 수 있도록 하는 메커니즘입니다.
	•	특징:
	•	컨테이너가 삭제되어도 데이터는 유지.
	•	여러 컨테이너 간 데이터 공유 가능.
	•	볼륨 사용 예시:

docker run -v /my-data:/data ubuntu

네트워크(Network)

도커는 컨테이너 간 통신을 관리하기 위해 네트워크 기능을 제공합니다.
	•	종류:
	1.	Bridge: 기본 네트워크. 같은 호스트 내의 컨테이너를 연결.
	2.	Host: 컨테이너가 호스트 네트워크를 직접 사용.
	3.	None: 네트워크가 없는 상태로 실행.
	4.	Custom Network: 사용자 정의 네트워크로 더 복잡한 연결을 지원.

도커 파일(Dockerfile)

이미지를 생성하기 위한 설정 파일로, 컨테이너 실행 환경을 정의.
	•	예시:

FROM ubuntu:latest
RUN apt-get update && apt-get install -y python3
COPY app.py /app/app.py
CMD ["python3", "/app/app.py"]

	•	FROM: 기반 이미지.
	•	RUN: 명령 실행.
	•	COPY: 파일 복사.
	•	CMD: 컨테이너 실행 시 기본으로 실행할 명령.

레지스트리(Registry)

이미지를 저장하고 관리하는 중앙 저장소.
	•	공개 레지스트리: Docker Hub (기본 레지스트리).
	•	사설 레지스트리: 기업 내에서 자체적으로 운영.

오케스트레이션 도구

대규모 컨테이너 관리를 위한 도구.
	•	쿠버네티스(Kubernetes): 컨테이너를 클러스터 환경에서 배포 및 관리.
	•	도커 스웜(Docker Swarm): 도커에서 제공하는 내장 오케스트레이션 도구.

컨텍스트(Context)

도커 CLI에서 다양한 환경(로컬, 원격 서버 등)을 쉽게 전환할 수 있도록 설정.
	•	사용 예시:

docker context create my-remote --docker "host=ssh://user@remote-server"
docker context use my-remote

시크릿(Secret)

도커 컨테이너에서 민감한 데이터를 안전하게 관리하기 위한 기능.
	•	예시:
	•	비밀번호, API 키 등을 컨테이너에서 직접 코드로 노출하지 않도록 관리.

도커 생태계의 확장성

도커는 단순히 컨테이너 관리 도구를 넘어, 복잡한 애플리케이션 환경을 효율적으로 구성하고 관리할 수 있는 강력한 생태계를 제공합니다. Compose, Network, Volume, Registry 등의 개념을 조합하면 다양한 사용 사례에 대응할 수 있습니다.

