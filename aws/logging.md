
# 1.Logback
## TimeBasedRollingPolicy
* 새로운 날짜가 되었을 때 새로운 로그 파일이 생성되고,
* 기존의 fossilfuel-web.log 파일이 이름이 변경(회전)되는 방식으로 동작
---

# 2.SCP
- ✅ logs 디렉토리 전체를 복사하는 명령어
> scp -i ~/AWS/aws-study.pem -r ec2-user@3.37.171.152:/home/ec2-user/FossilFuel/build/libs/logs ~/Downloads/

- 📌 참고: logs 디렉토리 내부 파일만 복사하고 싶다면?
> scp -i ~/AWS/aws-study.pem -r ec2-user@3.37.171.152:/home/ec2-user/FossilFuel/build/libs/logs/* ~/Downloads/


---
# 3.타임존 설정(for Log)


## 로그 시간을 한국 시간대로 설정하는 방법
- 3번만 진행해도 전부 해결 (EC2 서버 시스템 시간대 조정)

Spring Boot 애플리케이션의 로그 시간대를 한국 시간대로 강제 설정하는 방법을 설명합니다. 이를 위해 `application.yml`, `logback-spring.xml` 파일 수정, 또는 서버 시스템 시간대 변경을 활용할 수 있습니다.

---

### 1. **`application.yml` 파일 수정**
`application.yml`에서 로그 패턴에 타임존을 명시적으로 지정하여 로그 시간을 조정합니다.

```yaml
spring:
  application:
    name: fossilfuel

  logging:
    level:
      root: INFO
      dcu.fossilfuel: INFO
    pattern:
      console: "%d{yyyy-MM-dd HH:mm:ss.SSS,Asia/Seoul} - %-5level - %logger{36} - %msg%n"
      file: "%d{yyyy-MM-dd HH:mm:ss.SSS,Asia/Seoul} - %-5level - %logger{36} - %msg%n"
```

- **설명**:
    - `%d{yyyy-MM-dd HH:mm:ss.SSS,Asia/Seoul}`: 로그 시간에 **Asia/Seoul** 타임존을 적용.
    - `console`과 `file` 출력 모두 한국 시간대를 기준으로 설정.

---

### 2. **`logback-spring.xml` 파일 수정**
로그 설정을 `logback-spring.xml`에서 타임존을 명시적으로 설정합니다.

```xml
<configuration>

    <!-- 웹 서비스 로그 파일 저장 -->
    <appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <file>logs/fossilfuel-web.log</file>
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <fileNamePattern>logs/fossilfuel-web-%d{yyyy-MM-dd,Asia/Seoul}.log</fileNamePattern>
            <maxHistory>30</maxHistory>
        </rollingPolicy>
        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS,Asia/Seoul} - %-5level - %logger{36} - %msg%n</pattern>
        </encoder>
    </appender>

    <!-- 보안 및 해킹 탐지 로그 -->
    <appender name="JSON_FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <file>logs/fossilfuel-security.json</file>
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <fileNamePattern>logs/fossilfuel-security-%d{yyyy-MM-dd,Asia/Seoul}.json</fileNamePattern>
            <maxHistory>30</maxHistory>
        </rollingPolicy>
        <encoder class="net.logstash.logback.encoder.LogstashEncoder">
            <customFields>{"appName": "fossilfuel", "environment": "production"}</customFields>
        </encoder>
    </appender>

    <!-- 콘솔 출력 설정 -->
    <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS,Asia/Seoul} - %-5level - %logger{36} - %msg%n</pattern>
        </encoder>
    </appender>

    <!-- 로그 레벨 설정 -->
    <root level="info">
        <appender-ref ref="FILE"/>
        <appender-ref ref="JSON_FILE"/>
        <appender-ref ref="CONSOLE"/>
    </root>

</configuration>
```

- **설명**:
    - 모든 로그 출력(Appender)에서 `%d{yyyy-MM-dd HH:mm:ss.SSS,Asia/Seoul}`를 사용해 한국 시간대 적용.
    - 파일(`FILE`), JSON(`JSON_FILE`), 콘솔(`CONSOLE`) 출력 설정을 모두 포함.

---

### 3. **EC2 서버의 시스템 시간대 변경**
EC2 서버의 시간대가 기본적으로 UTC로 설정되어 있는 경우, 한국 시간대로 변경할 수 있습니다.

```bash
sudo timedatectl set-timezone Asia/Seoul
```

- **설명**:
    - EC2 서버의 시스템 시간대를 **Asia/Seoul**로 설정.
    - 모든 애플리케이션 및 시스템 로그에서 한국 시간대로 동작.

---

### 추천 설정 순서
1. **애플리케이션 단**에서 `application.yml` 또는 `logback-spring.xml`을 통해 로그 타임존 설정.
2. 필요 시 **서버의 시스템 시간대**를 변경.

---

확인방법
---

아래는 요청하신 내용을 마크다운 형식으로 정리한 것입니다:

---

## EC2 서버에서 시간대 설정 확인하기

EC2 서버의 시간대가 제대로 설정되었는지 확인하려면 아래 명령어를 실행합니다:

```bash
timedatectl
```

### 출력 예시

```plaintext
               Local time: Thu 2025-03-07 12:34:56 KST
           Universal time: Thu 2025-03-07 03:34:56 UTC
                 RTC time: Thu 2025-03-07 03:34:56
                Time zone: Asia/Seoul (KST, +0900)
```

### 설명
- **Local time**: 서버의 현지 시간 (한국 시간 기준).
- **Universal time**: 세계 표준시 (UTC).
- **RTC time**: 하드웨어(RTC) 클록 시간.
- **Time zone**: 서버의 시간대 설정 (위 경우, `Asia/Seoul`로 설정됨).

---

**Time zone**이 `Asia/Seoul`로 설정되어 있다면, 서버의 시간대가 한국 시간대로 올바르게 설정된 것입니다.

--- 

- timedatectl set-timezone 명령어로 시간대를 변경하면 시스템의 시간대가 즉시 적용됩니다. 재시작할 필요는 없습니다.
- 서버를 재시작해도 시간대 설정은 유지됩니다. timedatectl set-timezone 명령으로 변경된 시간대 설정은 시스템에 영구적으로 적용되며, 재부팅 후에도 계속 유지됩니다.
