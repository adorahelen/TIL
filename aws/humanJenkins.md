
---

# 셸 스크립트, deploy

## 1. 셸 스크립트 파일 만들기
1. 터미널을 열고, 스크립트 파일을 생성합니다. 파일 이름은 `deploy.sh`로 설정합니다.
   ```bash
   nano deploy.sh
   ```
2. 아래 코드를 스크립트 파일에 입력한 후 저장합니다:
   ```bash
   #!/bin/bash

   # 환경 변수 설정
   PROJECT_DIR="/home/ec2-user/FossilFuel"
   JAR_NAME="fossilfuel-0.0.2-SNAPSHOT.jar"
   JAR_PATH="$PROJECT_DIR/build/libs/$JAR_NAME"

   # 1. 현재 실행 중인 JAR 파일 종료
   echo "현재 실행 중인 JAR 파일 종료 중..."
   PID=$(ps aux | grep "$JAR_NAME" | grep -v grep | awk '{print $2}')
   if [ ! -z "$PID" ]; then
       kill -9 $PID
       echo "PID $PID 종료 완료."
   else
       echo "실행 중인 JAR 파일이 없습니다."
   fi

   # 2. 클린 빌드
   echo "Gradle 클린 빌드 중..."
   cd $PROJECT_DIR
   ./gradlew clean

   # 3. 최신 버전으로 Git pull
   echo "Git pull 실행 중..."
   git pull origin main

   # 4. 빌드 후 JAR 파일 다시 실행 (백그라운드에서 실행)
   echo "빌드 중..."
   ./gradlew build -x test

   echo "빌드 완료. JAR 파일 실행 중..."
   java -jar $JAR_PATH &

   echo "작업 완료."
   ```
3. 저장 및 종료:
    - **Ctrl + X**를 누르고 **Y**를 입력한 후, **Enter**를 눌러 저장 후 종료합니다.

---

## 2. 스크립트에 실행 권한 부여
스크립트 파일에 실행 권한을 부여합니다:
```bash
chmod +x deploy.sh
```

---

## 3. 스크립트 실행
스크립트를 실행합니다:
```bash
./deploy.sh
```

---

## 4. 스크립트 실행 결과 확인
스크립트 실행 중 다음 작업들이 진행됩니다:
1. **실행 중인 JAR 파일 종료**:
    - `ps` 명령을 사용하여 실행 중인 `fossilfuel-0.0.2-SNAPSHOT.jar` 파일을 찾아 종료.
2. **Gradle 클린 빌드**:
    - `./gradlew clean` 명령을 통해 기존 빌드를 제거.
3. **Git pull**:
    - Git 리포지토리에서 최신 코드를 가져옴.
4. **빌드 후 JAR 파일 다시 실행**:
    - 빌드 완료 후 JAR 파일을 백그라운드에서 실행.

---

## 스크립트 실행 후
- 실행 성공 시:
    - **"작업 완료."** 메시지가 출력됩니다.
    - 빌드 및 실행된 JAR 파일은 서버에서 백그라운드로 동작을 시작합니다.
- **로그**:
    - JAR 실행 중 생성된 로그는 별도로 설정된 위치에서 확인 가능.

---

