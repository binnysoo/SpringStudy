<h1>스프링 입문 - 2일차(스프링 빈과 의존관계 ~ 예제 - 웹 MVC 개발) </h1>
오늘 다 들으려다가 h2 설정하고 확인하는 과정에서 계속 에러가 나서 일단 섹션5까지 만 정리

## Section4 스프링 빈과 의존관계

1. 스프링 빈? Spring loC 컨테이너가 관리하는 자바 객체를 빈이라는 용어로 부름.

```
컨테이너에 빈을 등록하는 방법(의존관게를 설정하는 방법)은 크게 두가지로 나뉘어진다.
어노테이션(@Controller, @Service)를 명시하는 것을 통해 컴포넌트 스캔을 통한 자동 
의존관계 설정과 자바코드로 @Bean, @Autowired를 이용해 일일히 직접 의존관게를 주입시켜주는 방법이다.
```

2. 의존관계 주입? 서로 다른 두 클래스가 어떤 의존관계에 있는지 명시시켜 주는 행위

```
DI(Dependency injection)에는 필드 주입(필드에 직접 작성), setter 주입(public으로 노출되는 단점)
, 생성자 주입 세가지 방법이 있으나 생성자 주입을 대부분 이용한다.
```

## Section 5 회원 관리 예제 - 웹 MVC 개발

앞서 배운 내용을 토대로 회원 등록, 회원 조회 기능을 구현 했다. 이과정에서 html의 메소드(Get,Post)에
대한 복습이 필요하다는 걸 느꼈다. 코드 작성만 잘따라가면 되는 섹션이었음.

