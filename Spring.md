# Spring과 Spring Boot

## 부록. 프로젝트 환경설정
* start.spring.io에서 spring intializer를 이용한다.
* 프로젝트는 Gradle-Grooovy (pom.xml을 사용하는 Maven 대신 가독성이 좋고 더 유연한 Gradle을 사용)
* 언어는 당연히 Java, Java버전은 11 사용. LTS이면서 Java8과 동시에 가장 많이 사용되는 버전!
    * Java 8 : 람다, 스트림, 인터페이스 내 디폴트 메서드, Optional 클래스
    * Java 11 : String 클래스에 repeat, strip등 메서드 추가, 람다 파라미터에 var 사용, HTTP 클라이언트 라이브러리 추가
* 스프링 부트 2.7.x를 사용 (SNAPSHOT은 아직 개발단계임을 의미 => SNAPSHOT이 아닌 GA를 사용하도록 하자.)
* 패키징은 JAR로 사용. Dependencies에서 Spring Web을 추가하면, Tomcat이 Embeding된다. 
* 즉, 내장 톰캣으로 따로 서버를 설치하지 않아도 돌릴 수 있다.
* Dependencies는 Java Web(Spring MVC, Embeded Tomcat Container..)와 Thymeleaf(서버-사이드 템플릿, 디폴트 값 부여.)

## build.gradle
```
plugins {
	id 'java'
	id 'org.springframework.boot' version '2.7.12'
	id 'io.spring.dependency-management' version '1.0.15.RELEASE'
}

group = 'hello'
version = '0.0.1-SNAPSHOT'

java {
	sourceCompatibility = '11'      // Java 버전
}

repositories {
	mavenCentral()      // mavenCentral에서 종속성들을 가져온다.
}

dependencies {
	implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'     // 타임리프 템플릿 엔진 (View)
	implementation 'org.springframework.boot:spring-boot-starter-web'       // Spring Web (톰캣, web MVC) 
    // Spring Starter 공통 라이브러리에는 스프링 부트 + 스프링 코어 + 로깅(logback, slf4j) 등이 추가된다.
    // test를 위한 종속성인데, Junit, mockito, assertj 등 기본적으로 추가된다.
	testImplementation 'org.springframework.boot:spring-boot-starter-test'  
}

tasks.named('test') {
	useJUnitPlatform()
}
```

## 1. 간단하게 돌려보기 (Thymeleaf 사용)
* 스프링 부트에서는 Welcome Page라고 해서, static/index.html을 사이트 전면 페이지로 제공한다. 
```
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

/*
Controller와 스프링 WEB 작동원리.
Spring MVC의 기본적인 흐름은 Client의 요청은 @Controller에 진입해서 
Controller가 요청에 대한 작업을 수행하고 View로 데이터를 전달한다.

브라우저가 서버에 요청. ex) 'localhost:8080/hello' -> GET 요청임.
내장 톰캣 서버가 스프링부트, 스프링 컨테이너에서 매핑된 메서드를 찾아 실행.(HelloController)
Controller에서 문자 값을 반환하면 'viewResolver'가 해당하는 화면(문자값)을 찾아서 처리
스프링 부트 템플릿 엔진은 기본적으로 viewName 매핑을 지원한다.
viewResolver는 'resources:templates/' + '${viewName: hello}' + '.html' 에서 찾는다.
 */
@Controller
public class HelloController {
    /*
    '@GetMapping("hello")'의 의미?
    =>  Get 메서드를 통해 들어온 요청. 즉, 브라우저가 'localhost:8080/hello'로 서버에 요청했을 때.
    이 메서드를 실행시켜라.
     */
    @GetMapping("hello")
    public String hello(Model model) {      // 모델 객체를 통해 View로 데이터 전달
        model.addAttribute("data", "hello!!");
        /*
        'return "hello";'의 의미?
        =>  resourcse/templates/ 에 있는 'hello'라는 이름의 파일로 넘겨(렌더링해)라 (View 이름을 리턴한다.)
         */
        return "hello";
    }
}
```

## 빌드하고 실행하기 (CLI, iterm 환경)
* CLI 환경에서 스프링 프로젝트를 빌드하고 실행하기
* 프로젝트 폴더로 이동. ex) cd Users/namu/SpringProject/hello-spring
* gradlew를 가지고 빌드하면 된다. ex) ./gradlew build
* 만일 오류가 발생한다면, './gradlew clean'로 삭제 이후 다시 빌드하거나 './gradlew clean build' 사용
* 생성되는 build 디렉토리 안에 libs 디렉토리에서 'hello-spring-0.0.1-SNAPSHOT.jar'의 예시와 같이 생성되는 jar파일을 실행하 면된다.
* java -jar hello-spring-0.0.1-SNAPSHOT.jar를 통해 실행. '^ + c' 키로 종료(mac)

