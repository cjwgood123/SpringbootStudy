# Springboot ? 란 무엇일까

예전 강의를 들어 공부할때의 기억으로 작성해보면 ..  Srping  을  더 쉽게 사용하고 복잡한 설정들을 줄이기 위해 탄생한걸로 기억한다..
즉 , 간편한 개발을 위해 탄생했다 로 기억한다... Spring 과의 차이를 알아보면


## Spring에서의 설정을 기억해보면  각종 xml 설정들..

```
1.pom.xml:
 Maven의 빌드 정보를 담고 있는 파일로 ( Project Object Model 의 약자)

2.web.xml:
 WAS(Web Application Server)가 최초로 구동될 때, web.xml파일을 읽어서 메모리에 올리고 웹에서 사용하는 다양한 설정들을 하는 곳 즉, Web Application 설정을 위한 Deployment descriptor(DD, 배포 설명자) 라고 한다
Client에게 요청 받아올 때 web.xml에 정의되어 있는 URL이 Client 요청 URL과 매핑이 되는 경우 DispatcherServlet 이 이요청을 가로채가서 서비스를 제공하도록 합니다.
3.servlet.xml:
 웹 어플리케이션에서 클라이언트의 요청을 받기 위한 컨텍스트 설정이며, 요청과 관련된 객체를 정의. url과 관련된 Controller나, 어노테이션, ViewResolver(컨트롤러에서 view정보에 대해 설정하는 것), Interceptor, MultipartResolver 등의 설정을 해준다.
4.root-context.xml:
servlet-context와는 반대로 view와 관련되지 않은 객체를 정의. Service, Repository(DAO), DB
등 비즈니스 로직과 관련된 설정을 해준다.

5.servlet-context: 여기에 등록되는 Bean 들은 servlet-container에만 사용되어짐 **
 **root-context: 여기에 등록되는 Bean들은 모든 context에 사용되어짐(공유 가능)** 

6.mybatis-config.xml:
SQL 쿼리를 선언한 Mapper 에서 데이터를 자동 매핑할 수 있도록 VO(DTO) 객체 설정
여러 vo 설정 가능DataSource 에 관한 설정 (DI 방법에 따라 선택. 즉 생략 가능)
<environment~~> 는 여러개의 DB접속 내용을 추가로 설정할 수 있다

Mapper 파일의 위치 설정
여러개의 Mapper 파일을 설정 할 수 있는데, 단, 각 Mapper 파일 내부의 namespace는 전체 프로젝트에서 유일한 값.

7.mapper.xml:  SQL 쿼리 작성
```
이러한 xml파일들이 존재한다 

 ### Spring은 Aop 설정도 너무 귀찮다 .
![image](https://github.com/user-attachments/assets/31c6c4ef-3e2c-4076-9e7f-2a8090f96461)

```
Application Context
Web Application 최상단에 위치하고 있는 Context  Spring에서 ApplicationContext란 BeanFactory를 상속받고 있는 Context
Spring에서 root-context.xml, applicationContext.xml 파일은 ApplicationContext 생성 시 필요한 설정정보를 담은 파일 (Bean 선언 등..)
Spring에서 생성되는 Bean에 대한 IoC Container (또는 Bean Container) 특정 Servlet설정과 관계 없는 설정을 한다
(@Service, @Repository, @Configuration, @Component)
서로 다른 여러 Servlet에서 공통적으로 공유해서 사용할 수 있는 Bean을 선언한다.
Application Context에 정의된 Bean은 Servlet Context에 정의 된 Bean을 사용할 수 없다.


Servlet-Context (servlet-context.xml)
Servlet 단위로 생성되는 context
Spring에서 servlet-context.xml 파일은 DispatcherServlet 생성 시에 필요한 설정 정보를 담은 파일
(Interceptor, Bean생성, ViewResolver등..)
URL설정이 있는 Bean을 생성 (@Controller, Interceptor)
Application Context를 자신의 부모 Context로 사용한다.
Application Context와 Servlet Context에 같은 id로 된 Bean이 등록 되는 경우,
Servlet Context에 선언된 Bean을 사용한다.
Bean 찾는 순서 -> Servlet Context에서 먼저 찾는다.

만약 Servlet Context에서 bean을 못찾는 경우 Application Context에 정의된 bean을 찾는다.
Servlet Context에 정의된 Bean은 Application Context의 Bean을 사용할 수 있다.
```

# Springboot의 특징 
Springboot 의 철학은 XMl을 사용하지 않고도 어플리케이션 구성할수 있도록 하여    설정의 간편화 , 유지보수를 쉽게하기위함이다.
xml 설정을 최소화하고 대신  Java Config  와 어노테이션 기반설정을 주로 사용한다.
```

1. Javaconfig  : @Configuration , @bean 등의 어노테이션을 통해 Java코드로 필요한 빈을 정의한다. (설정용 xml을 config로 대체)
2. 자동 구성(Auto-configuration) :
SpringBoot는 classpath 설정, 다른 bean, 다양한 프로퍼티 설정에 기초하여 Spring 애플리케이션에 대한 합리적인 기본 설정을 제공
@Component , @Service , @Repository , @Controller 와 같은 어노테이션을 통해 자동으로 빈을 생성하여  설정하기가 쉽다.

자동구성.. 자동설정 이라는게 특징으로 많이 나오는데  ,
SpringBoot 의 메인 클래스에서는 @SpringBootApplication 때문에 자동으로 웹 애플리케이션이 동작한다.

 @SpringBootApplication 에는
   @EnableAutoConfiguration
   @ComponentScan(basePackages= {”해당 패키지 경로”}) 가 포함된다.

3.스타터 의존성(Starter Dependencies)
SpringBoot는 ‘starters’라는 의존성 집합을 제공하여 Maven이나 Gradle 설정을 간소화하고, 통합을 쉽게 만듦

4.내장 서버(Enbedded Servers)
Tomcat, Jetty, Undertow 등의 내장 서버를 통해 독립 실행형 Java 애플리케이션으로 배포 가능하게 함

5.상태 체크 및 외부 구성(Health checks & Externalized Configuration)
SpringBoot는 애플리케이션 상태 체크 기능을 제공
외부 구성을 통해 애플리케이션의 동작 방식을 변경하는 데 도움이 됨  _안써봄

6.애플리케이션 모니터링
SpringBoot Actuator를 통해 애플리케이션의 상세 정보를 모니터링하고 관리할 수 있음 _안써봄.
```


## spring pom.xml 

![image](https://github.com/user-attachments/assets/2f43006d-6d95-4f11-9991-fa732776ca2c)


단순하게 봐도 Springboot가 더 간편한  DI 설정을 통해 어플리케이션 개발 시작을 할수 있다.

## Springboot pom.xml

![image](https://github.com/user-attachments/assets/99f2b89c-1898-491f-a719-09ab1c6632f8)



### Srping 기본 파일 구성도 

```
my-springmvc-app/
├── src/
│   ├── main/
│   │   ├── java/                     # Java 소스 파일
│   │   │   └── com/example/demo/     # 패키지 이름에 맞춘 디렉토리 구조
│   │   │       ├── controller/       # 컨트롤러 클래스
│   │   │       ├── model/            # 모델 클래스
│   │   │       ├── service/          # 서비스 클래스
│   │   │       └── repository/       # 데이터 접근 클래스
│   │   ├── resources/
│   │   │   ├── applicationContext.xml # Spring 설정 파일
│   │   │   ├── static/               # 정적 리소스 (HTML, CSS, JS 등)
│   │   │   └── views/                # JSP, Thymeleaf와 같은 뷰 파일
│   ├── test/
│       └── java/                     # 테스트 소스 파일
│           └── com/example/demo/
│               └── DemoApplicationTests.java
├── pom.xml                           # Maven 설정 파일
└── web.xml                           # 웹 애플리케이션 설정 파일 (web.xml)

```



## Springboot 기본 파일 구성도.
```
my-springboot-app/
├── src/
│   ├── main/
│   │   ├── java/                     # Java 소스 파일
│   │   │   └── com/example/demo/     # 패키지 이름에 맞춘 디렉토리 구조
│   │   │       ├── controller/       # 컨트롤러 클래스
│   │   │       │   └── HomeController.java
│   │   │       ├── model/            # 모델 클래스
│   │   │       │   └── User.java
│   │   │       ├── service/          # 서비스 클래스
│   │   │       │   └── UserService.java
│   │   │       └── repository/       # 데이터 접근 객체(DAO) 클래스
│   │   │           └── UserRepository.java
│   │   ├── resources/
│   │   │   ├── application.properties # 애플리케이션 설정 파일
│   │   │   ├── static/               # 정적 리소스 (HTML, CSS, JS 등)
│   │   │   └── templates/            # Thymeleaf 템플릿 (HTML 파일)
│   │   │       └── home.html         # 홈 페이지 템플릿
│   ├── test/
│       └── java/                     # 테스트 소스 파일
│           └── com/example/demo/
│               └── DemoApplicationTests.java
├── .gitignore                        # Git 무시 파일
├── mvnw, mvnw.cmd                    # Maven wrapper 파일
├── gradlew, gradlew.bat              # Gradle wrapper 파일
├── pom.xml                           # Maven 설정 파일 (Maven 프로젝트일 경우)
└── build.gradle                      # Gradle 설정 파일 (Gradle 프로젝트일 경우)
```


## Springboot Db 설정방법.
1. application.properties  or application.yml
2. **@Configuration** 사용하여  java 파일을 설정파일로 인식해주기.
```
@Configuration
public class DatabaseConfig {

    @Bean
    public DataSource dataSource() {
        DriverManagerDataSource dataSource = new DriverManagerDataSource();
        dataSource.setDriverClassName("com.mysql.cj.jdbc.Driver");
        dataSource.setUrl("jdbc:mysql://localhost:3306/mydatabase");
        dataSource.setUsername("username");
        dataSource.setPassword("password");
        return dataSource;
    }
``` 

#application.propperties. 파일 경우 .
```
# H2 데이터베이스 설정
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driver-class-name=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=
spring.h2.console.enabled=true # H2 콘솔 활성화
spring.jpa.hibernate.ddl-auto=update # 데이터베이스 스키마 자동 생성
spring.jpa.show-sql=true # SQL 쿼리 출력

````

#yml 인경우
```
spring:
  datasource:
    url: jdbc:h2:mem:testdb
    driver-class-name: org.h2.Driver
    username: sa
    password: 
  h2:
    console:
      enabled: true # H2 콘솔 활성화
  jpa:
    hibernate:
      ddl-auto: update # 데이터베이스 스키마 자동 생성
    show-sql: true # SQL 쿼리 출력

```







AOP 출처 https://jamesyleather.tistory.com/387


