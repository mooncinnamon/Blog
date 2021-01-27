# Spring - DI - 개요

## Tags

- TAG
    - WEB
    - Spring

## Category

Spring

## Content

### 들어가며

Spring을 너무 띄엄띄엄 아는거 같아서 다시 공부를 시작했다. 계속 Spring boot 만 해서 그런지 Spring 에 대해 모르는 거 같기도 하고 그래서 기초적인 부분은 빼고 Spring core에 가깝게 공부를 시작하려고 무작정 시작했는데 뭐부터 할까 생각하니...  일단 스프링 코어 중 중요한 DI와 AOP부터 보기로 했다.

### DI : Dependency Injection

DI 는 왜 필요할까? 사실 이런거 잘 몰라도 개발은 잘 했다. 요즘 스프링 사용해보면 라이브러리와 어노테이션이 너무 잘 되어 있어서 초기 설계만 잘 하면 큰 어려운 없이 개발을 할 수가 있다. 

엔터프라이즈 애플리케이션을 개발하다보면 하나의 처리를 위해 한 개의 컴포넌트에서 다 처리하는 게 아니라 여러 컴포넌트를 조합해 구현을 한다. 예를 들면 데이터베이스를 접근하기 위한 컴포넌트, 보안을 위한 컴포넌트 등 다양한 컴포넌트를 이용하여 기능을 만들어 나가는 것이다. 

이렇게 하나의 처리를 만들기 위해 여러 컴포넌트를 조합할때 DI가 강점을 가지게 되는데 이는 외부에서 객체를 만들어서 주입하는 DI의 특성상 확장성이 좋고 (필요하면 외부에서 만들어서 가져다 붙이면 됨) 유지보수가 용이하다 (설계에 따라 다르겠지만 SOLID가 잘 지켜진 컴포넌트라면 해당 컴포넌트만 수정하면 된다) 는 이유가 있다.

### DI 가 그래서 뭔데?

DI 는 IoC (Inversion of Control) 의 디자인 패턴 중 하나인데 한국어로 번역하면 주도권의 역전  정도의 의미를 가지게 된다. 인스턴스간의 생성과 의존관계를 코드에서 파악하는 것이 아니라. DI 컨테이너에서 관리를 시켜주기 때문에 IoC 의 디자인 패턴이라고 한다.

개념 자체는 DI 컨테이너가 인스턴스들의 생성주기과 스코프를 관장하면서 컴포넌트간의 결합도를 낮추는 것인데 이 덕분에 우리는 Interface로 먼저 틀을 잡아두고, 나중에 세부 기능을 개발할 수 있어지는 것이다.

Spring 공식에서는 IoC 컨테이너라고 되어있지만, 보통 DI 컨테이너로 많이 불린다.

### Spring 에서는 어떻게 쓰일까?

사실 오늘의 핵심은 여기서부터다. IoC 디자인 패턴이나 DI Framework는 좀 더 나중에 고도화 시키기로 하고, 우선적으로 Spring에서는 어떻게 사용되는지 알아보자.

```java
ApplicationContext ctx = new AnnotationConfigApplicationContext(AppConfig.class);
MyService myService = ctx.getBean(MyService.class);
```

Spring 레퍼런스를 보면 DI 컨테이너에서 인스턴스를 꺼내 사용하는 예시가 있다. [[docs.Spring 참조](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-java-instantiating-container)]

먼저 첫 줄에서 AppConfig라는 설정 클래스를 파라미터로 전달해 ApplicationContext 라는 DI Container를 생성한다. 이후, getBean 메소드를 사용해 인스턴스를 가져오는 코드다.

Java Code 기반으로 설정할 시, 아래처럼 어노테이션만 정의해주면 자동으로 스캔해준다.

```java
@Configuration
@ComponentScan(basePackages = "com.acme") 
public class AppConfig  {
    ...
}
```

우리는 여기서 Spring 에서 사용하는 인스턴스들은 ApplicationContext에서 관리를 받고 있으며 인스턴스를 사용하기 위해서는 ApplicationContext를 통해 사용해야한다는 사실을 알 수 있다. 

### 다음글은?

DI의 개요에 대해서 알아보았다. 사실 DI에 관한 내용은 꽤 많은데 이걸 글 하나로 정리할 것인가 분할 할 것인가 고민을  했지만 역시 개요는 읽지 않고 먼저 소스코드를 보는 나 같은 사람들이 있을꺼라고 (습관이다) 생각하고 개요 부분만 간단하게 썼다. ㅇ앞으로 이 DI가 어떻게 동작하며 어떤 옵션값을 가지고 있는 지 알아 볼 예정이다.