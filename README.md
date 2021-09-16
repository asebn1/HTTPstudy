# HTTPstudy
HTTP 기본지식 공부

# 1. 인터넷 네트워크

### IP 프로토콜

- 대상 서버가 꺼져있어도 보냄.

→ TCP/UDP 로 해결

### TCP/UDP

1. TCP
    - 연결지향 : TCP 3 way handshake
    - 데이터 전달 보증
    - 순서 보장
    - 신뢰
2. UDP
    - IP 그대로 기능기 거의 같음

    → IP + PORT + 체크섬 정도

    - 순서 보장X
    - 데이터 보장X

### 한번에 둘 이상 연결? A <-> B(2개 어플리케이션)

- **포트**로 구분 : 80, 8080, 21000 등...

### DNS

- 전화번호 부
- 도메인 명 → IP로 변경

### URI

- URL + URN
- URL : Resource Locator
- URN : Resource Name

### URL

- ? 이후 key=value
- &로 추가 가능

# 2. HTTP 메서드

### 1. HTTP

1. 프로토콜
    - HTTP1, HTTP2 : TCP 기반
    - HTTP3 : UDP 기반. 성능개선
2. 특징
    1. 클라이언트 서버 구조
        - Request Response 구조
        - 클라이언트 : 서버에 요청, 응답 대기
        - 서버 : 요청에 대한 결과를 응답
    2. 무상태 프로토콜(stateless), 비연결성
        - 서버가 클라이언트 상태 보존X
        - 장점 : 서버 확장성 높음
        - 단점 : 클라이언트가 추가 데이터 전송
        - 한계 : 로그인. 실무 한계(최소한으로 사용할 것)
    3. HTTP 메시지
    4. 단순함, 확장 가능

    ### 2. HTTP 메서드 (요구사항 : 회원 정보 관리 API 만들기)

    → 방법 : 리소스(자원), 행위(메서드) 식별

    1.  리소스 식별 → **회원**. URI 계층 구조 활용
        - **회원** 목록 조회   /members
        - **회원** 조회           /members/{id}        → 어떻게 구분?    GET
        - **회원** 등록           /members        → 어떻게 구분?    POST
        - **회원** 수정           /members/{id}        → 어떻게 구분?    PATCH, PUT, POST
        - **회원** 삭제           /members/{id}        → 어떻게 구분?    DELETE
    2. 행위 (GET, POST)
        - GET : 리소스 **조회**
        - POST : 요청 데이터 처리, **등록**
        - PUT : 리소스를 대체, 해당 리소스 없으면 생성. **덮어쓰기**
        - PATCH : 리소스 부분 변경. **수정**
        - DELETE : 리소스 **삭제**

    ### HTTP 메서드의 속성

    1. 안전
        - 호출해도 리소스 변경X
    2. 멱등
        - f(f(x)) = f(x)
        - 100번 호출 = 1번 호출
        - POST는 멱등이 아님
    3. 캐시가능
        - GET, HEAD, POST, PATCH 가능
        - 실제로는 GET, HEAD 정도만 캐시로 사용

    ### 클라이언트 → 서버

    1. 쿼리 파라미터를 통한 데이터 전송
        - GET
        - 주로 정렬 필터
    2. 메시지 바디를 통한 데이터 전송
        - POST, PUT, PATCH

    ### HTML FORM 사용시 : GET/POST 만 사용가능

    → 컨트롤 URI로 해결(동사 추가) : /new, /edit, /delete

    1. 목록
    - GET :  /members
    1. 등록
    - GET : /members/new
    - POST : /members/new, /members
    1. 조회
    - GET :  /members/{id}
    1. 수정
    - GET : /members/{id}/edit
    - POST :  /members/{id}/edit, /members/{id}
    1. 삭제
    - POST : /members/{id}/delete

    # 3. HTTP 상태코드

    : 클라이언트가 보낸 요청의 처리상태를 응답에서 알려주는 기능

    - 1xx(Informational) : 요청이 수신되어 **처리 중**

        → 거의 사용X

    - 2xx(Successful) : 요청 **정상 처리**
    - 3xx(Redirection) : 요청 완료하려면 **추가 행동 필요**
        - 영구 리다이렉션(301, 308) : 리소스의 URI가 영구적으로 이동 ( /event → /new-event)
        - 일시 리다이렉션(302, 303, 307) : 리소스의 URI가 일시적으로 변경
        - 기타 리다이렉션
            - 304 : 캐시 목적(응답에 메시지 바디 포함X)
    - 4xx(Client Error) : **클라이언트 오류**
    - 5xx(Server Error) : **서버 오류**

    # 4. HTTP 헤더 - 일반 헤더

    ### 과거

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/371f41ce-f88f-4239-9c23-c35118d54e09/Untitled.png)

    - 엔티티(Entity) → 표현(Representation)
    - 표현 = 표현 메타데이터 + 표현데이터

    ### 1. 표현 헤더(표현 데이터)

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/22516bc6-e549-4665-adab-945da0d7b304/Untitled.png)

    : 전송, 응답 둘다 사용

    - Content-Type : 표현 데이터의 형식 (html, xml, json... 등)
        - text/html; charset=utf-8
        - application/json
        - image/png
    - Content-Encoding : 표현 데이터의 압축 방식
        - gzip
        - deflate
        - identity
    - Content-Language : 표현 데이터의 자연언어
    - Content-Length : 표현 데이터의 길이

    ### 2. 협상 (콘텐츠 네고시에이션)

    : 클라이언트가 선호하는 표현 요청

    → 클라이언트가 서버에 요청하는것임

    - Accept : 클라이언트가 선호하는 미디어 타입 전달
    - Accept-Charset : 클라이언트가 선호하는 문자 인코딩
    - Accept-Encoding : 클라이언트가 선호하는 압축 인코딩
    - Accept-Language : 클라이언트가 선호하는 자연언어
        - en, ko...

    ### 3. 전송

    - 단순전송 : Content-Length
    - 압축전송 : Content-Encoding
    - 분할전송 : Transfer-Encoding,   [ Content-Length없음 ]
    - 범위전송 : Content-Range

    ### 4. 일반정보

    - From : 유저 에이전트의 이메일 정보
    - Referer : 이전 웹 페이지 주소
        - 유입 경로 분선
        - 요청에서 사용
    - User-Agent : 유저 에이전트 애플리케이션 정보
    - Server : 요청을 처리하는 오리진 서버의 소프트웨어 정보
    - Date : 메시지가 생성된 날짜

    ### 5. 특별한 정보

    - **Host(필수)** : 요청한 호스트 정보
    - Location : 페이지 리다이렉션
    - Allow : 허용 가능한 HTTP 메서드
    - Retry-After : 유저 에이전트가 다음 요청을 하기까지 기다리는 시간

    ### 6. 쿠키

    → HTTP는 Stateless임. 해결방법(매번 정보를 서버에 넘겨야하는 단점)

    - Set-Cookie : 서버 → 클라이언트로 쿠키 **전달(응답)**
    - Cookie : 클라이언트가 서버에서 받은 쿠기 **저장**, HTTP 요청시 서버로 전달
    1. 주 사용처
        - 사용자 로그인 세션 관리
        - 광고 정보 트래킹
    2. 쿠키 정보는 항상 서버에 전송?
        - 네트워크 트래픽 추가 유발
        - 최소한의 정보만 사용 → 세션id, 인증 토큰
    3. 주의
        - 민감 데이터 저장X

# 5. HTTP 헤더 - 캐시와 조건부 요청

### 방법1) 검증 헤더와 조건부 요청 (Last-Modified)

- 캐시 2020년 11월 10일 10:00:00 vs 서버 2020년 11월 10일 10:00:00 **비교 → 같으면 변경X**
- 캐시 유효 시간 초과해도, 서버의 데이터 갱신되지 않으면? → 304 Not Modified + 헤더 메타정보만. (body 보내지않음)
- 클라이언트는 캐시의 데이터 재활용

### 단점

- 1초 미만 단위로 캐시 조정 불가능
- 날짜 기반의 로직

### 방법2) 검증 헤더와 조건부 요청(ETag)

- ETag(Entity Tag)
- ETag만 보내서 같으면 유지, 다르면 받기

### 1. 캐시 제어 헤더

- **Cache-Control : 캐시 제어**
- Pragma : 캐시 제어(하위호환)
- Expires : 캐시 유효 기간(하위호환)

### 2. 캐시 무효화

- Cache-Control: no-cache, no-store, must-revalidate
