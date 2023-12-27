
스프링부트로 게시판 만들기 (2023.12.18~진행중)


<details>
<summary>
  <img src="" alt="" width="2%" /> 게시판 프로젝트 구조
</summary>
   <br>


![캡처](https://github.com/asdfwoomin/woomin/assets/154343478/d2dffb55-31b5-4d04-b385-a3110cb57974)



  
<p>$\bf{\large{\color{#6580DD}1. src/main/java 디렉터리}}$</p>

스프링 레거시와 마찬가지로 클래스, 인터페이스 등 Java 관련 파일이 위치하는 디렉터리입니다.

<p>$\bf{\large{\color{#6580DD}2. BoardApplication 클래스}}$</p>

이전 글에서 생성한 Board 프로젝트의 com.study 패키지에는 우리가 생성하지 않은 BoardApplication 클래스가 포함되어 있습니다.  main( ) 메서드는 SpringApplication.run( )을 호출해서 웹 애플리케이션을 실행하는 역할을 합니다.
 다음은 클래스 레벨에 선언된 @SpringBootAplication 어노테이션입니다. 해당 어노테이션은 다음의 세 가지 어노테이션으로 구성되어 있습니다.

        2-1. @EnableAutoConfiguration
        스프링 부트는 개발에 필요한 몇 가지 필수적인 설정들의 처리가 되어 있으며, 해당 애너테이션에 의해 다양한 설정들의 일부가 자동으로 완료됩니다.

        2-2. @ComponentScan
        XML 설정 방식을 이용하는 스프링 레거시는 빈(Bean)의 등록 및 스캔을 위해, 수동으로 ComponentScan을 여러 개 선언하는 방식을 사용했었습니다.스프링 부트는 해당 어노테이션에 의해 자동으로 컴포넌트 클래스를 검색하고, 스프링 애플리케이션 콘텍스트(IoC 컨테이너)에 빈(Bean)으로 등록합니다. 쉽게 말해, 의존성 주입(DI) 과정이 더욱 간편해졌다고 생각할 수 있습니다.

        2-3. @Configuration
        해당 어노테이션이 선언된 클래스는 Java 기반의 설정 파일로 인식됩니다. 스프링 4 버전부터 Java 기반의 설정이 가능하게 되었으며, XML 설정에 큰 시간을 소모하지 않아도 됩니다.

<p>$\bf{\large{\color{#6580DD}3. src/main/resources 디렉터리}}$</p>

스프링 레거시는 프로젝트가 생성되었을 때 해당 디렉터리에 log4.xml 파일만  생성되었습니다. 스프링 부트는 templates 폴더, static 폴더, application.properties 파일이 기본적으로 생성됩니다.

        3-1. templates
        스프링 레거시는 HTML 내에 Java 코드를 삽입하는 방식인 JSP를 주로 사용했었습니다. 하지만, 스프링 공식 문서에서는 View(화면) 영역에서 JSP가 아닌 타임리프(Thymeleaf) 템플릿 엔진의 사용을 권장하고 있습니다.타임리프는 JSP와 마찬가지로 HTML 내에서 Java 영역의 데이터를 처리하는 데 사용됩니다. 문법 또한 JSTL과 유사하기에(큰 차이가 없음), 결론적으로 해당 디렉터리에는 타임리프 관련 파일이 위치하게 되고, 타임리프는 HTML5 기반이기 때문에 HTML 파일로 화면을 구성합니다.

        3-2. static
        해당 폴더에는 css, fonts, images, plugin, scripts 등의 정적 리소스 파일이 위치합니다.

        3-3. application.properties
        해당 파일은 웹 애플리케이션을 실행하면서 자동으로 로딩되는 파일입니다. 예를 들어, 부트에 내장된 톰캣의 포트 번호, 콘텍스트 패스(Context Path) 설정이나, 데이터베이스 관련 정보 등 애플리케이션에서 사용하는 여러가지 설정을 해당 파일에 Key - Value 형식으로 선언해서 사용할 수 있습니다.선언한 속성은 일반적으로 설정(Configuration) 파일에서 사용합니다.


<p>$\bf{\large{\color{#6580DD}4. src/test/java 디렉터리}}$</p>

해당 디렉터리의 com.study 패키지에는 BoardApplicationTests 클래스가 생성되어 있습니다. 해당 클래스를 이용해서 개발 단계별로 단위 테스트를 진행하게 되며, 스프링 레거시와는 달리 복잡한 설정 없이 곧바로 테스트가 가능합니다.

<p>$\bf{\large{\color{#6580DD}5. build.gradle}}$</p> 
프로젝트를 생성하면서 프로젝트의 빌드 도구를 그레이들(Gradle)로 선택했습니다. 기존의 스프링은 pom.xml에 dependency를 추가해서 라이브러리를 관리하는 방식의 메이븐(Maven)을 이용했었는데 최근에는 메이븐 보다 그레이들을 선호하는 추세라고 합니다. 메이븐은 하나의 라이브러리를 추가하려면 평균적으로 네 줄 이상의 코드를 작성해야 하지만, 그레이들은단 한 줄의 코드로 라이브러리를 추가할 수 있습니다.
 프로젝트에 포함된 모든 라이브러리는 IDE의 External Libraries에서 확인할 수 있습니다.
 
<p>$\bf{\large{\color{#6580DD}6. MVC 패턴}}$</p> 

기존의 스프링과 마찬가지로 MVC 패턴으로 개발하게 됩니다.
 
   
        

         6-1.  모델, Model - (M)
           데이터를 처리하는 영역으로, 흔히 비즈니스 로직을 처리하는 영역이라고 이야기합니다. 해당 영역은 DB와 통신하고, 사용자가 원하는 데이터를 가공하는 역할을 합니다.

         6-2.  뷰, View - (V)
           사용자가 보는 화면을 의미하며, 타임리프(HTML)를 이용해서 화면을 처리합니다.뷰(View) == 화면(UI) == 사용자(User)
   
         6-3.  컨트롤러, Controller - (C)
           모델(M)과 뷰(V)의 중간 다리 역할을 하는 영역입니다. 사용자가 웹에서 어떠한 요청을 하면 가장 먼저 컨트롤러를 경유합니다. 컨트롤러는 사용자의 요청을 처리할 어떠한 로직을 호출하고, 호출한 로직의 실행 결과를 사용자에게 전달하는 역할을 합니다.예를 들어, 사용자가 게시글 등록을 요청하면 컨트롤러는 게시글의 제목, 내용, 작성자 등 사용자가 입력한 데이터(파라미터)를 전달받아 유효성을 검증합니다.검증이 완료되면 모델 영역에 데이터의 가공을 요청하며, 가공이 완료되면 전달받은 데이터를 DB에 저장한 후, 데이터 등록 성공 또는 실패 여부를 컨트롤러로 전달합니다. 마지막으로 컨트롤러는 등록 요청에 대한 결과를 사용자(View)에게 전달합니다.

</details>


<details>
<summary>
  <img src="" alt="" width="2%" /> 게시판 mariaDB 연동하기
</summary>
   <br>




<p>$\bf{\large{\color{#6580DD}1. 데이터 소스(DataSource) 설정하기}}$</p> 

데이터 소스는 DB와의 커넥션을 관리해 주는 인터페이스입니다.
 
데이터 소스 설정은 대표적으로 두 가지 방법을 이용할 수 있습니다.

     
   1-1. application.properties에 DB 정보를 선언해 두고, 설정(Configuration) 파일에서 참조하는 방법
     
   1-2. 설정(Configuration) 파일에서 DB 정보를 직접 입력하는 방법

 
이 중 1-1 방법을 이용해 데이터 소스 빈(Bean)을 구성합니다. 우선, src/main/resources 디렉터리의 application.properties에 코드작성


        spring.datasource.hikari.driver-class-name=net.sf.log4jdbc.sql.jdbcapi.DriverSpy
        spring.datasource.hikari.jdbc-url=jdbc:log4jdbc:mariadb://localhost:3306/board?serverTimezone=Asia/Seoul&useUnicode=true&characterEncoding=utf8&useSSL=false&allowPublicKeyRetrieval=true
        spring.datasource.hikari.username=root
        spring.datasource.hikari.password=root
        spring.datasource.hikari.connection-test-query=SELECT NOW() FROM dual


        jdbc-url
        데이터베이스의 주소를 의미합니다. 포트 번호(3306) 뒤의 board는 애플리케이션에서 참조할 DB의 이름이며, serverTimezone 등의 파라미터는 시간, 한글 처리 등 기본적인 설정을 처리하는 용도의 파라미터입니다.

        username
        DB의 계정 아이디를 의미합니다. 별개로 추가한 사용자가 없다면, 마스터 계정인 root를 입력해 주시면 됩니다.

        password
        username에 입력한 사용자의 비밀번호를 의미합니다. 별개로 추가한 사용자가 없다면, root 계정의 비밀번호를 입력해 주시면 됩니다. 아이디랑 똑같이 root로 하였습니다.

        connection-test-query
        커넥션이 정상적으로 맺어졌는지 확인하기 위한 SQL 쿼리입니다. 애플리케이션이 실행되면 다음의 테스트 쿼리가 콘솔에 출력됩니다.


<p>$\bf{\large{\color{#6580DD}2. 데이터 소스 설정(Data Source Configuration) 클래스 추가하기}}$</p> 
스프링 부트는 클래스 선언부에 @Configuration 어노테이션만 선언해 주면, 해당 파일이 Java 기반의 설정 파일임을 인식합니다.
데이터 소스 객체(Bean)를 관리해 줄 설정(Configuration) 클래스가 필요합니다.

      2-1. 패키지, 클래스 추가하기
      src/main/java 디렉터리의 com.study 패키지에 config 패키지를 추가하고, 그 안에 DatabaseConfig 클래스를 추가를 해야 합니다.
      소스는 다음과 같습니다.
   
        package com.study.config;

        import com.zaxxer.hikari.HikariConfig;
        import com.zaxxer.hikari.HikariDataSource;
        import org.apache.ibatis.session.SqlSessionFactory;
        import org.mybatis.spring.SqlSessionFactoryBean;
        import org.mybatis.spring.SqlSessionTemplate;
        import org.springframework.beans.factory.annotation.Autowired;
        import org.springframework.boot.context.properties.ConfigurationProperties;
        import org.springframework.context.ApplicationContext;
        import org.springframework.context.annotation.Bean;
        import org.springframework.context.annotation.Configuration;
        import org.springframework.context.annotation.PropertySource;

        import javax.sql.DataSource;

        @Configuration
        @PropertySource("classpath:/application.properties")
        public class DatabaseConfig {

            @Autowired
            private ApplicationContext context;

            @Bean
            @ConfigurationProperties(prefix = "spring.datasource.hikari")
            public HikariConfig hikariConfig() {
                return new HikariConfig();
            }

            @Bean
            public DataSource dataSource() {
                return new HikariDataSource(hikariConfig());
            }

            @Bean
            public SqlSessionFactory sqlSessionFactory() throws Exception {
                SqlSessionFactoryBean factoryBean = new SqlSessionFactoryBean();
                factoryBean.setDataSource(dataSource());
        //		factoryBean.setMapperLocations(context.getResources("classpath:/mappers/**/*Mapper.xml"));
        //    주석처리된 부분은 myBatisX 플러그인 설치 후 가능합니다.
                return factoryBean.getObject();
            }

            @Bean
            public SqlSessionTemplate sqlSession() throws Exception {
                return new SqlSessionTemplate(sqlSessionFactory());
            }

        }


어노테이션
설명


        @Configuration
        스프링은 @Configuration이 선언된 클래스를 자바(Java) 기반의 설정 파일로 인식합니다. 스프링 레거시의 XML 설정 방식을 Java 클래스로 대체한 것으로 생각하면 됩니다.


        @PropertySource
        해당 클래스에서 참조할 properties의 경로를 선언(지정)합니다.


        @Autowired
        빈(Bean)으로 등록된 인스턴스(이하 객체)를 클래스에 주입하는 데 사용합니다. @Autowired 이외에도 @Resource, @Inject 등이 존재합니다.나중에는 롬복(Lombok)이라는 라이브러리를 이용해 스프링에서 권장하는 생성자 주입 방식을 이용합니다.


        ApplicationContext
        스프링 컨테이너(Spring Container) 중 하나입니다. 컨테이너는 사전적 의미로 무언가를 담는 용기 또는 그릇을 의미하는데요. 스프링 컨테이너는 빈(Bean)의 생성과 사용, 관계, 생명 주기 등을 관리합니다.빈(Bean)은 쉽게 말해 Java 객체입니다. 예를 들어, 프로젝트에 100개의 클래스가 있다고 가정해 보겠습니다. 이 클래스들이 서로에 대한 의존성이 높다고 했을 때 "결합도가 높다."라고 표현하는데, 이러한 문제를 컨테이너에서 빈(Bean)을 주입받는 방법으로 해결할 수 있습니다. 즉, 클래스간의 의존성을 낮출 수 있는 것입니다.


        @Bean
        Configuration 클래스의 메서드 레벨에만 선언이 가능하며, @Bean이 선언된 객체는 스프링 컨테이너에 의해 관리되는 빈(Bean)으로 등록됩니다.해당 어노테이션은 인자로 몇 가지 속성(옵션)을 지정할 수 있습니다. 


        @ConfigurationProperties
        해당 어노테이션은 인자에 prefix 속성을 선언(지정)할 수 있는데요. prefix는 접두사, 즉 머리를 의미합니다.prefix에 spring.datasource.hikari를 선언했습니다. 쉽게 말해 @PropertySource에 선언된 파일(application.properties)에서 prefix에 해당하는 spring.datasource.hikari로 시작하는 설정을 모두 읽어 들여 해당 메서드에 매핑(바인딩)하는 개념입니다.추가적으로 해당 어노테이션은 메서드뿐만 아니라 클래스 레벨에도 선언할 수 있습니다.


        hikariConfig
        히카리CP 객체를 생성합니다. 히카리CP는 커넥션 풀(Connection Pool) 라이브러리 중 하나입니다.


        dataSource
        데이터 소스 객체를 생성합니다. 순수 JDBC는 SQL을 실행할 때마다 커넥션을 맺고 끊는 I/O 작업을 하는데, 이 작업은 상당한 양의 리소스를 잡아먹는다고 합니다. 그리고, 이 문제의 해결책으로 커넥션 풀이 등장했습니다.커넥션 풀은 커넥션 객체를 생성해두고, DB에 접근하는 사용자에게 미리 생성해둔 커넥션을 제공했다가 다시 돌려받는 방법입니다.데이터 소스는 커넥션 풀을 지원하기 위한 인터페이스입니다.


        sqlSessionFactory
        SqlSessionFactory 객체를 생성합니다. SqlSessionFactory는 DB 커넥션과 SQL 실행에 대한 모든 것을 갖는 객체입니다.SqlSessionFactoryBean은 FactoryBean 인터페이스의 구현 클래스로, 마이바티스(MyBatis)와 스프링의 연동 모듈로 사용됩니다.쉽게 말해 factoryBean 객체는 데이터 소스를 참조하며, XML Mapper(SQL 쿼리 작성 파일)의 경로와 설정 파일 경로 등의 정보를 갖는 객체입니다.


        sqlSession
        sqlSession 객체를 생성합니다. 마이바티스 공식 문서에는 다음과 같이 정의되어 있습니다.1. SqlSessionTemplate은 마이바티스 스프링 연동 모듈의 핵심이다.2. SqlSessionTemplate은 SqlSession을 구현하고, 코드에서 SqlSession을 대체한다.3. SqlSessionTemplate은 쓰레드에 안전하고, 여러 개의 DAO나 Mapper에서 공유할 수 있다.4. 필요한 시점에 세션을 닫고, 커밋 또는 롤백하는 것을 포함한 세션의 생명주기를 관리한다.SqlSessionTemplate은 SqlSessionFactory를 통해 생성되고, 공식 문서의 내용과 같이 DB의 커밋, 롤백 등 SQL의 실행에 필요한 모든 메서드를 갖는 객체로 생각할 수 있습니다.

<p>$\bf{\large{\color{#6580DD}3. JUnit으로 단위 테스트 해보기}}$</p> 
스프링은 단위 테스트를 위한 환경과 다양한 기능들을 아낌없이 제공해주고 있습니다. 일반적으로 단위 테스트는 비즈니스 로직 또는 SQL 쿼리에 문제가 있는지 확인하는 용도로 사용되는데 WAS(톰캣)를 구동하지 않은 상태에서도 테스트가 가능하기 때문에 시간적인 측면에서 상당히 유리합니다.

3-1) 소스 코드 작성하기
여기서는 DatabaseConfig 클래스에 구성한 빈(Bean)을 기준으로 JUnit 단위 테스트 방법입니다. src/test/java 디렉터리의 BoardApplicationTests에 다음의 코드를 작성

        package com.study;

        import org.apache.ibatis.session.SqlSessionFactory;
        import org.junit.jupiter.api.Test;
        import org.springframework.beans.factory.annotation.Autowired;
        import org.springframework.boot.test.context.SpringBootTest;
        import org.springframework.context.ApplicationContext;

        @SpringBootTest
        class BoardApplicationTests {

            @Autowired
            private ApplicationContext context;

            @Autowired
            private SqlSessionFactory sessionFactory;

            @Test
            void contextLoads() {
            }

            @Test
            public void testByApplicationContext() {
                try {
                    System.out.println("=========================");
                    System.out.println(context.getBean("sqlSessionFactory"));
                    System.out.println("=========================");

                } catch (Exception e) {
                    e.printStackTrace();
                }
            }

            @Test
            public void testBySqlSessionFactory() {
                try {
                    System.out.println("=========================");
                    System.out.println(sessionFactory.toString());
                    System.out.println("=========================");

                } catch (Exception e) {
                    e.printStackTrace();
                }
            }

        }

3-2)소스코드 결과
테스트에 성공한다면 다음과 같이 주소값이 나오게 됩니다.

![캡처](https://github.com/asdfwoomin/woomin/assets/154343478/3b678d10-3d24-4ac7-9d42-ddb7aa74a720)

</details>


<details>
<summary>
  <img src="" alt="" width="2%" /> 게시판 CRUD 처리하기
</summary>
   <br>

<p>$\bf{\large{\color{#6580DD}1. 게시글 테이블 생성하기}}$</p> 


         CREATE TABLE `tb_post` (
            `id`            bigint(20)    NOT NULL AUTO_INCREMENT COMMENT 'PK',
            `title`         varchar(100)  NOT NULL COMMENT '제목',
            `content`       varchar(3000) NOT NULL COMMENT '내용',
            `writer`        varchar(20)   NOT NULL COMMENT '작성자',
            `view_cnt`      int(11)       NOT NULL COMMENT '조회 수',
            `notice_yn`     tinyint(1)    NOT NULL COMMENT '공지글 여부',
            `delete_yn`     tinyint(1)    NOT NULL COMMENT '삭제 여부',
            `created_date`  datetime      NOT NULL DEFAULT current_timestamp() COMMENT '생성일시',
            `modified_date` datetime               DEFAULT NULL COMMENT '최종 수정일시',
            PRIMARY KEY (`id`)
        ) COMMENT '게시글'; 


<p>$\bf{\large{\color{#6580DD}2. 요청 클래스 생성 및 소스 코드 작성하기}}$</p> 
게시글 생성(INSERT)과 수정(UPDATE)에 사용할 요청(Request) 클래스

         package com.study.domain.post;

         import lombok.Getter;
         import lombok.Setter;

         @Getter
         @Setter
         public class PostRequest {

             private Long id;             // PK
             private String title;        // 제목
             private String content;      // 내용
             private String writer;       // 작성자
             private Boolean noticeYn;    // 공지글 여부
    
         }

@Getter / @Settter
클래스 레벨에 선언된 두 어노테이션은 롬복(Lombok) 라이브러리에서 제공해 주는 기능으로, 클래스에 선언된 모든 멤버 변수에 대한 getter와 settter를 생성해 주는 역할을 합니다.

<p>$\bf{\large{\color{#6580DD}3. 게시글 응답(Response) 클래스 생성하기}}$</p> 
사용자에게 보여줄 데이터를 처리할 응답용 클래스입니다. 응답 클래스에는 테이블의 모든 칼럼을 멤버 변수로 선언합니다.

         package com.study.domain.post;

         import lombok.Getter;

         import java.time.LocalDateTime;

         @Getter
         public class PostResponse {

             private Long id;                       // PK
             private String title;                  // 제목
             private String content;                // 내용
             private String writer;                 // 작성자
             private int viewCnt;                   // 조회 수
             private Boolean noticeYn;              // 공지글 여부
             private Boolean deleteYn;              // 삭제 여부
             private LocalDateTime createdDate;     // 생성일시
             private LocalDateTime modifiedDate;    // 최종 수정일시

         }

<p>$\bf{\large{\color{#6580DD}4. Mapper 인터페이스 생성하기}}$</p> 


         package com.study.domain.post;

         import org.apache.ibatis.annotations.Mapper;

         import java.util.List;

         @Mapper
         public interface PostMapper {

             /**
              * 게시글 저장
              * @param params - 게시글 정보
              */
             void save(PostRequest params);

             /**
              * 게시글 상세정보 조회
              * @param id - PK
              * @return 게시글 상세정보
              */
             PostResponse findById(Long id);
    
             /**
              * 게시글 수정
              * @param params - 게시글 정보
              */
             void update(PostRequest params);

             /**
              * 게시글 삭제
              * @param id - PK
              */
             void deleteById(Long id);

             /**
              * 게시글 리스트 조회
              * @return 게시글 리스트
              */
             List<PostResponse> findAll();

             /**
              * 게시글 수 카운팅
              * @return 게시글 수
              */
             int count();

         }

4-1. @Mapper
MyBatis는 Mapper(Java 인터페이스)와 XML Mapper(실제로 DB에 접근해서 호출할 SQL 쿼리를 작성(선언)하는 파일)를 통해 DB와 통신합니다.
메서드명이 "savePost( )"라고 가정했을 때 SQL id는 "savePost"가 되어야 합니다.
Mapper에는 @Mapper 어노테이션을 필수적으로 선언해 주어야 하며, Mapper와 XML Mapper는 XML Mapper의 namespace라는 속성을 통해 연결됩니다.


4-2. save( )
게시글을 생성하는 INSERT 쿼리를 호출합니다. 
  
  파라미터로 전달받는 params는 요청(PostRequest) 클래스의 객체이며, params에는 저장할 게시글 정보가 담기게 됩니다.
 
 
4-3. findById( )
특정 게시글을 조회하는 SELECT 쿼리를 호출합니다.

  파라미터로 id(PK)를 전달받아 SQL 쿼리의 WHERE 조건으로 사용하며, 쿼리가 실행되면 메서드의 리턴 타입인 응답(PostResponse) 클래스 객체의 각 멤버 변수에 결괏값이 매핑(바인딩)됩니다.
 

 
4-4. update( )
게시글 정보를 수정하는 UPDATE 쿼리를 호출합니다.

  save( )와 마찬가지로 요청(PostRequest) 클래스의 객체를 파라미터로 전달받으며, params에는 수정할 게시글 정보가 담기게 됩니다. save( )와 차이가 있다면, UPDATE 쿼리의 WHERE 조건으로 사용되는 id(PK)에도 값이 담긴다는 점입니다.
 

 
4-5. deleteById( )
게시글을 삭제 처리하는 UPDATE 쿼리를 호출합니다.

  findById( )와 마찬가지로 id(PK)를 파라미터로 전달받아 SQL 쿼리의 WHERE 조건으로 사용하게 되며, SQL 쿼리가 실행되면 삭제 여부(delete_yn) 칼럼의 상태 값을 0(false)에서 1(true)로 업데이트합니다.
삭제 여부(delete_yn)는 칼럼의 상태 값을 기준으로 삭제된 데이터(1)인지, 삭제되지 않은 데이터(0)인지 구분해 주는 역할을 합니다. 사용자에게 데이터를 보여줄 땐 삭제 여부가 0(false)인 데이터만 노출하게 됩니다.
 


4-6. findAll( )
게시글 목록을 조회하는 SELECT 쿼리를 호출합니다.

  findById( )는 id(PK)를 기준으로 하나의 게시글을 조회한다면, 해당 메서드는 여러 개의 게시글(PostResponse)을 리스트(List)에 담아 리턴해주는 역할을 합니다.
 
4-7. count( )
  전체 게시글 수를 조회하는 SELECT 쿼리를 호출합니다. 



<p>$\bf{\large{\color{#6580DD}5. mappers 폴더와 XML Mapper 추가하기}}$</p> 

   
 src/main/resources에 mappers 폴더를 추가하고, 그 안에 PostMapper.xml을 추가합니다.
 소스는 다음과 같습니다.

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.study.domain.post.PostMapper">

    <!-- tb_post 테이블 전체 컬럼 -->
    <sql id="postColumns">
          id
        , title
        , content
        , writer
        , view_cnt
        , notice_yn
        , delete_yn
        , created_date
        , modified_date
    </sql>


    <!-- 게시글 저장 -->
    <insert id="save" parameterType="com.study.domain.post.PostRequest">
        INSERT INTO tb_post (
            <include refid="postColumns" />
        ) VALUES (
              #{id}
            , #{title}
            , #{content}
            , #{writer}
            , 0
            , #{noticeYn}
            , 0
            , NOW()
            , NULL
        )
    </insert>


    <!-- 게시글 상세정보 조회 -->
    <select id="findById" parameterType="long" resultType="com.study.domain.post.PostResponse">
        SELECT
            <include refid="postColumns" />
        FROM
            tb_post
        WHERE
            id = #{value}
    </select>


    <!-- 게시글 수정 -->
    <update id="update" parameterType="com.study.domain.post.PostRequest">
        UPDATE tb_post
        SET
              modified_date = NOW()
            , title = #{title}
            , content = #{content}
            , writer = #{writer}
            , notice_yn = #{noticeYn}
        WHERE
            id = #{id}
    </update>


    <!-- 게시글 삭제 -->
    <delete id="deleteById" parameterType="long">
        UPDATE tb_post
        SET
            delete_yn = 1
        WHERE
            id = #{id}
    </delete>


    <!-- 게시글 리스트 조회 -->
    <select id="findAll" resultType="com.study.domain.post.PostResponse">
        SELECT
            <include refid="postColumns" />
        FROM
            tb_post
        WHERE
            delete_yn = 0
        ORDER BY
            id DESC
    </select>

</mapper>


5-1. <mapper> 태그
XML Mapper는 <mapper>로 시작해서 </mapper>로 끝나며, <mapper> 태그의 namespace 속성에 Mapper 인터페이스의 경로를 선언해 주면 Mapper와 XML Mapper가 연결됩니다.


 
5-2. <sql> 태그와 <include> 태그
MyBatis는 <sql> 태그와 <include> 태그를 이용해서 공통으로 사용되거나 반복적으로 사용되는 쿼리를 처리할 수 있습니다. 
각각의 쿼리에 전체 칼럼을 선언해 줘도 되지만, 해당 태그들을 이용하면 코드 라인을 줄일 수 있습니다. 두 태그의 포인트는 중복 제거이며, 동일한 XML Mapper뿐만 아니라, 다른 XML Mapper에 선언된 SQL 조각도 인클루드(Include) 할 수 있습니다.

 
5-3. parameterType
SQL 쿼리 실행에 필요한 파라미터의 타입을 의미합니다. 단일(하나의) 파라미터가 아닌 경우에는 일반적으로 객체를 전달받아 쿼리를 실행합니다.
 

 
5-4. resultType
SQL 쿼리의 실행 결과를 매핑할 결과 타입을 의미합니다. Mapper 인터페이스에 선언한 메서드의 리턴 타입과 동일한 타입으로 선언해 주시면 됩니다.
 

 
5-5. #{ } 표현식
MyBatis는 #{ 변수명 } 표현식을 이용해서 전달받은 파라미터를 기준으로 쿼리를 실행합니다.


<p>$\bf{\large{\color{#6580DD}6. 칼럼과 멤버 변수 매핑하기}}$</p> 


   
MyBatis에서 SELECT 한 결괏값은 응답(Response) 클래스의 멤버 변수와 매핑되어야 합니다. 그러나 DB에서 테이블의 칼럼명은 언더스코어(_)로 연결된 스네이크 케이스를 사용하며, 자바에서 변수명은 소문자로 시작하고, 구분되는 단어의 앞 글자만 대문자로 처리하는 카멜 케이스를 사용합니다.
이럴때 application.properties에 다음의 설정을 추가하면 됩니다.

      
      mybatis.configuration.map-underscore-to-camel-case=true

<p>$\bf{\large{\color{#6580DD}7. DatabaseConfig 클래스 수정하기}}$</p> 
스프링이 properties에서 MyBatis 설정을 읽을 수 있도록 빈(Bean)을 선언해 주어야 합니다.
DatabaseConfig 소스는 다음과 같습니다.



         package com.study.config;

         import com.zaxxer.hikari.HikariConfig;
   
         import com.zaxxer.hikari.HikariDataSource;
   
         import org.apache.ibatis.session.SqlSessionFactory;
   
         import org.mybatis.spring.SqlSessionFactoryBean;
   
         import org.mybatis.spring.SqlSessionTemplate;
   
         import org.springframework.beans.factory.annotation.Autowired;
   
         import org.springframework.boot.context.properties.ConfigurationProperties;
   
         import org.springframework.context.ApplicationContext;
   
         import org.springframework.context.annotation.Bean;
   
         import org.springframework.context.annotation.Configuration;
   
         import org.springframework.context.annotation.PropertySource;

         import javax.sql.DataSource;

         @Configuration
         @PropertySource("classpath:/application.properties")
         public class DatabaseConfig {

        @Autowired
        private ApplicationContext context;

        @Bean
        @ConfigurationProperties(prefix = "spring.datasource.hikari")
        public HikariConfig hikariConfig() {
            return new HikariConfig();
        }

        @Bean
        public DataSource dataSource() {
            return new HikariDataSource(hikariConfig());
        }

        @Bean
        public SqlSessionFactory sqlSessionFactory() throws Exception {
            SqlSessionFactoryBean factoryBean = new SqlSessionFactoryBean();
            factoryBean.setDataSource(dataSource());
            factoryBean.setMapperLocations(context.getResources("classpath:/mappers/**/*Mapper.xml"));
            factoryBean.setConfiguration(mybatisConfig());
            return factoryBean.getObject();
        }

        @Bean
        public SqlSessionTemplate sqlSession() throws Exception {
            return new SqlSessionTemplate(sqlSessionFactory());
        }

        @Bean
        @ConfigurationProperties(prefix = "mybatis.configuration")
        public org.apache.ibatis.session.Configuration mybatisConfig() {
            return new org.apache.ibatis.session.Configuration();
        }

         }


<p>$\bf{\large{\color{#6580DD}8. CRUD 테스트해보기}}$</p> 

8-1. 테스트 클래스 추가 & 코드 작성하기
src/test/java의 com.study 패키지에 PostMapperTest 클래스를 추가하고, 코드 작성

    package com.study;

    import com.study.domain.post.PostMapper;
    import com.study.domain.post.PostRequest;
    import com.study.domain.post.PostResponse;
    import org.junit.jupiter.api.Test;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.boot.test.context.SpringBootTest;

    import java.util.List;

    @SpringBootTest
    public class PostMapperTest {

        @Autowired
        PostMapper postMapper;

        @Test
        void save() {
            PostRequest params = new PostRequest();
            params.setTitle("1번 게시글 제목");
            params.setContent("1번 게시글 내용");
            params.setWriter("테스터");
            params.setNoticeYn(false);
            postMapper.save(params);

            List<PostResponse> posts = postMapper.findAll();
            System.out.println("전체 게시글 개수는 : " + posts.size() + "개입니다.");
        }

    }



    

8-1-1. postMapper
@Autowired를 이용해서 스프링 컨테이너에 등록된 PostMapper 빈(Bean)을 클래스에 주입합니다.
 



 
8-1-2. save( )
게시글을 생성하는 메서드입니다. PostRequest 객체를 생성하고, set( ) 메서드를 이용해 값을 세팅한 후 PostMapper의 save( )를 호출합니다. 메서드가 호출되면 PostMapper.xml의 save 쿼리가 실행되며, #{ 변수명 } 표현식을 통해 PostRequest 객체의 멤버 변수에 접근하게 됩니다.




8-2. findById( ) 테스트하기
테이블의 PK인 id를 WHERE 조건으로 특정 게시글을 조회하는 findById( )입니다.

        @Test
        void findById() {
            PostResponse post = postMapper.findById(1L);
            try {
                String postJson = new ObjectMapper().registerModule(new JavaTimeModule()).writeValueAsString(post);
                System.out.println(postJson);

            } catch (JsonProcessingException e) {
                throw new RuntimeException(e);
            }
        }

8-2-1. postJson
스프링 부트에 기본으로 내장되어 있는 Jackson 라이브러리를 이용해서,  응답 객체를 JSON 문자열로 변환한 결과입니다

8-3. update( ) 테스트하기
기존에 등록된 게시글 정보를 수정하는 update( )입니다.

        @Test
        void update() {
            // 1. 게시글 수정
            PostRequest params = new PostRequest();
            params.setId(1L);
            params.setTitle("1번 게시글 제목 수정합니다.");
            params.setContent("1번 게시글 내용 수정합니다.");
            params.setWriter("도뎡이");
            params.setNoticeYn(true);
            postMapper.update(params);

            // 2. 게시글 상세정보 조회
            PostResponse post = postMapper.findById(1L);
            try {
                String postJson = new ObjectMapper().registerModule(new JavaTimeModule()).writeValueAsString(post);
                System.out.println(postJson);

            } catch (JsonProcessingException e) {
                throw new RuntimeException(e);
            }
        }

8-4. delete( ) 테스트하기

 게시글을 삭제 처리하는 delete( )입니다.
 
          @Test
          void delete() {
              System.out.println("삭제 이전의 전체 게시글 개수는 : " + postMapper.findAll().size() + "개입니다.");
              postMapper.deleteById(1L);
              System.out.println("삭제 이후의 전체 게시글 개수는 : " + postMapper.findAll().size() + "개입니다.");
          }


</details>   

