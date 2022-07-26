## Spring initializr   
스프링 프레임워크를 제작해줌   
   
## maven gradle   
gradle을 주로 사용하는 추세   
   
## 디펜던시 추가   
spring web   
thymelaf(템플릿엔진) html로 만들어주는 엔진   
   
## 라이브러리   
### spring-boot-web
톰캣   
webmvc 웹 mvc   
### 타임리프 템플릿 엔진(view)
### spring-boot-starter : 스프링부트 + 스프링코어 + 로깅

## 테스트라이브러리   
junit 테스트 프레임워크      
mockito 목 라이브러리   
assertj 테스트 코드를 좀 더 편하게 작성하게 도와주는 라이브러리   
spring-test 스프링 통합 테스트 지원   

## thymelaf
1번 커밋   
HelloController.java
```
@Controller
public class HelloController {
    @GetMapping("hello")
    public String hello(Model model) {
        model.addAttribute("data", "hello!!");
        return "hello";
    }
}
```
   
hello.html
```
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Hello</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
</head>
<body>
<p th:text="'안녕하세요. ' + ${data}" >안녕하세요. 손님</p>
</body>
</html>
```
hello 페이지 요청 -> helloController GetMapping -> return hello -> hello.html

## 실행
./gradlew build   
cd libs   
java -jar 파일명   
빌드 폴더 삭제 ./gradlew clean build
