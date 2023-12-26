
스프링부트로 게시판 만들기 (2023.12.18~진행중)

![캡처](https://github.com/asdfwoomin/woomin/assets/154343478/d2dffb55-31b5-4d04-b385-a3110cb57974)


1. src/main/java 디렉터리
스프링 레거시와 마찬가지로 클래스, 인터페이스 등 Java 관련 파일이 위치하는 디렉터리입니다.

2. BoardApplication 클래스
이전 글에서 생성한 Board 프로젝트의 com.study 패키지에는 우리가 생성하지 않은 BoardApplication 클래스가 포함되어 있습니다. 파일을 열어보면 메서드 선언부에는 딸랑 main( ) 메서드 하나만 선언되어 있는데요. main( ) 메서드는 SpringApplication.run( )을 호출해서 웹 애플리케이션을 실행하는 역할을 합니다.
 
 
다음은 클래스 레벨에 선언된 @SpringBootAplication 어노테이션입니다. 해당 어노테이션은 다음의 세 가지 어노테이션으로 구성되어 있습니다.

@EnableAutoConfiguration
스프링 부트는 개발에 필요한 몇 가지 필수적인 설정들의 처리가 되어 있으며, 해당 애너테이션에 의해 다양한 설정들의 일부가 자동으로 완료됩니다.

@ComponentScan
XML 설정 방식을 이용하는 스프링 레거시는 빈(Bean)의 등록 및 스캔을 위해, 수동으로 ComponentScan을 여러 개 선언하는 방식을 사용했었습니다.스프링 부트는 해당 어노테이션에 의해 자동으로 컴포넌트 클래스를 검색하고, 스프링 애플리케이션 콘텍스트(IoC 컨테이너)에 빈(Bean)으로 등록합니다. 쉽게 말해, 의존성 주입(DI) 과정이 더욱 간편해졌다고 생각할 수 있습니다.

@Configuration
해당 어노테이션이 선언된 클래스는 Java 기반의 설정 파일로 인식됩니다. 스프링 4 버전부터 Java 기반의 설정이 가능하게 되었으며, XML 설정에 큰 시간을 소모하지 않아도 됩니다.

1-1) 테이블 생성 스크립트 실행하기

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

