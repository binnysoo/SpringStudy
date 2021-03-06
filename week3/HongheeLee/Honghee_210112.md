## HTTP 메서드

### HTTP API 만들기

좋은 URI 설계? 가장 중요한 것은 **리소스 식별**

그런데 그렇게 설계하면 리소스가 같지만 다른 기능을 가지는 URI를 어떻게 구분?

리소스와 해당 리소스를 대상으로 하는 행위를 분리. 

URI는 리소스만 식별하고 행위는 HTTP 메서드로 구분. 

실무에서 어쩔 수 없이 행위를 URI에 담을 수도 있긴 하다 -> 컨트롤 URI ex) /start-delivery 

### HTTP 메서드

**GET** : 리소스 조회. 서버에 전달하고 싶은 데이터는 query를 통해서 전달. 

​		 메시지 전달 -> 서버 도착 -> 응답 데이터

**POST** : 요청 데이터 처리. **메시지 바디**를 통해 서버로 요청 데이터 전달. 서버는 요청 데이터를 처리.

​			 메시지 전달 -> 신규 리소스 생성 -> 응답 데이터

​			리소스 URI에 POST 요청이 오면 요청 데이터를 어떻게 처리할지 리소스마다 따로 정해야한다.

​			용도 : 새 리소스 생성(등록), 요청 데이터 처리(단순 생성 이외에도 프로세스 처리), 다른 메서드 애매한 경우

**PUT** : 리소스 대체, 없으면 생성. 클라이언트가 리소스 위치를 알고 URI 지정한다는 것이 POST와 차이.

​			리소스를 수정하거나 부분변경이 아니라 리소스를 완전히 대체한다는 점 주의! 

**PATCH** : 리소스 부분 변경. 

**DELETE** : 리소스 제거

### HTTP 메서드의 속성

##### 안전

호출해도 리소스를 변경하지 않는다.  GET, HEAD. 

##### 멱등(Idempotent)

한 번 호출하든 두 번 호출하든 100번 호출하든 결과가 똑같다. 

활용 : 자동 복구 메커니즘. 서버가 정상 응답을 못주었을 때 클라이언트가 같은 요청을 다시 해도 되는가? 판단 근거

멱등 메서드 : GET, PUT, DELETE | POST는 멱등이 아니다. 

##### 캐시가능

응답 결과 리소스를 **캐시**해서 사용해도 되는가?

GET, HEAD, POST, PATCH 캐시 가능하지만 실제로 **GET, HEAD** 정도만 캐시로 사용. 

## HTTP 메서드 활용

### 클라이언트에서 서버로 데이터 전송

데이터 전달 방식은 크게 2가지

쿼리 파라미터를 통한 데이터 전송 : GET , ex) 정렬 필터(검색어)

메시지 바디를 통한 데이터 전송 : POST, PUT, PATCH, ex) 회원가입, 상품 주문 등

크게 4가지 상황

- 정적 데이터 조회 : 쿼리 파라미터 없이 리소스 경로로 조회(GET), 이미지나 정적 텍스트 문서

- 동적 데이터 조회 : 쿼리 파라미터 사용. 주로 검색, 게시판 목록에서 정렬필터(검색어)

- HTML Form을 통한 데이터 전송 : POST 전송-저장(메시지 바디에 url encoding 처리해서 전달), GET 전송-조회

- HTTP API를 통한 데이터 전송 : HTML Form을 사용하지 않는 모든 상황. ex) 서버 to 서버(백엔드 시스템 통신), 앱 클라이언트, 웹 클라이언트. 

  POST, PUT, PATCH : 메시지 바디를 통해 데이터 전송. GET : 쿼리 파라미터로 데이터 전달해 조회. 데이터 전달할 때는 JSON 주로 사용.

### HTTP API 설계 예시

크게 3가지 예시

1. HTTP API - 컬렉션 : POST 기반 등록(회원 관리 시스템)

   회원 목록 /members -> GET | 회원 등록 /members -> POST | 회원 조회 /members/{id} -> GET |회원 수정 /members/{id} -> PATCH(PUT, POST) | 회원 삭제 /members/{id} -> DELETE

   클라이언트는 등록될 리소스의 URI를 모른다.  새로 등록된 리소스 URI를 생성하는 건 서버!

   컬렉션 : 서버가 관리하는 리소스 디렉토리. 대부분 이걸로 쓴다.

2. HTTP API - 스토어 : PUT 기반 등록(파일 관리 시스템)

   파일 목록 /files -> GET | 파일 조회 /files/{filename} -> GET | 파일 등록 /files/{filename} -> PUT
   | 파일 삭제 /files/{filename} -> DELETE| 파일 대량 등록 /files -> POST

   클라이언트가 리소스 URI를 알고 있어야 하고 직접 리소스 URI를 지정한다. 

   스토어 : 클라이언트가 관리하는 리소스 저장소. 가끔 사용.

3. HTML FORM 사용 

   회원 목록 /members -> GET | 회원 등록 폼 /members/new -> GET | 회원 등록 /members/new, /members -> POST  | 회원 조회 /members/{id} -> GET | 회원 수정 폼 /members/{id}/edit -> GET |  회원 수정 /members/{id}/edit, /members/{id} -> POST | 회원 삭제 /members/{id}/delete -> POST 

   GET, POST만 지원하기 때문에 제약이 있다. 

   이 때문에 동사로 된 리소스 경로, 컨트롤 URI를 사용한다. ex ) /edit, /new, /delete

참고하면 좋은 URI 설계 개념 : 문서 / **컬렉션** / 스토어 / 컨트롤러, 컨트롤 URI
