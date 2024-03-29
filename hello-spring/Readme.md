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
>  [2번 커밋](https://github.com/kim-seungmin/Spring/commit/30c313c6eca925cdc0cc19d35aab858b7c17d8bb)   
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
>  [3번 커밋](https://github.com/kim-seungmin/Spring/commit/d7515eb24245f674edb1afde35bc707b0a71d096)   
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
>  웹브라우져에서 오픈시 ```<p>hello spring</p>``` 출력됨, th값을 숨길수있음   
>
>  ## api
>  [4번 커밋](https://github.com/kim-seungmin/Spring/commit/728a712ed53d85efeba702a54855df8dac1a29bf)   
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

# 백엔드 개발   
>    
> ## 컨트롤러, 서비스, 리포지토리   
> 컨트롤러: 웹 MVC의 컨트롤러 역할   
> 서비스: 핵심 비즈니스 로직 구현   
> 리포지토리: DB접근, 저장, 관리   
>
> [5번커밋](https://github.com/kim-seungmin/Spring/commit/ebd26a22ccf73b2a0b10582698ae2164ad34f9ee)   
> 로그인 제작시 DB가 확정되지 않아 interface를 통해 테스트   
> ## Optional 
> ```
> Optional<Member> findById(Long id);
>  ```
> null리턴시 optional을 감싸서 반환
> ```
>  memberRepository.findByName(member.getName())
>                .ifPresent(m -> {
>                    throw new IllegalStateException("이미 존재하는 회원 입니다");
>                });
>  ```
>  
>
> [6번커밋](https://github.com/kim-seungmin/Spring/commit/7ec3adb80a143b02b26b6b8ac5e401022ae8d4a5)
> ## Junit
> @test를 통해 선언 함수 부분만 실행 가능, 순서가 보장안됨 어느 함수가 먼져 실행될지 알수없음
>```
> @Test
>    public void save(){
>        Member member = new Member();
>        member.setName("spring");
>
>        repository.save(member);
>
>        Member result = repository.findById(member.getId()).get();
>        assertThat(member).isEqualTo(result);
>    }
>```
>
> ## Asertion
> junit   
> ```
> Assertions.assertEquals(member, result)
> ```
> assertj   
> ```
> assertThat(member).isEqualTo(result);
> ```
> 두 값이 다르면 오류 출력, 같으면 출력없음
>  
> ## AfterEach
> ```
>     @AfterEach
>    public void afterEach(){
>        repository.clearStore();
>    }  
> ```
> @Test가 종료될떄마다 실행  
>
> [7번커밋](https://github.com/kim-seungmin/Spring/commit/1ab25a4d806452ef0e8cef774eb170ad4285d860)   
>  ## 함수명 정하기
> 서비스 -> 비즈니스(join) ,리포지토리 -> 기계적(addMember)
>
> [8번커밋](https://github.com/kim-seungmin/Spring/commit/979a703d0a8e4047fc379e39bf51b71955092e9b) 
> ## given,when,then 주석   
> ```
>   //given
>   Member member = new Member();
>   member.setName("hello");
>   //when
>   Long saveId = memberService.join(member);
>   //then
>   Member findMember = memberService.findOne(saveId).get();
>   assertThat(member.getName()).isEqualTo(findMember.getName());
> ```
> given 값 지정 when 예외 발생 시점 then 예외검사
>  
  
# 스프링 빈과 의존관계
> [9번커밋](https://github.com/kim-seungmin/Spring/commit/cbcd5270573b9d0a0908d0e942fae8597705fdf4)
> ## @Service
> 스프링 컨테이너에 등록
> ## @Controller
> 컨트롤러 어노테이션 등록시 스프링이 객체 생성후 들고있음 -> 스프링 빈이 관리된다고 표현      
> ## @Autowired   
> 스프링 컨테이너에서 가져와 연결시켜줌 -> 여러개가 아니라 하나만 만들어 사용할수 있음   
> 기존방식 (컨테이너마다 새로 생성됨)
> ```
> MemberService memberService = new MemberService;
> ```
> Autowired (스프링 빈에 등록되어 있는 객체를 가져와 사용, 의존관계 주입)
> ```
> @Autowired
>    public MemberController(MemberService memberService) {
>        this.memberService = memberService;
>    }
>```
> ## @Repository
> 리포지토리로 등록 @Autowired로 가져와 사용
>
>|컴포넌트 스캔과 자동 의존관계 설정|자바 코드로 직접 스프링 빈 등록하기|
>|:---:|:---:|
>|@Service,@Controller,@Autowired|@Configuration, @Bean|
>| [9번커밋](https://github.com/kim-seungmin/Spring/commit/cbcd5270573b9d0a0908d0e942fae8597705fdf4) | [10번커밋](https://github.com/kim-seungmin/Spring/commit/b06a49f42d1c467b02f8519ea14ab87365d1944f) |   
>
> 의존관계가 실행중 동적으로 변하는 경우는 거의 없으므로 생성자 주입을 권장함   
> 실무에서는 주로 정형화된 컨트롤러, 서비스, 리포지토리 같은 코드는 컴포넌트 스캔을 사용한다. 정형화 되지 않거나 상황에 따라 구현클래스를 변경해야 하면 설정을 통해 스프링 빈으로 등록
# 웹MVC 개발   
> [11번커밋](https://github.com/kim-seungmin/Spring/commit/11a385ee86e7e15cd29063787e3c52f8ac85134f)   
> 메인페이지등록   
> ```
> @GetMapping("/")
> ```
> [12번커밋](https://github.com/kim-seungmin/Spring/commit/eeb0b2d87e1883ab56f4c4ec83d21e438c787a88)   
> 포스트 처리   
> ```  
>     @PostMapping("/members/new")
>    public String create(MemberForm form){
>        Member member = new Member();
>        member.setName(form.getName());
>
>        System.out.println("member = " + member.getName());
>        memberService.join(member);
>
>        return "redirect:/";
>    }
> ```
> [13번커밋](https://github.com/kim-seungmin/Spring/commit/1b8571e444d79b1828198ecf87a11c2ab1605102)   
> thymeleaf   
> ```
> <html xmlns:th="http://www.thymeleaf.org">   
> ~~~
>   <tr th:each="member : ${members}">
>     <td th:text="${member.id}"></td>
>     <td th:text="${member.name}"></td>  
> ```
> each="member : ${members}" forEach문과 동일 members리스트의 내부 객체를 member로 가져옴   
> .id, .name getId, getName으로 가져옴   
# 스프링 DB 접근 기술
> ## 순수 JDBC
> [14번커밋](https://github.com/kim-seungmin/Spring/commit/795bbd8cd8ae2614a98eb01e5b296d80b7f6cc75)  
> build.gradle 파일에 jdbc, h2 데이터 베이스 관련 라이브러리 추가
> ```
> implementation 'org.springframework.boot:spring-boot-starter-jdbc'
>	runtimeOnly 'com.h2database:h2'
>```  
> 그래들 아이콘(코끼리)클릭하여 라이브러리 설치   
>  
> hello-spring/src/main/resources/application.properties
> ```  
> spring.datasource.url=jdbc:h2:tcp://localhost/~/test
> spring.datasource.driver-class-name=org.h2.Driver
> spring.datasource.username=sa
> ```  
>  
> SpringConfig.java
> ```
> private DataSource dataSource;
>
>   @Autowired
>   public SpringConfig(DataSource dataSource){
>       this.dataSource = dataSource;
>   }
>  
>  ~~~~~
>
>  @Bean
>    public MemberRepository memberRepository(){
>        // return new MemoryMemberRepository();
>        return new JdbcMemberRepository(dataSource);
>    }
> ```  
> 스프링으로부터 dataSource를 받아옴, 메모리 리포지토리에서 jdbc리포지토리로 변경
> 
> JdbcMemberRepository.java
> ```
> public class JdbcMemberRepository implements MemberRepository {
>    private final DataSource dataSource;
>    public JdbcMemberRepository(DataSource dataSource) {
>        this.dataSource = dataSource;
>    }
>  ~~~~~~~
>   try {
>            conn = getConnection();
>            pstmt = conn.prepareStatement(sql,
>                    Statement.RETURN_GENERATED_KEYS);
>            pstmt.setString(1, member.getName());
>            pstmt.executeUpdate();
> ```  
> [15번커밋](https://github.com/kim-seungmin/Spring/commit/8079e298eae0ea96c71a8c658cfd0112f70c4617)  
> ## @SpringBootTest 
> 스프링 컨테이너와 테스트를 함께 실행
> ## @Transactional  
> 데이터베이스에 커밋하지않음 -> sql문 실행후 롤백
> [16번커밋](https://github.com/kim-seungmin/Spring/commit/be58116fe3f8e8ee85525f6d1829eb27783e306b)  
> ## JDBC Template
> db사용을 간편하게 바꿔 sql문 만으로 작동하게 해줌 
> ```
> @Autowired
>    public JdbcTemplateMemberRepositorty(DataSource dataSource){
>        jdbcTemplate = new JdbcTemplate(dataSource);
>    }
> ~~~~~~
>   return jdbcTemplate.query("select * from member", memberRowMapper());
> ```
> [17번커밋](https://github.com/kim-seungmin/Spring/commit/c0d3e0875090c10c8c839c89029ad8bd7a372f5c)
> #JPA
>  쿼리문까지 자동으로 처리해줌, @Transactional 와 @Commit을 필요로함
> ```
> em.persist(member);
> ~~~~~~~~
> return em.createQuery("select m from Member m", Member.class).getResultList();  
> ``` 
> 
> ## 스프링 데이터 JPA
> [18번커밋](https://github.com/kim-seungmin/Spring/commit/764da18dd63e7a6d94ee715b96b1f8569f969c99)  
> 인터페이스를 통해 간단한 CRUD를 지원 복잡한 동적 쿼리는 Quertdsl이라는 라이브러리 사용, 그래도 어려운 쿼리는 JPA가 제공하는 네이티브 쿼리 사용
# AOP
> [20번커밋](https://github.com/kim-seungmin/Spring/commit/b145ff668566eb847ba0bc28a64be76501864b9c)  
> 실행 될때마다 앞뒤로 실행이 필요한 경우 사용(예. 실행 시간 측정)
> ```
> @Aspect
> @Component
> public class TimeTraceAop {
>    @Around("execution(* hello.hellospring..*(..))")
>    public Object execute(ProceedingJoinPoint joinPoint) throws Throwable{
>        long start = System.currentTimeMillis();
>        System.out.println("Start"+joinPoint.toString());
>        try{
>            return joinPoint.proceed();
>        } finally{
>            long finish = System.currentTimeMillis();
>            long timeMs=finish-start;
>            System.out.println("END: " + joinPoint.toString() + " " + timeMs + "ms");
>        }
>    }
>}
> ```  
> @Aspect -> AOP로 선언  
> joinPoint.toString() -> 메서드명  
> joinPoint.proceed(); -> 콜한 메서드 실행
> @Around("execution(* hello.hellospring..*(..))")  -> 실행할 패키지 지정   

> ## SOLID 좋은 객체 지향 설계의 5가지 원칙   
> #  SRP 단일 책임 원칙   
> 한 클래스는 하나의 책임만 가져야한다   
> 변경이 있을때 파급 효과가 적으면 단일 책임 원칙을 잘 따른 것   
> EX) UI변경, 객체 생성 사용 변경   
> # OCP 개방-폐쇄 원칙   
> 확장에는 열려있고 변경에는 닫혀있어야 한다   
> 객체를 생성하고, 연관관계를 맺어주는 별도의 조립, 설정자가 필요   
> DI, 컨테이너를 필요   
> # LSP 리스코프 치환 원칙   
> 프로그램 객체는 프로그램 정확성을 깨뜨리지 않으면서 하위 타입의 인스턴스로 바꿀수 있어야함   
> 예) 자동차 인터페이스의 엑셀은 앞으로 가라는 기능, 뒤로가게 만들면 LSP위반, 느리게라도 앞으로 나아가야함   
> # ISP 인터페이스 분리 원칙   
> 특정 클라이언트를 위한 인터페이스 여러개가 범용 인터페이스 하나보다 낫다   
> 예) 자동차 인터페이스 -> 운전 인터페이스, 사용자 클라이언트 -> 운전자 클라이언트   
> # DIP 의존 관계 역전 원칙    
> 의존 관계 역전 원칙   
> 역할과 구현을 분리   
> 클래스에 의존하지 말고 인터페이스에 의존해야 유연하게 변경 가능
> DI, 컨테이너를 필요

단축키   
컨트롤+쉬프트+엔터 문장(if, for등)자동완성
쉬프트+윈도우+V 자동으로 리턴값을 만들어줌   
컨트롤+윈도우+쉬프트+T Refactor This
컨트롤+쉬프트+T TEST자동생성
쉬프트+F10 이전 실행 
