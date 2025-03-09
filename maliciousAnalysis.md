

## 악성코드 분석 방법 (1주차)
![img_2.png](img/hycu/malware1week.png)

- 악성코드[Malware] : 악의적인 행동이나 목적을 위해 작성된 실행 가능한 코드

![img.png](img/hycu/malware1week1.png)
![img_1.png](img/hycu/malware1week2.png)

- *악성코드/바이러스 <- 1세대부터 5세대 등으로 나뉘어짐

![img_2.png](img/hycu/malware1week3.png)

## 악성코드 분석 방법 (2주차)

#### 프로세스 [초기분석 -> 동적분석 -> 정적분석:소스코드분석]
![img.png](img/hycu/malware2week1.png)

### 핵심개념
- 안티바이러스 스캐닝 : 기존에 분석된 악성코드인지 아닌지 확인하는 방법
  * 바이러스 토탈(웹사이트)
> VirusTotal Public API (https://www.virustotal.com/ko/documentation/public-api/)

- 해시 함수의 사용 : 항상 같은 출력이 나오기 때문에 입력 메시지에 대한 불변적 증거 값
  * 즉 메시지의 오류나 변조를 탐지할 수 있는 무결성을 제공해 줌
  * 원본 파일의 해시 값과 <-> 위조된 파일의 해시 값을 비교(무결성 깨져있음)
  * => 포렌식 영역, 증거 수집이나 이동 후에 변조 여부를 확인 가능

![img_1.png](img/hycu/malware2week2.png)

![img_3.png](img/hycu/malware2week3.png)


- 문자열 검색 (Strings.exe, BinText)
  * 실행파일에서 문자열을 찾아 추출함
  * GUI 환경에서 분석할 수 있는 바이너리 도구


- PE 포맷(Portable Executable)
  * 윈도우 운영 체제에서 사용되는 파일 형식 
![img_6.png](img/hycu/malicious0308.png)

- 패킹 == 실행파일의 크기를 줄인다.
  * 패킹 : PE 파일의 크기를 줄이거나 내부 코드와 리소스를 감추기 위해 사용하는 기술 
  * 대표적인 예 : UPX, ASPack, Upack, PESpin 등
  * ++ 현재 백신 프로그램은 이러한 패킹 파일을 전부 악성코드라 탐지함

![img.png](img/hycu/malicious0309v3.png)

![img_1.png](img/hycu/malicious0309v4.png)

