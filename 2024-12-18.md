# kali-linux

## DNS 정보 수집
### $ dnsenum

┌──(kali㉿kali)-[~]
└─$ dnsenum fossilfuel.site
dnsenum VERSION:1.3.1

-----   fossilfuel.site   -----


Host's addresses:
__________________

fossilfuel.site.                         5        IN    A        3.37.171.152


Name Servers:
______________

ns-40.awsdns-05.com.                     5        IN    A        205.251.192.40
ns-802.awsdns-36.net.                    5        IN    A        205.251.195.34
ns-1453.awsdns-53.org.                   5        IN    A        205.251.197.173
ns-1740.awsdns-25.co.uk.                 5        IN    A        205.251.198.204


### $ Fierce 

┌──(kali㉿kali)-[~]
└─$ fierce --domain fossilfuel.site       
NS: ns-802.awsdns-36.net. ns-1740.awsdns-25.co.uk. ns-40.awsdns-05.com. ns-1453.awsdns-53.org.
SOA: ns-1740.awsdns-25.co.uk. (205.251.198.204)
Zone: failure
Wildcard: failure



### $ dimitry

┌──(kali㉿kali)-[~]
└─$ dmitry -iwnse fossilfuel.site.
Deepmagic Information Gathering Tool
"There be some deep magic going on"

HostIP:3.37.171.152
HostName:fossilfuel.site.

Gathered Inet-whois information for 3.37.171.152

- => whois 정보 수집

inetnum:        3.0.0.0 - 4.255.255.255
netname:        NON-RIPE-NCC-MANAGED-ADDRESS-BLOCK
descr:          IPv4 address block not managed by the RIPE NCC

- => INET-WHOIS 정보 (AWS, IP 풀), 그 외 다수 결과

# Result
세 가지 도구(`dnsenum`, `fierce`, `dmitry`)는 모두 DNS 및 네트워크 정보를 수집하지만, 사용 목적과 제공하는 정보가 다릅니다. 아래에 주요 차이점과 각 도구가 어떤 정보를 제공했는지 요약합니다:

---

### 1. **`dnsenum`**
#### 특징
- DNS 레코드(예: A, NS, MX)와 같은 기본 DNS 정보를 수집하는 데 사용됩니다.
- 서브도메인 및 관련된 IP 주소 정보를 제공합니다.

#### 결과 요약
- **도메인:** `fossilfuel.site`
- **A 레코드:**
    - `3.37.171.152` (도메인의 IP 주소)
- **NS 레코드:**
    - 네임 서버 목록 및 IP 주소:
        - `ns-40.awsdns-05.com` → `205.251.192.40`
        - `ns-802.awsdns-36.net` → `205.251.195.34`
        - `ns-1453.awsdns-53.org` → `205.251.197.173`
        - `ns-1740.awsdns-25.co.uk` → `205.251.198.204`

---

### 2. **`fierce`**
#### 특징
- 네트워크 스캐닝 도구로, 주로 DNS 정보와 영역 전송(Zone Transfer) 취약점을 탐색합니다.
- 와일드카드 도메인 설정 및 영역 전송 실패 여부도 탐지합니다.

#### 결과 요약
- **NS 레코드:**
    - 동일한 네임 서버 목록과 IP 주소가 나열됨 (`dnsenum`과 일치).
- **SOA(SOA 레코드):**
    - `ns-1740.awsdns-25.co.uk` (주 네임 서버).  
      IP 주소: `205.251.198.204`.
- **Zone Transfer:** 실패 (네임 서버가 영역 전송을 허용하지 않음).
- **Wildcard DNS:** 실패 (와일드카드 도메인 설정 없음).

---

### 3. **`dmitry`**
#### 특징
- DNS 및 WHOIS 정보를 포함한 다양한 정보 수집이 가능합니다.
- 추가적으로 WHOIS 정보를 통해 네트워크 소유자와 관리 기관 정보를 제공합니다.

#### 결과 요약
- **도메인:** `fossilfuel.site`
- **IP 주소:** `3.37.171.152`
- **WHOIS 정보:**
    - IP 주소가 IANA에 의해 관리되며, **AWS의 IP 풀(3.0.0.0/8)**에 속함.
    - WHOIS에서 RIPE DB(유럽 네트워크 관리 기관)를 통해 결과 조회됨.
    - **IANA (Internet Assigned Numbers Authority)**가 주요 관리 기관.
- **추가 정보:**
    - 서브도메인 또는 이메일 정보는 발견되지 않음.

---

### **차이점 요약**
| **도구**         | **주요 기능**                             | **결과**                                                                                  |
|-------------------|-------------------------------------------|------------------------------------------------------------------------------------------|
| `dnsenum`         | DNS 레코드와 IP 수집                      | A, NS 레코드, 네임 서버 목록, IP 제공.                                                   |
| `fierce`          | DNS 및 Zone Transfer 취약점 탐지          | SOA 레코드 확인, 영역 전송 및 와일드카드 도메인 테스트.                                    |
| `dmitry`          | WHOIS 및 다양한 네트워크 정보 수집         | IP WHOIS 정보, 관리 기관 확인, 추가 도메인/이메일 정보 없음.                               |

---

### **결론**
- `dnsenum`과 `fierce`는 DNS 정보 수집에 초점이 맞춰져 있지만, `fierce`는 보안 테스트(Zone Transfer 및 Wildcard)도 포함합니다.
- `dmitry`는 WHOIS 및 네트워크 관련 정보를 추가로 제공하여 보다 광범위한 정보를 탐지합니다.
- 세 도구 모두 결과가 보완적이므로 목적에 따라 함께 사용하는 것이 효과적입니다.


