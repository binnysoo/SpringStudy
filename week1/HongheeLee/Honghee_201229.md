## 섹션 3 회원 관리 예제 - 벡엔드 개발

- 비즈니스 요구사항 정리

  해당 예제에서는 실습을 위해 최소한의 요구사항으로 구현하였다. 데이터는 회원ID와 이름으로 구성된다.  회원ID는 자체적으로 가입할 때 부여되도록 하였고 사용자는 이름만 입력하면 되는데 이름은 중복되지 않도록 했다. 기능은 회원 등록과 회원 조회가 있으며 RDB, NoSQL과 같은 여러 저장소 중 어떤 데이터 저장소를 사용할지 아직 선정하지 않은 상태로 가정하였다. 이 가정때문에 인터페이스로 구현 클래스를 변경할 수 있도록 설계했고 초기 개발 단계에서는 구현객체로 메모리 기반 DB를 사용했다. 

  일반적인 웹 애플리케이션의 계층구조과 각 파트의 역할은 아래와 같다.

  컨트롤러 -> 서비스 -> 리포지토리 -> DB

  ​						도메인

  

  - 컨트롤러 : 웹 MVC의 컨트롤러 역할
  - 서비스 : 핵심 비즈니스 로직 구현
  - 리포지토리 : 데이터베이스에 접근, 도메인 객체를 DB에 저장하고 관리
  - 도메인 : 비즈니스 도메인 객체, 예) 회원, 주문, 쿠폰 등등 DB에 저장되고 관리

- 회원 도메인과 리포지토리 만들기

  회원 객체, 회원 리포지토리 인터페이스, 회원 리포지토리 메모리 구현체를 각각 작성한다. 여기서 Optional, List, @(어노테이션)에 대한 추가 공부가 필요하다. 

  - Optional : NULL이 될 수도 있는 객체를 감싸고 있는 일종의 래퍼 클래스이다. null을 직접 다루지 않아도 되고 null 체크를 직접 하지 않아도 되며 명시적으로 해당 변수가 null일 수도 있다는 가능성을 표현한다. 
  - List : 배열과 비슷한 자바의 자료형으로 배열보다 편리한 기능을 많이 가지고 있는데 특히 자료형의 개수를 미리 알 수 없을 때 List를 사용한다.  List 자료형에는 List 인터페이스를 ArrayList, LinkedList 등으로 구현한 자료형이 있다. 
  - Annotation: 어노테이션이 붙은 코드는 어노테이션이 구현된 정보에 따라 연결되는 방법이 결정된다. 이를 통해 비즈니스 로직에는 영향을 주지 않지만 해당 타겟의 연결 방법이나 소스코드의 구조를 변경할 수 있다.

- 테스트 케이스 작성

  main 메소드 통해 테스트할 수도 있지만 여러 테스트를 간단히 수행하기 위해 JUnit 프레임워크를 활용한다. 테스트 코드는 Given(주어진 것), When(구체적 상황), Then(기대하는 결과)의 로직으로 구성된다.

  테스트는 순서에 의존관계 없이 각각 독립적으로 실행되어야 한다. 이를 위해 @AfterEach, @BeforeEach를 활용할 수 있다. 테스트 이후에는 그 결과값을 지우고 테스트 전에는 새로운 객체를 생성하면서. 

---



## 섹션 4 스프링 빈과 의존관계

생성자에 @Autowired가 있으면 스프링이 연관된 객체를 스프링 컨테이너에서 찾아서 넣어준다. 이렇게 객체 의존관계를 외부에서 넣어주는 것을 DI(Dependency Injection)이라고 한다. 

스프링 빈을 등록하는 2가지 방법 : 컴포넌트 스캔과 자동 의존관계 설정,  자바 코드로 직접 스프링 빈 등록

1. 컴포넌트 스캔 

   - @Component 애노테이션이 있으면 스프링 빈으로 자동 등록된다. @Controller, @Service, @Repository에는 @Component가 포함되어 있다. 
   - 스프링은 스프링 컨테이너에 스프링 빈을 등록할 때 기본적으로 싱글톤으로 등록한다. 따라서 같은 스프링 빈이면 모두 같은 인스턴스이다.

2. 자바 코드로 직접 스프링 빈 등록

   - 설정 파일을 @Configuration과 @Bean을 넣어서 만들어준다. DI에는 필드 주입, setter 주입, 생성자 주입 이렇게 3가지 방법이 있지만 생성자 주입을 주로 사용한다. 그래서 생성자 위에 @Bean.

   ---

   

## 섹션 5 회원 관리 예제 - 웹 MVC 개발

여기서 구현하는 회원 웹 기능은 홈 화면, 등록, 조회이다.

홈 컨트롤러와 회원 관리용 홈 HTML을 추가하면 컨트롤러가 정적파일 보다 우선 순위가 높기 때문에 처음 접속했을 때 static/index.html welcome page가 아니라 홈 화면이 표시된다.

등록 기능에서는 POST 방식을 활용한다. url 창에다가 치는 건 보통 GET 방식 보통 데이터를 조회할 떄 사용. POST는 데이터를 form에 넣어서 전달하는 방식인데 데이터를 등록할 때 보통 사용.

조회 기능에서 타임리프가 본격적으로 작동. 이 탬플릿 엔진의 역할은 모델 안에 있는 값을 꺼내는 것이다. 여기서는 루프를 돌면서 객체를 하나씩 꺼내서 출력.  그 결과가 랜더링 되어서 브라우저는 그걸 뿌려줌. 데이터가 메모리에 있기 때문에 서버를 내렸다가 다시 켜면 데이터가 사라진다. 파일이나 DB에 저장해야함. DB에 저장하는 방식을 앞으로 학습. 

---



Optional 참고 링크: https://www.daleseo.com/java8-optional-after/

List 참고 링크: https://wikidocs.net/207

Annotation 참고 링크: http://www.nextree.co.kr/p5864/