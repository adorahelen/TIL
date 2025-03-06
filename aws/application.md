
## AWS EC2 내부 서버 보안
- `application.yml` 파일을 외부에서 접근하지 못하도록 막는 방법

### 1. **파일 시스템 권한 설정 (Linux 권한 제어)**
서버 내에서 `application.yml` 파일에 대한 접근 권한을 제한하려면 파일의 읽기/쓰기 권한을 설정합니다:

```bash
sudo chmod 600 /path/to/application.yml
```

- 이 명령어는 **소유자만 읽기 및 쓰기 권한**을 가지도록 설정합니다.
- **`chmod 600`**: 다른 사용자에게는 접근 권한을 부여하지 않음.

---

### 2. **서버 방화벽 설정**
AWS EC2에서 **방화벽(Security Groups)** 설정을 통해 외부에서 특정 IP만 서버에 접근하도록 제한합니다:
- **보안 그룹(Security Group)**:
    - **인바운드 규칙**에서 외부 접근을 제한합니다.
    - 특정 포트(예: 80, 443)에 대한 외부 접근 차단 가능.
    - 내부 네트워크(IP)에서만 접근 허용.

---

### 3. **환경 변수 사용**
`application.yml` 파일에 저장된 민감한 정보를 환경 변수로 대체하여 외부 접근 위험을 제거합니다.

#### EC2에서 환경 변수 설정
1. EC2 인스턴스에 로그인한 후, `.bashrc` 파일 편집:
   ```bash
   nano ~/.bashrc
   ```

2. 파일 끝에 환경 변수 추가:
   ```bash
   export SPRING_DATASOURCE_URL=jdbc:mysql://localhost:3306/mydb
   export SPRING_DATASOURCE_USERNAME=myuser
   export SPRING_DATASOURCE_PASSWORD=mypassword
   ```

3. `.bashrc` 파일 적용:
   ```bash
   source ~/.bashrc
   ```

4. **Spring Boot** 애플리케이션에서 환경 변수 참조:
   ```yaml
   spring:
     datasource:
       url: ${SPRING_DATASOURCE_URL}
       username: ${SPRING_DATASOURCE_USERNAME}
       password: ${SPRING_DATASOURCE_PASSWORD}
   ```

- 이 방식은 **application.yml** 파일에 민감한 정보가 저장되지 않도록 보안을 강화합니다.

---

### 4. **IAM 권한 설정 (AWS 보안)**
AWS IAM Role을 활용하여 **EC2 인스턴스**와 민감한 데이터 접근 권한을 설정합니다:
- **S3 사용 예시**:
    - `application.yml` 파일을 S3에 저장.
    - S3 버킷에 EC2 인스턴스의 접근 권한을 부여.
    - IAM 정책을 통해 접근을 제어.

---

### 5. **웹 서버 보안 설정**
웹 서버(Apache, Nginx)를 사용해 특정 파일에 대한 외부 접근을 차단합니다.

#### Apache 설정 예시:
```apache
<Files "application.yml">
    Order Allow,Deny
    Deny from all
</Files>
```

- 위 설정은 **application.yml** 파일에 대한 모든 외부 접근을 차단합니다.

---

### 6. **AWS Parameter Store 사용**
AWS Systems Manager Parameter Store를 활용해 민감한 정보를 안전하게 저장하고, 애플리케이션에서 이를 참조합니다:
- **장점**:
    - EC2 환경 내 민감한 정보 보호.
    - Spring Boot 애플리케이션에서 안전하게 정보 참조 가능.

---

### 결론
- `chmod 600`을 사용하여 **파일 시스템 권한**을 제한합니다.
- **방화벽(SG)** 및 **IAM 정책**을 사용하여 서버 접근을 제한합니다.
- 민감한 정보를 **환경 변수**로 관리하여 보안을 강화합니다.
- 필요에 따라 **AWS Parameter Store**를 활용하여 민감한 정보를 안전하게 저장합니다.

이 방법들을 통해 `application.yml` 파일에 대한 외부 접근을 안전하게 차단할 수 있습니다.

--- 

