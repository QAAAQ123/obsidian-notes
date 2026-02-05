- 스프링 컨테이너는 내부에 존재하는 빈의 생명주기를 관리하며,생성된 빈에게 추가 기능을 제공해주는 컴포넌트이다.
- 빈(Bean)= Java 객체


## 스프링 컨터이너의 종류
![[spring container.png]]
- 스프링 컨테이너는 BeanFactory와 ApplicationContext 두 종류의 인터페이스로 구현되어있다.
- 주로 ApplicationContext를 사용한다.

### BeanFactory
- Bean을 등록,생성,조회하며 Bean을 관리하는 역할
- getBean()을 통해 Bean을 인스턴스화 가능
- @Bean 어노테이션이 붙은 메서드의 이름으로 Bean을 등록한다.

### ApplicationContext
- BeanFactory가 하지 않는 부가 기능 제공
- 부가 기능
	1. MessageSource
	2. EnvironmentCapable
	3. ApplicationEventPublisher
	4. ResourceLoader

## 스프링 컨터이너의 기능
- 개발자가 정의한 Bean을 객체로 만들어 관리하고 개발자가 필요할 때 제공
- 의존성 주입을 통해 애플리케이션의 컴포넌트를 관리
- 서로 다른 Bean을 연결하여 애플리케이션 Bean을 연결하는 역할
- -> 모듈 간에 의존 및 결합으로 인해 발생하는 문제 해결
- 싱글톤 방식???으로 빈을 관리

### 스프링 컨테이너를 사용하는 이유
- 객체를 생성하기 위해 new 생성자를 사용하면, 그로 인해 애플리케이션에 수많은 객체가 존재하고 서로 참조하게 된다. 하지만 참조가 많아질 수록 의존성이 높아지는데, 이는 낮은 결합도와 높은 캡슐화를 지향하는 객체지향과는 거리가 먼 방식이다.
- 따라서 스프링 컨테이너를 사용하여 객체 간의 ~={red}**의존성을 제거**=~하고 ~={red}**인터페이스에만 의존**=~하도록 설계해야 한다.