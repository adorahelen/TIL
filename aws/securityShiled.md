### 1. 웹 셸 탐지 (Spring Boot 환경)

- Spring Boot에서는 정적 파일이 src/main/resources/static/에 위치하는 경우가 많음 (웹 셸도 비슷함)

> find /home/ec2-user/FossilFuel/src/main/resources/static/ -type f -exec grep -EI 'system\(|shell_exec\(|passthru\(|eval\(|base64_decode\(' {} \; -print

> find /home/ec2-user/FossilFuel/ -type f -exec grep -EI 'system\(|shell_exec\(|passthru\(|eval\(|base64_decode\(' {} \; -print

### 2. 악성코드 탐지 (JAR 파일 검사)
- Spring Boot 프로젝트는 실행 가능한 JAR 파일을 생성하는데, build/libs/에 위치한 JAR 파일이 변조되었을 가능성이 있기에, JAR 내부 파일을 확인
```
mkdir /tmp/jar_scan
unzip /home/ec2-user/FossilFuel/build/libs/fossilfuel-0.0.2-SNAPSHOT.jar -d /tmp/jar_scan
grep -r "Runtime.getRuntime()" /tmp/jar_scan
```

- 만약 Runtime.getRuntime().exec() 같은 코드가 있다면, 공격자가 원격 명령 실행을 시도할 수 있는 코드일 가능성이 있음

```
find /tmp/jar_scan -type f -name "*.class"
find /tmp/jar_scan -type f -name "*.jsp"
find /tmp/jar_scan -type f -name "*.php"
```

 
- etc 바이너리 무결성 검사 : rpm -Va
  * 출력 결과에서 5나 T 등의 값이 나오면 파일이 변조되었을 가능성이 있음

### 3. 실행 중인 프로세스 확인 (비정상적인 Java 프로세스 탐색)
> ps aux | grep java

- 이외에도 최근에 추가된 계정 확인 

### 4. 네트워크 연결 확인 (열려있는 포트 확인)
- 웹 셸이나 악성코드가 원격 서버와 통신하는지 확인하려면 네트워크 연결을 체크
```
netstat -tulnp

일반적인 Spring Boot 애플리케이션이라면 8080이나 443 같은 포트를 사용할 텐데, 만약 예상치 못한 포트가 열려 있다면 의심
```
- 현재 통신 중인 IP 확인
> netstat -antp | grep ESTABLISHED
  * 만약 알 수 없는 IP와 연결되어 있다면, 공격자가 C2 서버와 통신 중

### 5. etc 
- 최근 변경된 파일 확인 : find /var/www/ -type f -mtime -5
- 시스템 로그 분석
  * 웹 서버 로그 분석, 시스템 인증 로그 확인, 크론탭 확인(정기적 자동실행)
