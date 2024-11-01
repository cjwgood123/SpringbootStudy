# Springboot ? 란 무엇일까

예전 강의를 들어 공부할때의 기억으로 작성해보면 ..  Srping  을  더 쉽게 사용하고 복잡한 설정들을 줄이기 위해 탄생한걸로 기억한다..
즉 , 간편한 개발을 위해 탄생했다 로 기억한다. 내장톰캣, 내장was는 제외하고.. Spring 과의 차이를 알아보면


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
이러한 xml파일들이 존재한다 .

# Springboot의 특징 

Springboot 에서는 xml 설정을 최소화하고 대신 ** Java Config ** 와 어노테이션 기반설정을 주로 사용한다.
** Springboot 의 철학은 XMl을 사용하지 않고도 어플리케이션 구성할수 있도록 하여 **   설정의 간편화 , 유지보수를 쉽게하기위함이다.

xml을 최소화 하고 
```
1. Javaconfig  : @Configuration , @bean 등의 어노테이션을 통해 Java코드로 필요한 빈을 정의한다. (설정용 xml을 config로 대체)
2. 어노테이션 기반 자동설정 : @Component , @Service , @Repository , @Controller 와 같은 어노테이션을 통해
자동으로 빈을 생성하고 관리한다.
3.자동설정 : 개발자가 일일히 설정하지 않아도 자동으로 필요한 bean을 설정해준다.
```



## Springboot 기본 파일 구성도.
```
my-springboot-app/
├── src/
│   ├── main/
│   │   ├── java/                     # Java 소스 파일
│   │   │   └── com/example/demo/     # 패키지 이름에 맞춘 디렉토리 구조
│   │   │       └── DemoApplication.java
│   │   ├── resources/
│   │   │   ├── application.properties # 애플리케이션 설정 파일
│   │   │   └── static/               # 정적 리소스 (HTML, CSS, JS 등)
│   │   │   └── templates/            # Thymeleaf 템플릿 (HTML 파일)
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

개인적으로  gradle이 좀더 세팅하기엔 간편한것 같다.





