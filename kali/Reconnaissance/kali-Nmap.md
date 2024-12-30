# **Nmap 결과 분석**

`fossilfuel.site`(AWS EC2 인스턴스)에 대해 Nmap 스캔을 실행한 결과를 정리하고 분석했습니다.

---

## **1. 기본 스캔** 
(`nmap fossilfuel.site`)

```plaintext
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http

분석
	•	22/tcp (SSH):
	•	원격 접속용 SSH 서비스가 실행 중입니다.
	•	보안 권장 사항:
	•	신뢰할 수 있는 IP로만 접근 제한.
	•	키 기반 인증 사용.
	•	80/tcp (HTTP):
	•	HTTP 서비스가 실행 중입니다.
	•	fossilfuel.site에서 웹 애플리케이션이 실행되고 있음을 나타냅니다.

2. 서비스 버전 확인 (nmap -sV fossilfuel.site)

PORT     STATE SERVICE    VERSION
22/tcp   open  tcpwrapped
80/tcp   open  tcpwrapped
5060/tcp open  tcpwrapped

분석
	•	tcpwrapped:
	•	연결은 가능하지만 Nmap이 서비스 세부 정보를 탐지하지 못했습니다.
	•	원인:
	•	방화벽, 접근 제어 리스트(ACL), 또는 보안 설정으로 인해 탐지가 차단되었을 가능성.
	•	해결 방안:
	•	다른 스캔 옵션을 추가하거나 접근 권한 조정.

3. 운영 체제 탐지 실패 (nmap -O fossilfuel.site)

TCP/IP fingerprinting (for OS scan) requires root privileges.
QUITTING!

분석
	•	운영 체제 탐지는 root 권한이 필요합니다.
	•	해결 방안:
	•	sudo 명령어를 사용하여 실행:

sudo nmap -O fossilfuel.site

4. 종합 스캔 (nmap -A fossilfuel.site)

PORT     STATE SERVICE     VERSION
22/tcp   open  ssh         OpenSSH 8.7 (protocol 2.0)
80/tcp   open  nagios-nsca Nagios NSCA
5060/tcp open  sip?

분석
	1.	22/tcp (SSH):
	•	OpenSSH 8.7 실행 중 (SSH 프로토콜 2.0 지원).
	•	보안 권장 사항:
	•	SSH 포트를 비표준 포트로 변경(예: 22022).
	•	키 기반 인증 및 IP 화이트리스트 설정.
	2.	80/tcp (HTTP):
	•	서비스: Nagios NSCA.
	•	시스템 모니터링 및 알림 전송 서비스.
	•	문제:
	•	웹페이지 한글 깨짐 현상(문자 인코딩 오류).
	•	해결 방안:
	•	웹 애플리케이션에서 올바른 문자 인코딩 설정 확인.
	3.	5060/tcp (SIP):
	•	SIP(Session Initiation Protocol) 관련 포트로 추정.
	•	보안 권장 사항:
	•	사용하지 않을 경우 해당 포트를 닫거나 방화벽에서 차단.

5. OS 탐지 결과 (sudo nmap -O fossilfuel.site)

결과 요약
	1.	열려 있는 포트:
	•	22/tcp (SSH), 80/tcp (HTTP), 5060/tcp (SIP).
	2.	추정되는 OS:
	•	가장 높은 확률(97%)로 리눅스 기반 네트워크 장비 또는 오래된 Windows 시스템으로 감지.
	•	AWS EC2 인스턴스임을 고려하면, Linux 배포판(예: Ubuntu, Amazon Linux)이 실행 중일 가능성이 높음.
	3.	오류:
	•	“Warning: OSScan results may be unreliable…” 메시지 포함.
	•	이유: 탐지 정확성을 위해 최소 1개의 “열린 포트”와 “닫힌 포트”가 필요하지만 조건이 충족되지 않음.

해결 방안
	•	SSH로 서버에 접속하여 실제 운영 체제를 확인:

ssh -i your_key.pem ec2-user@fossilfuel.site
uname -a

보안 권장 사항
	1.	포트 관리:
	•	22번 포트:
	•	SSH 포트를 비표준 포트로 변경(예: 22022).
	•	AWS 보안 그룹에서 특정 IP만 접근 허용.
	•	80번 포트:
	•	HTTP를 HTTPS로 전환하여 통신 보안 강화(SSL/TLS 설정).
	•	5060번 포트:
	•	SIP 서비스가 필요하지 않다면 포트를 닫거나 방화벽에서 차단.
	2.	웹 애플리케이션:
	•	문자 인코딩 문제 해결(UTF-8 설정).
	3.	Nmap 추가 분석:
	•	조건 개선 후 다시 OS 탐지 수행:

sudo nmap -O --osscan-guess fossilfuel.site

결론
	•	Nmap 스캔 결과를 통해 서버의 포트 상태와 서비스 동작을 확인하였습니다.
	•	보안을 강화하고 웹 애플리케이션의 문제를 해결하여 공격 노출을 최소화해야 합니다.

