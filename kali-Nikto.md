# **Nikto 사용법**

## **1. 기본 명령어**

```bash
nikto -h fossilfuel.site

2. 주의 사항
	1.	본인 소유 서버에만 실행
	•	Nikto 스캔은 공격적인 방식으로 웹 서버에 다수의 요청을 보냅니다.
	•	본인이 소유하지 않은 서버에 실행하면 법적 문제가 발생할 수 있습니다.
	2.	AWS 보안 설정
	•	AWS 보안 그룹이 Nikto 요청을 차단하지 않는지 확인하세요.
	•	필요하면 IP를 허용 리스트에 추가하세요.
	3.	성능 영향
	•	스캔 중 서버의 성능이 일시적으로 저하될 수 있으니, 트래픽이 적은 시간에 실행하는 것이 좋습니다.

3. 스캔 결과

┌──(kali㉿kali)-[~]
└─$ nikto -h fossilfuel.site

- Nikto v2.5.0

---

- Target IP: 3.37.171.152
- Target Hostname: fossilfuel.site
- Target Port: 80
- Start Time: 2024-12-16 03:22:55 (GMT-5)

---

- Server: No banner retrieved
- /: The anti-clickjacking X-Frame-Options header is not present. See: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options
- /: The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type. See: https://www.netsparker.com/web-vulnerability-scanner/vulnerabilities/missing-content-type-header/
- /BAkaqgiq.sqlite: Uncommon header 'content-disposition' found, with contents: inline;filename=f.txt.
- No CGI Directories found (use '-C all' to force check all possible dirs)
- OPTIONS: Allowed HTTP Methods: GET, HEAD, POST, PUT, DELETE, OPTIONS .
- HTTP method ('Allow' Header): 'PUT' method could allow clients to save files on the web server.
- HTTP method ('Allow' Header): 'DELETE' may allow clients to remove files on the web server.
- 7934 requests: 0 error(s) and 6 item(s) reported on remote host
- End Time: 2024-12-16 03:24:35 (GMT-5) (100 seconds)

---

- 1 host(s) tested

4. Nikto 결과 해석 및 조치 방안

1. Anti-Clickjacking Header 누락
	•	문제
X-Frame-Options 헤더가 누락되었습니다. 이 헤더는 클릭재킹(Clickjacking) 공격을 방지하기 위해 사용됩니다.
	•	위험성
공격자가 악성 사이트에 현재 사이트를 <iframe>으로 삽입하여 사용자를 속일 가능성이 있습니다.
	•	조치
웹 서버 설정에서 X-Frame-Options 헤더를 추가합니다:

X-Frame-Options: SAMEORIGIN

	•	Apache
Header always append X-Frame-Options SAMEORIGIN
	•	Nginx
add_header X-Frame-Options "SAMEORIGIN";

2. X-Content-Type-Options Header 누락
	•	문제
X-Content-Type-Options 헤더가 설정되지 않았습니다. 이 헤더는 브라우저가 파일의 MIME 타입을 잘못 추측하는 것을 방지합니다.
	•	위험성
MIME 스니핑 공격에 노출될 수 있습니다.
	•	조치
웹 서버 설정에서 X-Content-Type-Options 헤더를 추가합니다:

X-Content-Type-Options: nosniff

	•	Apache
Header set X-Content-Type-Options "nosniff"
	•	Nginx
add_header X-Content-Type-Options "nosniff";

3. /BAkaqgiq.sqlite: 파일 노출
	•	문제
SQLite 파일(BAkaqgiq.sqlite)이 노출되었습니다.
	•	위험성
이 파일에 민감한 데이터가 포함되어 있을 수 있습니다. 공격자는 이를 다운로드하고 분석할 가능성이 있습니다.
	•	조치
민감한 파일은 공개 디렉토리에서 제거하거나 접근을 차단해야 합니다.
디렉토리 접근을 막기 위해 .htaccess를 사용하거나, Nginx/Apache에서 파일 유형별 접근을 제한합니다:

<FilesMatch "\.(sqlite|db)$">
  Require all denied
</FilesMatch>

4. 허용된 HTTP 메소드
	•	문제
HTTP 메소드로 PUT, DELETE가 활성화되어 있습니다.
	•	PUT: 클라이언트가 파일을 업로드할 수 있음.
	•	DELETE: 클라이언트가 파일을 삭제할 수 있음.
	•	위험성
인증되지 않은 사용자에게 악용될 경우 심각한 보안 위협이 될 수 있음.
	•	조치
불필요한 HTTP 메소드를 비활성화합니다.
	•	Apache

<LimitExcept GET POST>
    Require all denied
</LimitExcept>


	•	Nginx

if ($request_method !~ ^(GET|POST|HEAD)$ ) {
    return 405;
}

5. HTTP 메소드 테스트

Nikto는 OPTIONS 메소드를 통해 지원되는 HTTP 메소드를 확인합니다. 이러한 정보는 공격자가 잠재적으로 시스템을 악용할 단서를 제공할 수 있으므로, 필요하지 않은 메소드는 제거하는 것이 중요합니다.

6. 요약
	•	보안 헤더 추가
	•	X-Frame-Options
	•	X-Content-Type-Options
	•	민감한 파일 보호
	•	BAkaqgiq.sqlite와 같은 민감한 파일을 서버에서 제거하거나 접근 제한.
	•	HTTP 메소드 제한
	•	PUT, DELETE 메소드를 비활성화하여 불필요한 기능 제거.
	•	추가 점검
	•	웹 서버 설정을 조정한 뒤 다시 Nikto를 실행하여 취약점 수정 여부를 확인하세요.

Nikto가 감지한 내용은 기본적인 웹 서버 보안 설정에 집중되어 있으니, 이러한 문제를 해결하는 것이 서버 보안을 강화하는 첫 걸음이 됩니다.

