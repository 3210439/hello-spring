# hello-spring
해당 프로젝트는 
스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술
https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8
강의 내용을 바탕으로 진행된 프로젝트입니다. 


이번 프로젝트를 진행하며 주요했던 개념들을 정리하겠습니다.

컨트롤러에서 리턴 값으로 문자를 반환하면 뷰 리졸버(viewResolver)가 화면을 찾아서 처리한다.
 * 스프링부트 템플릿엔진 기본 viewName 매핑
 * resources: templates/+{ViewName}+.html
 
 정적 컨텐츠
 정적 컨텐츠는 static 폴더안에 존재하는 파일들을 말합니다.
 첫번 째로, 웹 브라우저에서 어떤 요청을 할시 해당 url로 매핑되는 컨트롤러가 있는지 확인합니다.
 만약에 없다면 resources/static 경로에서 해당 url과 일치하는 파일 명이 존재하는지 확인합니다.
 존재한다면 해당 파일을 리턴합니다.
 정적 컨텐츠는 말그대로 정적이기 때문에 html 파일 그래도 리턴됩니다.
 
 
@ResponseBody를 사용하면 뷰 리졸버(viewRsolver)를 사용하지 않음
대신에 HTTP의 BODY에 문자 내용을 직접 반환합니다.

@ResponseBody를 사용
- HTTP의 BODY에 문자 내용을 직접 반환
- viewResolver 대신에 HttpMessageConverter가 동작
- 기본 문자처리: StringHttpMessageConverter
- 기본 객체처리: MappingJackson2HttpMessageConverter
- byte 처리 등등 기타 여러 HttpMessageConverter가 기본으로 등록되어 있음

테스트케이스와 JUNIT의 필요성
 개발 기능을 실행해서 테스트 할 때 자바의 main 메서드를 통해서 실행하거나, 웹 애플리케이션의 컨트롤러를 통해서 해당 기능을
 실행한다. 이러한 방법은 준비하고 실행하는데 오래 걸리고, 반복 실행하기 어렵고 여러 테스트를 한번에 실행하기 어렵다는
 단점이 있다. 자바는 JUnit이라는 프레임워크로 테스트를 실행해서 이러한 문제르 해결한다.
 
 @AfterEach: 
 - 한번에 여러 테스트를 실행하면 메모리 DB에 직전 테스트의 결과가 남을 수 있다. 이렇게 되면 다음 이전 테스트 때문에
 다음 테스트가 실패할 가능성이 있다. @AfterEach를 사용하면 각 테스트가 종료될 때 마다 이 기능을 실행한다. 여기서는
 메모리 DB에 저장된 데이터를 삭제한다.
 - 테스트는 각각 독립적으로 실행되어야 한다. 테스트 순서에 의존관계가 있는 것은 좋은 테스트가 아니다.
 
 @BeforeEach: 각 테스트 실행 전에 호출된다. 테스트가 서로 영향이 없도록 항상 새로운 객체를 생성하고,
 의존관계도 새로 맺어준다.
 
 DI(Dependency Injection) 
 - 객체 의존관계를 외부에서 넣어주는 것을 DI, 의존성 주입이라 한다. 
 
 생성자에 @AutoWired가 있으면 스프링이 연관된 객체를 스프링 커테이너에서 찾아서 넣어준다. 이렇게 객체 의존관계를
 외부에서 넣어주는 것을 DI(Dependency Injection), 의존성 주입이라 한다.
 
 스프링 빈을 등록하는 2가지 방법
 - 컴포넌트 스캔과 자동 의존관계 설정
 - 자바 코드로 직접 스프링 빈 등록하기
 
 컴포넌트 스캔 원리
 @Component 애노테이션이 있으면 스프링 빈으로 자동 등록된다.
 @Controller 컨트롤러가 스프링 빈으로 자동 등록되 이윧 컴포넌트 스캔 때문이다.
 
 @Component를 포함하는 다음 애노테이션도 스프링 빈으로 자동 등록된다.
 @Controller
 @Service
 @Repository
 
 생성자에 @Autowired를 사용하면 객체 생성 시점에 스프링 컨테이너에서 해당 스프링 빈을 찾아서 주입한다. 생성자가 1개만 있으면
 @Autowired는 생략할 수 있다.
 
 스프링은 스프링 컨테이너에 스프링 빈을 등록할 때, 기본으로 싱글톤으로 등록한다.
 
 DI에는 필드 주입, setter 주입, 생성자 주입 이렇게 3가지 방법이 있다.
 
 컨트롤러가 정적 파일보다 우선순위가 높다.
 
 H2 데이터베이스
 개발이나 테스트 용도로 가볍고 편리한 DB, 웹 화면 제공
 
 DataSource는 데이터베이스 커넥션을 획들할 때 사용하는 객체다. 스프링 부트는 데이터베이스 커넥션 정보를 바탕으로
 Datasource를 생성하고 스프링 빈으로 만들어둔다. 그래서 DI를 받을 수 있다.
 
 개방-폐쇄 원칙(OCP, Open-Closed Principle)
 - 확장에는 열려있고, 수정, 변경에는 닫혀있다.
 스프링의 DI(Dependency Injection)을 사용하면 기존 코드를 전혀 손대지 않고, 설정만으로 구현 클래스를 변경할 수 있다.
 
 @SpringBootTest: 스프링 컨테이너와 테스트를 함께 실행한다.
 @Transactional: 테스트 케이스에 이 애노테이션이 있으면, 테스트 시작 전에 트랜잭션을 시작하고,
 테스트 완료 후에 항상 롤백한다. 이렇게 하면 DB에 데이터가 남지 않으므로 다음 테스트에 영향을 주지 않는다.
 
 스프링 JdbcTemplate
 - 순수 Jdbc와 동일한 환경설정을 하면 된다.
 - 스프링 JdbcTemplate과 MyBatis 같은 라이브러리는 JDBC API에서 본 반복 코드를 대부분 제거해준다. 하지만 SQL은
 직접 작성해야 한다.
 
 JPA
 JPA는 기존의 반복 코드는 물론이고, 기본적인 SQL도 JPA가 직접 만들어서 실행해준다.
 JPA를 사용하면, SQL과 데이터 중심의 설계에서 객체 중심의 설계로 패러다임을 전환할 수 있다.
 JPA를 사용하면 개발 생산서을 크게 높일 수 있다.
 
 JPA 라이브러리
 implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
 spring-boot-starter-data-jpa는 내부에 jdbc 관련 라이브러리를 포함한다.
 
 
 스프링 부트에 JPA설정 추가 방법
 spring.jpa.show-sql=true : JPA가 생성하는 SQL을 출력한다. 
 spring.jpa.hibernate.ddl-auto=none : JPA는 테이블을 자동으로 생성하는 기능을 제공하는데 none를 사용하면 해당 기능을 끈다.
 create를 사용하면 엔티티 정보를 바탕으로 테이블도 직접 생성해준다.
 
 @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
 private Long id;
 
 id - 값 자동 증가 사용시 위와 같은 어노테이션을 활용한다.
 
 JPA를 통한 모든 데이터 변경은 트랜잭션 안에서 실행해야 한다.
 
 AOP: Aspect Oriented Programming
 공통 관심 사항(cross-cutting concern) vs 핵심 관심 사항(core concern) 
 
 
 
