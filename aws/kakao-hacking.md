### script 태그에서 카카오 API 키가 해킹당한 경위와 방식

```
2025-02-25 13:58:21 - INFO  - d.f.f.GlobalRequestLoggingFilter - Request from IP: 8.136.96.113 - GET /
2025-02-25 13:58:21 - INFO  - d.f.f.GlobalRequestLoggingFilter - Request from IP: 8.136.96.113 - GET /login
2025-02-25 13:58:21 - INFO  - d.f.f.GlobalRequestLoggingFilter - Request from IP: 8.136.96.113 - GET /chatbot
2025-02-25 13:58:21 - INFO  - d.f.f.GlobalRequestLoggingFilter - Request from IP: 8.136.96.113 - GET /signup
2025-02-25 13:58:21 - INFO  - d.f.f.GlobalRequestLoggingFilter - Request from IP: 8.136.96.113 - GET /js/signup.js
2025-02-25 13:58:21 - INFO  - d.f.f.GlobalRequestLoggingFilter - Request from IP: 8.136.96.113 - GET /js/kakao.js
2025-02-25 13:58:21 - INFO  - d.f.f.GlobalRequestLoggingFilter - Request from IP: 8.136.96.113 - GET /js/security.js
2025-02-25 13:58:21 - INFO  - d.f.f.GlobalRequestLoggingFilter - Request from IP: 8.136.96.113 - GET /js/chatbot.js
2025-02-25 13:58:21 - INFO  - d.f.f.GlobalRequestLoggingFilter - Request from IP: 8.136.96.113 - GET //dapi.kakao.com/v2/maps/sdk.js%3Fappkey=1fade549189e06ea6c209f5738909df0
```



#### 1. **th:src와 서버 사이드 템플릿 렌더링**
```
- `th:src="@{'//dapi.kakao.com/v2/maps/sdk.js?appkey=' + ${kakaoApiKey}}"`는 **Thymeleaf 템플릿 엔진**을 사용하여 서버 사이드 렌더링을 통해 클라이언트로 동적으로 값을 삽입하는 방식입니다.
- 이 코드에서 `${kakaoApiKey}`는 서버 측 변수로, 서버에서 템플릿을 렌더링할 때 실제 **카카오 API 키**가 삽입됩니다.
```


#### 2. **카카오 API 키의 클라이언트 측 노출**
- `th:src` 속성은 서버에서 HTML로 변환됩니다. 예를 들어 `${kakaoApiKey}`는 실제 API 키로 대체되어 클라이언트로 전달됩니다.
- 최종적으로, 렌더링된 HTML 코드에는 아래와 같은 형태의 `<script>` 태그가 포함됩니다:

> script type="text/javascript" src="//dapi.kakao.com/v2/maps/sdk.js?appkey=1fade549189e06ea6c209f5738909df0"></script>

- 브라우저의 **개발자 도구** 또는 **페이지 소스 보기**를 통해 API 키를 쉽게 확인할 수 있습니다.
- 네트워크 요청을 검토하면 `appkey` 파라미터를 포함한 URL도 노출됩니다.

#### 3. **공격자가 API 키를 획득하는 방식**

- **개발자 도구 사용**: 클라이언트 웹 페이지의 **JavaScript 코드**나 네트워크 요청 URL에서 API 키를 추출합니다.
- **Network 탭**에서 `GET` 요청을 확인하여 `appkey` 파라미터를 확인할 수 있습니다.
- **소스 코드 보기**: HTML 내 `<script>` 태그에서 직접 API 키를 확인 가능합니다.
- **악용 사례**: 공격자는 추출한 API 키를 사용하여 다음과 같은 작업을 수행할 수 있습니다:
    - API에 불법적으로 접근.
    - 지도 데이터 요청을 남용하여 서비스 쿼터 초과 유발.
    - API 키를 악성 요청에 재활용.


#### 4. **카카오 API 키 유출로 인한 보안 문제**
- **API 키 노출의 위험성**:
    - 누구나 해당 키를 사용해 API 호출이 가능.
    - **DoS(서비스 거부) 공격** 유발 가능.
    - 악성 사용자에 의해 API 남용.

#### 5. **방지 방법**
1. **API 키를 클라이언트에 노출하지 않기**:
    - **API 호출을 백엔드에서 처리**하고, 클라이언트는 백엔드에서 받은 데이터를 사용하도록 구현.
2. **프록시 서버 사용**:
    - 백엔드 서버가 카카오 API와 통신하도록 설정.
3. **환경 변수로 API 키 관리**:
    - 하드코딩을 피하고, API 키를 서버의 환경 변수로 저장.
4. **API 키 제한 설정**:
    - IP 또는 도메인 기반 접근 제한을 설정.
5. **API 호출 권한 관리**:
    - 인증 및 권한 검사를 통해 악의적 접근 차단.

#### 6. **결론**
- 카카오 API 키가 클라이언트 측에 노출되면 공격자에 의해 악용될 위험이 큽니다.
- **API 키는 서버 측에서만 안전하게 관리**되어야 하며, 클라이언트에 노출되지 않도록 구현해야 합니다.