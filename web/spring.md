# Spring
- [IoC](https://github.com/yurak-choi/tech-interview/blob/master/web/spring.md#ioc)
  - IoC Container
  - IoC의 구현
- [Spring MVC 순서](https://github.com/yurak-choi/tech-interview/blob/master/web/spring.md#spring-mvc-순서)
- [Mybatis와 JPA](https://github.com/yurak-choi/tech-interview/blob/master/web/spring.md#mybatis와-jpa)
  - Mybatis
  - JPA(ORM)
___

## IoC
**IoC(Inversion of Control)**란 제어의 역전, 즉 제어권이 역전되었다는 뜻이다.
객체의 생성부터 관리까지 모든 객체에 대한 제어권이 개발자가 아닌 외부 컨테이너에게 넘어간 것을 IoC라고 한다.

### IoC Container
객체를 생성하고 객체간의 의존성을 이어주는 역할을 한다.
- **BeanFactory**  
BeanFactory 인터페이스는 IoC컨테이터의 기능을 정의하고 있는 인터페이스이며, Bean의 생성 및 의존성 주입, 생명주기 관리 등의 기능을 제공한다.
여기서 Bean이란 IoC컨테이너에 의해 생성되고 관리되는 객체를 의미한다.
- **ApplicationContext**  
BeanFactory 인터페이스를 상속받는 ApplicationContext는 BeanFactory가 제공하는 기능 외에 AOP, 이벤트 처리 등의 기능을 제공한다.
모든 ApplicationContext 구현체는 BeanFactory의 기능을 모두 제공하므로 특별한 경우를 제외하고는 ApplicationContext를 사용하는 것이 바람직하다.

### IoC의 구현
- **DL(Dependency Lookup)**  
컨테이너에서 제공하는 API를 이용해 사용하고자 하는 빈을 Lookup하는 것을 말한다.
- **DI(Dependency Injection)**  
넓은 의미로는 의존하는 객체를 직접 생성하지 않고 전달받는 방식을 말한다. 객체를 외부에서 생성하여 전달함으로써 변경에 대한 유연함을 가질 수 있다.  
IoC에서의 DI는 Bean 설정 정보를 바탕으로 컨테이너가 클래스 사이의 의존 관계를 자동으로 연결해주는 것을 말한다. DL 사용시 컨테이너 종속성이 증가하므로 이를 줄이기 위해 DI를 사용한다.
  - Setter Injection
  - Constructor Injection
  - Method Injection

___

## Spring MVC 순서
1. 클라이언트로부터 받은 요청을 DispatcherServlet이 HandlerMapping에게 넘긴다.
2. HandlerMapping은 이에 맞는 컨트롤러명을 반환한다.
3. DispatcherServlet은 해당 컨트롤러에게 클라이언트로부터 받은 데이터를 넘긴다.
4. 컨트롤러는 비즈니스 로직을 수행한 후 Model과 View를 ModelAndView 객체로 DispatcherServlet에게 넘겨준다.
5. DispatcherServlet은 이에 맞는 View를 ViewResolver에 요청한다.
6. ViewResolver에게 받은 View에게 Model을 넘겨 출력한다.

___

## Mybatis와 JPA
### Mybatis
JDBC를 좀더 편하게 사용할 수 있도록 SQL이나 저장 프로시저를 객체와 연결시켜주는 Persistence Framework  
  
**특징**
- 코드와 SQL의 분리
- SQL을 직접 다루므로 복잡한 쿼리 작성 가능

### JPA(ORM)
- ORM(Object Relational Mapping) : 데이터베이스 테이블과 자바 객체 사이의 매핑을 처리해주는 기술
- JPA(Java Persistence API) : 자바 진영의 ORM 표준 기술

**특징**
- CRUD와 같은 간단한 쿼리는 자동으로 처리
- SQL이 아닌 객체 중심 개발
- 복잡한 쿼리 작성의 어려움
  - JPQL, 순수 SQL, Mybatis 혼용 사용

___
