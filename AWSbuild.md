# AWS - git clone build

## `./gradlew build` 명령어를 실행하려면 필요한 파일들

Gradle 프로젝트에 다음 파일들이 필요합니다. 이 파일들은 Gradle Wrapper를 사용하여 프로젝트를 빌드하고 의존성을 관리하는 데 필수적입니다.

### 필수 파일들

1. **gradlew**:
    - Unix 기반 시스템에서 사용하는 Gradle Wrapper 실행 파일입니다.
    - 권한을 설정하여 실행 가능하도록 합니다: `chmod +x gradlew`

2. **gradlew.bat**:
    - Windows 시스템에서 사용하는 Gradle Wrapper 실행 파일입니다.

3. **gradle-wrapper.jar**:
    - Gradle Wrapper의 실제 JAR 파일로, `gradlew`와 `gradlew.bat` 스크립트가 이 파일을 실행하여 Gradle을 다운로드하고 실행합니다.
    - 위치: `gradle/wrapper/gradle-wrapper.jar`

4. **gradle-wrapper.properties**:
    - Gradle Wrapper의 설정 파일로, 사용할 Gradle 버전 및 배포 URL을 지정합니다.
    - 위치: `gradle/wrapper/gradle-wrapper.properties`

5. **build.gradle**:
    - Gradle 빌드 파일로, 프로젝트의 빌드 설정, 의존성, 플러그인 등을 정의합니다.

6. **settings.gradle**:
    - Gradle 설정 파일로, 멀티모듈 프로젝트에서 사용할 수 있으며, 포함된 프로젝트들을 정의합니다.

### 디렉토리 구조 예시
```plaintext
fossilfuel/
├── build.gradle
├── gradlew
├── gradlew.bat
├── gradle/
│   └── wrapper/
│       ├── gradle-wrapper.jar
│       └── gradle-wrapper.properties
├── settings.gradle
└── src/
    ├── main/
    │   ├── java/
    │   └── resources/
    └── test/
        └── java/
```

### 예시 파일 내용

**gradle/wrapper/gradle-wrapper.properties**:
```properties
distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists
distributionUrl=https\://services.gradle.org/distributions/gradle-6.7-bin.zip
```

**build.gradle**:
```groovy
plugins {
    id 'java'
}

group 'com.example'
version '1.0-SNAPSHOT'

repositories {
    mavenCentral()
}

dependencies {
    implementation 'org.springframework.boot:spring-boot-starter'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
}
```

**settings.gradle**:
```groovy
rootProject.name = 'fossilfuel'
```

---

## AWS에서 GitHub 리포지토리를 클론하고 빌드하기

AWS에서 `git clone`을 통해 프로젝트를 받아와서 빌드하려면 기본적으로 다음과 같은 파일들이 GitHub에 올라가 있어야 합니다:

  * gradle-wrapper.properties: gitignore 확장자 제한으로 안올라갈 수도 있음

### 필수 파일 및 디렉토리

- **소스 코드 디렉토리**:
    - `src/main/java/`와 같은 소스 코드 파일이 위치한 디렉토리
    - `src/main/resources/`와 같은 리소스 파일이 위치한 디렉토리

- **빌드 파일**:
    - `build.gradle`: Gradle 빌드 파일
    - `settings.gradle`: Gradle 설정 파일

- **Gradle Wrapper**:
    - `gradlew`: Unix 기반 시스템에서 사용하는 Gradle Wrapper 실행 파일
    - `gradlew.bat`: Windows 시스템에서 사용하는 Gradle Wrapper 실행 파일
    - `gradle/wrapper/gradle-wrapper.jar`: Gradle Wrapper JAR 파일
    - `gradle/wrapper/gradle-wrapper.properties`: Gradle Wrapper 속성 파일

- **설정 파일**:
    - `.gitignore`: Git에 의해 무시될 파일 및 디렉토리 정의
    - `.gitattributes`: Git 속성을 정의하는 파일

--- 

