# RDS

- EC2 , RDS 둘다 IP 접든 제한 걸어 둘 수 있음
- 특정 iP 가 아니면 접근 할 수 없도록 

## MySQL 데이터베이스 목록

```sql
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| fossilfuel         |
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
5 rows in set (0.02 sec)
```

### 데이터베이스 목록 해석

- **fossilfuel**:
    - 이 데이터베이스는 사용자에 의해 생성된 사용자 정의 데이터베이스입니다. 여기에는 'fossilfuel'과 관련된 애플리케이션 데이터가 저장되어 있을 가능성이 높습니다.

- **information_schema**:
    - `information_schema`는 MySQL에서 시스템 메타데이터를 저장하는 시스템 데이터베이스입니다. 데이터베이스, 테이블, 열(칼럼), 권한, 사용자 및 기타 객체에 대한 정보를 포함합니다. 이는 주로 데이터베이스 구조 정보를 확인하는 데 사용됩니다.

- **mysql**:
    - `mysql` 데이터베이스는 MySQL 서버에서 계정, 권한, 설정 정보 등을 저장하는 시스템 데이터베이스입니다. 사용자와 권한 관리, 서버 설정 등의 정보를 포함합니다. MySQL 서버의 중요한 내부 데이터베이스입니다.

- **performance_schema**:
    - `performance_schema`는 성능 관련 메타데이터와 실행 정보를 저장하는 시스템 데이터베이스입니다. 성능 최적화 및 분석을 위해 주로 사용됩니다. 쿼리 성능, 잠금 정보, 스레드 상태 등을 모니터링할 수 있습니다.

- **sys**:
    - `sys` 데이터베이스는 `performance_schema`와 `information_schema`의 데이터를 더 쉽게 읽을 수 있는 형태로 제공하는 뷰와 함수를 포함하는 데이터베이스입니다. 이는 데이터베이스 관리자 및 개발자가 시스템 성능과 상태를 더 쉽게 모니터링하고 분석할 수 있도록 돕습니다.

### 요약
- `fossilfuel`: 사용자 정의 애플리케이션 데이터베이스.
- `information_schema`: 메타데이터와 시스템 정보.
- `mysql`: 계정, 권한 및 설정 정보.
- `performance_schema`: 성능 관련 정보.
- `sys`: `performance_schema`와 `information_schema`의 데이터를 더 쉽게 접근할 수 있는 뷰와 함수.

## 조작 

- show databases;
- USE fossilfuel2;
- SHOW TABLES;
  SELECT * FROM members;



### 1. `fossilfuel2` 데이터베이스 생성

먼저 MySQL 클라이언트에 접속한 후, `fossilfuel2` 데이터베이스를 생성합니다:
```sql
CREATE DATABASE fossilfuel2;
```

### 2. `fossilfuel2` 데이터베이스에 접속

생성한 데이터베이스에 접속하려면 다음 명령어를 사용합니다:
```sql
USE fossilfuel2;
```

### 3. 데이터베이스 안에서 사용할 수 있는 다양한 MySQL 문법

#### 테이블 생성
```sql
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) NOT NULL,
    email VARCHAR(100),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

#### 데이터 삽입
```sql
INSERT INTO users (username, email) VALUES ('john_doe', 'john@example.com');
INSERT INTO users (username, email) VALUES ('jane_doe', 'jane@example.com');
```

#### 데이터 조회
- 모든 데이터 조회
  ```sql
  SELECT * FROM users;
  ```

- 조건에 맞는 데이터 조회
  ```sql
  SELECT * FROM users WHERE username = 'john_doe';
  ```

#### 데이터 업데이트
```sql
UPDATE users SET email = 'john_doe_new@example.com' WHERE username = 'john_doe';
```

#### 데이터 삭제
```sql
DELETE FROM users WHERE username = 'jane_doe';
```

#### 테이블 구조 확인
```sql
DESCRIBE users;
```

#### 인덱스 생성
```sql
CREATE INDEX idx_username ON users (username);
```

#### 데이터 정렬
```sql
SELECT * FROM users ORDER BY created_at DESC;
```

#### 테이블 삭제
```sql
DROP TABLE users;
```

