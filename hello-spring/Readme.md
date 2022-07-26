# 프로젝트 
>  ## Spring initializr   
>  스프링 프레임워크를 제작해줌   
>   
>  ## maven gradle   
>  gradle을 주로 사용하는 추세   
>   
>  ## 디펜던시 추가   
>  spring web   
>  thymelaf(템플릿엔진) html로 만들어주는 엔진   
>     
>  ## 라이브러리   
>  ### spring-boot-web
>  톰캣   
>  webmvc 웹 mvc   
>  ### 타임리프 템플릿 엔진(view)
>  ### spring-boot-starter : 스프링부트 + 스프링코어 + 로깅
>
>  ## 테스트라이브러리   
>  junit 테스트 프레임워크      
>  mockito 목 라이브러리   
>  assertj 테스트 코드를 좀 더 편하게 작성하게 도와주는 라이브러리   
>  spring-test 스프링 통합 테스트 지원   
>
>  ## thymelaf
>  [2번 커밋](https://github.com/kim-seungmin/Spring/commit/30c313c6eca925cdc0cc19d35aab858b7c17d8bb, "이동")   
>  HelloController.java
>  ```
>  @Controller
>  public class HelloController {
>      @GetMapping("hello")
>      public String hello(Model model) {
>          model.addAttribute("data", "hello!!");
>          return "hello";
>      }
>  }
>  ```
>   
>  hello.html
>  ```
>  <!DOCTYPE HTML>
>  <html xmlns:th="http://www.thymeleaf.org">
>  <head>
>      <title>Hello</title>
>      <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
>  </head>
>  <body>
>  <p th:text="'안녕하세요. ' + ${data}" >안녕하세요. 손님</p>
>  </body>
>  </html>
>  ```
>  hello 페이지 요청 -> helloController GetMapping -> return hello -> hello.html
>  
>  ## 실행
>  ./gradlew build   
>  cd libs   
>  java -jar 파일명   
>  빌드 폴더 삭제 ./gradlew clean build   
>
# 스프링 웹 개발 기초   
>
>  ### 정적 컨텐츠
>  html파일등 그대로 서버에서 클라이언트로 전송   
>  
>  ## mvc
>  [3번 커밋](https://github.com/kim-seungmin/Spring/commit/d7515eb24245f674edb1afde35bc707b0a71d096, "이동")   
>  모델 뷰 컨트롤 분리
>  HelloController.java
>  ```
>  ~
>  @GetMapping("hello-mvc")
>     public String helloMvc(@RequestParam(value= "name") String name, Model model){
>     model.addAttribute("name", name);
>     return "hello-template";
>  }
>  ~
>  ```
>  hello-template.html   
>  ```
>  ~
>  <p th:text="'hello' + ${name}" >hello!</p>
>  ~
>  ```
>  hello-mvc?name=spring   
>  hello-mvc 접속 -> name의 값을 파라미터로 받아옴(spring) -> name에 spring저장 -> hello-template호출 -> hello spring 출력   
>  웹브라우져에서 오픈시 '<p>hello spring</p>' 출력됨, th값을 숨길수있음   
>
>  ## api
>  [4번 커밋](https://github.com/kim-seungmin/Spring/commit/728a712ed53d85efeba702a54855df8dac1a29bf, "이동")   
>  ```
>  @GetMapping("hello-string")
>     @ResponseBody
>     public String helloString(@RequestParam("name") String name){
>     return "hello "+name;
>  }
>  ```
>  ResponseBody = html이 아닌 http의 바디, 응답에 직접 값을 넣음, HttpMessageConverter를 통해 문자면 문자로(StringConverter)만 객체면 JSON 형식(JsonConverter)로 값을 넣음   
>  hello-string?name=spring 입력시 모든 태그없이 hello spring 출력   
>
>  ```
>      @GetMapping("hello-api")
>      @ResponseBody
>      public Hello helloApi(@RequestParam("name") String name) {
>          Hello hello = new Hello();
>          hello.setName(name);
>          return hello;
>      }
>      static class Hello {
>          private String name;
>
>          public String getHello() {
>              return name;
>          }
>
>        public void setName(String name) {
>              this.name = name;
>          }
>      }
>  ```
>  hello-api?name=spring 입력시 json 형식으로 출력   
>  {"name":"spring"}   
