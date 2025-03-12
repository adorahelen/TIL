```
2025-03-12 00:41:05 - WARN  - d.f.f.GlobalRequestLoggingFilter - 
🚨 Potential attack attempt from IP: 193.41.206.246 - 
GET /home/&lt;USER&gt;/.aws/config - Query: null - Port: 8080 - Protocol: http

🧐 이게 뭐야?
	•	/home/<USER>/.aws/config 경로는 AWS CLI 설정 파일이 있는 경로예요.
	•	공격자가 AWS 인증 정보(Access Key, Secret Key 등) 를 빼내려고 하는 시도입니다.
	
🎯 공격 의도
	•	만약 웹 서버가 디렉토리 리스팅(directory listing)이 열려 있다면, 공격자가 AWS 자격 증명 파일을 직접 가져갈 수도 있어요.
	•	취약한 웹 서버(예: 잘못된 파일 경로 노출)가 설정되어 있다면 Cloud Credential Leak(클라우드 인증 정보 유출) 공격이 될 수 있음.



2025-03-12 00:34:32 - WARN  - d.f.f.GlobalRequestLoggingFilter -
 🚨 Potential attack attempt from IP: 193.41.206.246 -
  GET /jmx-console/HtmlAdaptor - Query: action=inspectMBean&amp;name=jboss.system:type=ServerConfig - Port: 8080 - Protocol: http

🧐 이게 뭐야?
	•	jmx-console/HtmlAdaptor는 JBoss 서버의 JMX 콘솔 관리 페이지
	•	JBoss는 Java EE 기반 애플리케이션 서버인데, HtmlAdaptor를 통해 서버의 설정을 변경할 수 있음.

🎯 공격 의도
	•	만약 jmx-console이 열려 있다면, 공격자는 서버 설정 변경, 원격 코드 실행(RCE), 시스템 명령 실행 같은 작업을 수행할 수도 있음.
	•	과거 CVE(취약점) 중에서 인증 없이 JMX Console에 접근할 수 있는 취약점이 있었음.
	•	action=inspectMBean&name=jboss.system:type=ServerConfig → 이건 서버 설정 정보를 확인하려는 시도


```