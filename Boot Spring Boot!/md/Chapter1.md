## 스프링 부트 시작하기 

스프링 기반의 웹 애플리케이션을 개발하던 때와는 확연하게 다른 개발 경험을 제공 한다.

1. 다른언어에서 제공하는 `빠른 프로토타이핑`
2. `개발에서 운영`까지 이어지는 부드러운 흐름

- 스프링
    - XML을 이용해서 문맥을 작성 하여 애플리케이션 구조 생성
- 스프링부트
    - 컴포넌트와 설정을 조합하는 Java 를 이용해서 구성


`프로토타이핑 - 개발 - 운영/모니터링`까지 이어지는 일괄된 개발 환경 제공

- 스프링 부트의 기능
    - 단독실행 가능한 스프링 애플리케이션 생성
    - 내장 컨테이너로 톰캣, 제티 혹은 언더토우 중에서 선택가능(WAR 배포 필요 없음)
        - 스프링 부트의 spring-boot-starter-web은 기본적으로 는 톰캣을 포함하지만 개발자가 원하면, 제티 혹은 언더토우로 대체 할 수 있다.
    - 스타터(`starter`) POM을 제공하여 간결한 의존성 구성 지원
        - pom.xml
        - build.gradle
            - spring-boot-starter-*들은 각각의 컴포넌트가 실행되는데 필요한 의존성을 내포하고 있다.
    - 스프링에 대한 자동구성 제공
        - `@EnableAutoConfiguration` 어노테이션을 이용해서 각 컴포넌트별 기본적인 자동구성을 지원
    - 제품출시 후 운영에 필요한 상태, 건강 점검(health check)과 외부구성 등 다양한 기능 제공
    - 구성을 위한 XML 코드를 필요로 하지 않으며 작성하지 않아도됨
        - 자바구성(JavaConfig)을 통해서 애플리케이션에 필요한 컴포넌트 스캔(Component-Scan)과 스프링 빈(Bean) 등록 등을 할 수 있다.

따라서 스프링 부트를 사용하면 개발자는 비지니스 로직에만 집중할 수 있고, 필요한 기능을 가진 라이브러리를 추가하고 기능을 구성하고 배포하는 것은 스프링 부트가 알아서 해준다.

## 시스템 요구사항

스프링부트 2.0.0 Release는 기본적으로 스프링 프레임워크 5.0.1 Release 혹은 상위 버전을 요구한다. 스프링 프레임워크는 4.X 부터 Java 8을 완벽하게 지원한다.

> Java 8 : Stream API와 람다식 사용가능

스프링 프레임워크 5부터는 Java 9를 지원한다.

> https://docs.spring.io/spring/docs/current/spring-framework-reference/overview.html#overview

Spring Framework 5.1부터 Spring은 JDK 8+ (Java SE 8+)를 필요로하며 JDK 11 LTS를 즉시 지원합니다. Java SE 8 업데이트 60은 Java 8의 최소 패치 릴리스로 제안되지만 일반적으로 최신 패치 릴리스를 사용하는 것이 좋습니다.


## 프로젝트 들어가기에 앞서 Java 버전 고르기

Java 버전을 고르는데 가장 큰 영향을 주는 것은 `사용하는 컨테이너가 무엇인가?` 하는 것이다. 컨테이너 버전에 따라 `서블릿 스펙`이 달라지며 컨테이너에서 사용할 수 있는 기능이 제한될 수 있다.
서블릿 3.0은 Java 6까지 지원하며 서블릿 3.1은 Java 7부터 지원한다.

`Tomcat 8.5 - Servlet 3.1 - Java 1.7`

스프링 부트는 서블릿 3.0+ 이상을 지원하는 컨테이너면 어디에든 배포가 가능하다.

## 프로젝트 생성

- Spring Initializr

프로젝트 생성시 jdbc template를 클릭했으면 설정을 해줘야 오류가 안난다.

## 실행가능한 (Executable) jar 만들기

실행가능한 jar는 Fat Jars라고 불린다.

gradle의 경우 아래 코드를 추가해야 한다.

```java
buildscript {
    ext {
        springBootVersion = '2.0.0.RELEASE'
    }
    repositories {
        mavenCentral()
    }
    dependencies {
        classpath("org.springframework.boot:spring-boot-gradle-plugin:${springBootVersion}")
    }    
}
```

maven의 경우 아래 코드를 추가해야 한다.

- repackage: 자동 실행 가능한 jar 또는 war 파일을 작성하십시오. 일반 아티팩트를 대체하거나 별도의 분류기 를 사용하여 빌드 라이프 사이클에 첨부 할 수 있습니다 .
- run: Spring Boot 응용 프로그램을 여러 옵션으로 실행하여 매개 변수를 전달하십시오.
- start그리고 stop다음에 봄 부팅 응용 프로그램을 통합하는 integration-test응용 프로그램이 이전에 시작되도록 단계입니다.
- build-info: 액츄에이터가 사용할 수있는 빌드 정보를 생성합니다.

```xml
<build>
  ...
  <plugins>
    ...
    <plugin>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-maven-plugin</artifactId>
      <version>2.2.0.RELEASE</version>
      <executions>
        <execution>
          <goals>
            <goal>repackage</goal>
          </goals>
        </execution>
      </executions>
    </plugin>
    ...
  </plugins>
  ...
</build>
```

springinitializer을 통해서 프로젝트를 생성하면 아래 코드가 기본적으로 들어가 있다.

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```