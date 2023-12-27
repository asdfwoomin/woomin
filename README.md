
스프링부트로 게시판 만들기 (2023.12.18~진행중)


<details>
<summary>
  <img src="" alt="" width="2%" /> 게시판 프로젝트 구조
</summary>
   <br>


![캡처](https://github.com/asdfwoomin/woomin/assets/154343478/d2dffb55-31b5-4d04-b385-a3110cb57974)



  

1. src/main/java 디렉터리
스프링 레거시와 마찬가지로 클래스, 인터페이스 등 Java 관련 파일이 위치하는 디렉터리입니다.

2. BoardApplication 클래스
이전 글에서 생성한 Board 프로젝트의 com.study 패키지에는 우리가 생성하지 않은 BoardApplication 클래스가 포함되어 있습니다.  main( ) 메서드는 SpringApplication.run( )을 호출해서 웹 애플리케이션을 실행하는 역할을 합니다.
 다음은 클래스 레벨에 선언된 @SpringBootAplication 어노테이션입니다. 해당 어노테이션은 다음의 세 가지 어노테이션으로 구성되어 있습니다.

        2-1. @EnableAutoConfiguration
        스프링 부트는 개발에 필요한 몇 가지 필수적인 설정들의 처리가 되어 있으며, 해당 애너테이션에 의해 다양한 설정들의 일부가 자동으로 완료됩니다.

        2-2. @ComponentScan
        XML 설정 방식을 이용하는 스프링 레거시는 빈(Bean)의 등록 및 스캔을 위해, 수동으로 ComponentScan을 여러 개 선언하는 방식을 사용했었습니다.스프링 부트는 해당 어노테이션에 의해 자동으로 컴포넌트 클래스를 검색하고, 스프링 애플리케이션 콘텍스트(IoC 컨테이너)에 빈(Bean)으로 등록합니다. 쉽게 말해, 의존성 주입(DI) 과정이 더욱 간편해졌다고 생각할 수 있습니다.

        2-3. @Configuration
        해당 어노테이션이 선언된 클래스는 Java 기반의 설정 파일로 인식됩니다. 스프링 4 버전부터 Java 기반의 설정이 가능하게 되었으며, XML 설정에 큰 시간을 소모하지 않아도 됩니다.

3. src/main/resources 디렉터리
스프링 레거시는 프로젝트가 생성되었을 때 해당 디렉터리에 log4.xml 파일만  생성되었습니다. 스프링 부트는 templates 폴더, static 폴더, application.properties 파일이 기본적으로 생성됩니다.

        3-1. templates
        스프링 레거시는 HTML 내에 Java 코드를 삽입하는 방식인 JSP를 주로 사용했었습니다. 하지만, 스프링 공식 문서에서는 View(화면) 영역에서 JSP가 아닌 타임리프(Thymeleaf) 템플릿 엔진의 사용을 권장하고 있습니다.타임리프는 JSP와 마찬가지로 HTML 내에서 Java 영역의 데이터를 처리하는 데 사용됩니다. 문법 또한 JSTL과 유사하기에(큰 차이가 없음), 결론적으로 해당 디렉터리에는 타임리프 관련 파일이 위치하게 되고, 타임리프는 HTML5 기반이기 때문에 HTML 파일로 화면을 구성합니다.

        3-2. static
        해당 폴더에는 css, fonts, images, plugin, scripts 등의 정적 리소스 파일이 위치합니다.

        3-3. application.properties
        해당 파일은 웹 애플리케이션을 실행하면서 자동으로 로딩되는 파일입니다. 예를 들어, 부트에 내장된 톰캣의 포트 번호, 콘텍스트 패스(Context Path) 설정이나, 데이터베이스 관련 정보 등 애플리케이션에서 사용하는 여러가지 설정을 해당 파일에 Key - Value 형식으로 선언해서 사용할 수 있습니다.선언한 속성은 일반적으로 설정(Configuration) 파일에서 사용합니다.


4. src/test/java 디렉터리
해당 디렉터리의 com.study 패키지에는 BoardApplicationTests 클래스가 생성되어 있습니다. 해당 클래스를 이용해서 개발 단계별로 단위 테스트를 진행하게 되며, 스프링 레거시와는 달리 복잡한 설정 없이 곧바로 테스트가 가능합니다.
 
5. build.gradle
프로젝트를 생성하면서 프로젝트의 빌드 도구를 그레이들(Gradle)로 선택했습니다. 기존의 스프링은 pom.xml에 dependency를 추가해서 라이브러리를 관리하는 방식의 메이븐(Maven)을 이용했었는데 최근에는 메이븐 보다 그레이들을 선호하는 추세라고 합니다. 메이븐은 하나의 라이브러리를 추가하려면 평균적으로 네 줄 이상의 코드를 작성해야 하지만, 그레이들은단 한 줄의 코드로 라이브러리를 추가할 수 있습니다.
 프로젝트에 포함된 모든 라이브러리는 IDE의 External Libraries에서 확인할 수 있습니다.
 
6. MVC 패턴
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




1. 데이터 소스(DataSource) 설정하기
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


2. 데이터 소스 설정(Data Source Configuration) 클래스 추가하기
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

3. JUnit으로 단위 테스트 해보기
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









</details>   

