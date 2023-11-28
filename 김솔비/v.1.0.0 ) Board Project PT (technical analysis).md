# vs.0.1 ) Board Project PT (**technical analysis**)

논리구조 스터디 첫 번째 시간입니다! 😊  
저는 이전에 만든 **JPA로 구현한 게시판**을 주제로   
지난 프로젝트를 다시 뜯어보고 파악하는 시간을 가져보려 합니다:)


본론에 앞서 개발 스택과 자세히 알아볼 주요 키워드 목록과 파일구조를 아래에서 확인 가능합니다. 

2023-11-28 간단하게 오늘은 JPA 데이터 흐름도 즉, 데이터의 요청과 응답에 대해 이야기 해보려 합니다. 


## 🧩 Technical stack

```java
spring Boot 2.7.0 
JAVA : 17 
```


## 🧩 Analysis  List

```java
H2DATABASE 
QueryDSL
Qclass
QAuth
test code 
JPA
```


## 🧩 File Tree
<details>
<summary>토글 접기/펼치기</summary>
<div markdown="1">

```java
.
├── main
│   ├── generated
│   │   └── com
│   │       └── example
│   │           └── projectboard
│   │               └── domain
│   │                   ├── QArticle.java
│   │                   ├── QArticleComment.java
│   │                   ├── QAuditingFields.java
│   │                   ├── QHashtag.java
│   │                   └── QUserAccount.java
│   ├── java
│   │   └── com
│   │       └── example
│   │           └── projectboard
│   │               ├── ProjectBoardApplication.java
│   │               ├── config
│   │               │   ├── DataRestConfig.java
│   │               │   ├── JpaConfig.java
│   │               │   ├── SecurityConfig.java
│   │               │   └── ThymeleafConfig.java
│   │               ├── controller
│   │               │   ├── ArticleCommentController.java
│   │               │   ├── ArticleController.java
│   │               │   └── MainController.java
│   │               ├── domain
│   │               │   ├── Article.java
│   │               │   ├── ArticleComment.java
│   │               │   ├── AuditingFields.java
│   │               │   ├── Hashtag.java
│   │               │   ├── UserAccount.java
│   │               │   ├── constant
│   │               │   │   ├── FormStatus.java
│   │               │   │   └── SearchType.java
│   │               │   └── projection
│   │               │       ├── ArticleCommentProjection.java
│   │               │       ├── ArticleProjection.java
│   │               │       └── UserAccountProjection.java
│   │               ├── dto
│   │               │   ├── ArticleCommentDto.java
│   │               │   ├── ArticleDto.java
│   │               │   ├── ArticleWithCommentsDto.java
│   │               │   ├── HashtagDto.java
│   │               │   ├── HashtagWithArticlesDto.java
│   │               │   ├── UserAccountDto.java
│   │               │   ├── request
│   │               │   │   ├── ArticleCommentRequest.java
│   │               │   │   └── ArticleRequest.java
│   │               │   ├── response
│   │               │   │   ├── ArticleCommentResponse.java
│   │               │   │   ├── ArticleResponse.java
│   │               │   │   └── ArticleWithCommentsResponse.java
│   │               │   └── security
│   │               │       ├── BoardPrincipal.java
│   │               │       └── KakaoOAuth2Response.java
│   │               ├── repository
│   │               │   ├── ArticleCommentRepository.java
│   │               │   ├── ArticleRepository.java
│   │               │   ├── HashtagRepository.java
│   │               │   ├── UserAccountRepository.java
│   │               │   └── querydsl
│   │               │       ├── ArticleRepositoryCustom.java
│   │               │       ├── ArticleRepositoryCustomImpl.java
│   │               │       ├── HashtagRepositoryCustom.java
│   │               │       └── HashtagRepositoryCustomImpl.java
│   │               └── service
│   │                   ├── ArticleCommentService.java
│   │                   ├── ArticleService.java
│   │                   ├── HashtagService.java
│   │                   ├── PaginationService.java
│   │                   └── UserAccountService.java
│   └── resources
│       ├── application.yml
│       ├── data.sql
│       ├── static
│       │   ├── css
│       │   │   ├── articles
│       │   │   │   ├── article-content.css
│       │   │   │   └── table-header.css
│       │   │   └── search-bar.css
│       │   └── images
│       │       └── kakao_login_medium.png
│       └── templates
│           ├── articles
│           │   ├── detail.html
│           │   ├── detail.th.xml
│           │   ├── form.html
│           │   ├── form.th.xml
│           │   ├── index.html
│           │   ├── index.th.xml
│           │   ├── search-hashtag.html
│           │   └── search-hashtag.th.xml
│           ├── footer.html
│           ├── header.css
│           ├── header.html
│           ├── header.th.xml
│           ├── index.html
│           └── sign-up.html
└── test
    └── java
        └── com
            └── example
                └── projectboard
                    ├── ProjectBoardApplicationTest.java
                    ├── config
                    │   └── TestSecurityConfig.java
                    ├── controller
                    │   ├── ArticleCommentControllerTest.java
                    │   ├── ArticleControllerTest.java
                    │   ├── AuthControllerTest.java
                    │   ├── DataRestTest.java
                    │   └── MainControllerTest.java
                    ├── dto
                    │   ├── response
                    │   │   └── ArticleWithCommentsResponseTest.java
                    │   └── security
                    │       └── KakaoOAuth2ResponseTest.java
                    ├── repository
                    │   └── JpaRepositoryTest.java
                    ├── service
                    │   ├── ArticleCommentServiceTest.java
                    │   ├── ArticleServiceTest.java
                    │   ├── HashtagServiceTest.java
                    │   ├── PaginationServiceTest.java
                    │   └── UserAccountServiceTest.java
                    └── util
                        ├── FormDataEncoder.java
                        └── FormDataEncoderTest.java
```
<div markdown="1">
</details>   

<br>
<br>

# JPA **logic architecture**

![image](https://github.com/solfany/solfany.github.io/assets/123814718/a4623378-4783-4baa-9316-223529b733fd)



# 계층도

## API Layer 계층

> **API 계층**: 사용자의 요청을 받고 응답을 반환하는 계층

1. 클라이언트 요청: 사용자의 브라우저나 앱 등 클라이언트가 서버에 HTTP 요청을 보고 응답을 받게된다. 사용자의 인터페이스 레벨의 요청과 응답을 처리하게 된다.

2.  컨트롤러(Controller): Controller 클래스가 <U>클라이언트의 HTTP 요청을 받고, 특정 요청에 대해 어떤 처리를 해야 할지 결정하는 엔드포인트</U> 를 정의한다. (GET,POST,PUT, DELETE)

3.  DTO(Data Transfer Object): 계층 간 데이터 전송에 사용된다.
    클라이언트로부터 받은 데이터를 서버 내부로 전달하거나, 서버에서 처리한 데이터를 클라이언트에게 응답하는데 사용된다.   
    Entity의 데이터를 클라이언트에게 직접 노출하지 않고, <U>필요한 데이터만을 전송하는 데 유용하다.</U>




## Service 계층

> **서비스 계층**: 애플리케이션의 비즈니스 로직을 처리하는 계층

4. 서비스(Service): 컨트롤러는 비즈니스 로직을 수행하기 위해 service 폴더 내에 정의된Service 클래스의 메소드를 호출한다.   
이곳에서 <U>비즈니스 규칙을 처리하고, 데이터의 유효성 검사, 비즈니스 규칙의 적용, 트랜잭션 관리 </U> 등을 수행한다.

5. 도메인/엔티티: <U> 비즈니스 데이터와 관련된 로직을 표현하는 객체</U> 들이다.
   데이터베이스의 테이블과 매핑되어 CRUD 작업에 사용된다.
   서비스 계층에서는 엔티티를 사용하여 비즈니스 로직을 수행한다.
  

## Data Access Layer 계층

> **데이터 액세스 계층**: 데이터베이스와의 상호작용을 담당하는 계층

6. 레포지토리(Repository): 서비스 계층은 데이터를 <U>영속화</U> 하거나 데이터베이스로부터   
데이터를 조회하기 위해 repository 폴더 내의 Repository 인터페이스를 사용한다.  
데이터베이스의 각 테이블에 대응하는 엔티티 클래스의 CRUD 작업을 처리한다.  
데이터 액세스 계층은 <U>데이터베이스 연산을 추상화하고,  
비즈니스 로직과 데이터베이스의 세부 구현 사이를 분리</U> 한다.

<br>


# JPA Request and Response


### 요청 데이터 흐름도 (Client to Server)

1. **클라이언트 요청**: 사용자가 게시판에 글을 작성하거나 읽는 등의 요청을 보낸다.

2. **컨트롤러**: 요청을 받아 해당 서비스 로직을 호출한다.

3. **서비스 계층**: 비즈니스 로직을 수행한다. 예를 들어, 게시글 생성, 조회 등의 작업을 담당한다.

4. **레파지토리 계층 (JPA 사용)**: 게시글에 대한 CRUD 작업을 JPA를 통해 처리한다.
   Entity 객체를 사용하여 데이터베이스와의 상호 작용을 수행한다.

5. **데이터베이스(DB)**: 실제 데이터가 저장, 조회, 수정, 삭제되는 장소이다.

### 응답 데이터 흐름도 (Server to Client)

1. **데이터베이스(DB)**: 처리 결과를 반환한다.
2. **레파지토리 계층 (JPA 사용)**: JPA를 통해 데이터베이스로부터 데이터를 검색하고,
   이를 Entity 객체로 변환한다.
3. **서비스 계층**: 비즈니스 로직에 따라 데이터를 처리하고, 필요한 경우 DTO(Data Transfer Object)로 변환한다.
4. **컨트롤러**: 클라이언트에 반환할 응답을 준비한다.
5. **클라이언트**: 최종적으로 사용자에게 데이터가 전달된다 (예: 게시글 목록, 게시글 내용 등).

<br>

# Entity 와 Domain


의문점이 생겨 한번 찾아보았다.  
개발에서 <U>엔티티(Entity)</U>와 <U>도메인(Domain)</U> 이라는 용어는 종종 같은 맥락에서 사용된다.  
그러나, 이 두 용어의 의미에는 약간의 차이가 있을 수 있다.

1. **엔티티 (Entity)**:
   - JPA(Java Persistence API)와 같은 ORM(Object-Relational Mapping) 컨텍스트에서, 엔티티는 데이터베이스의 테이블을 반영하는 Java 클래스를 의미한다.
   - 이 클래스들은 **`@Entity`** 어노테이션으로 표시되며, 데이터베이스의 행(row)과 객체 간의 매핑을 정의한다.
   - 엔티티는 데이터베이스와의 상호작용을 위해 사용되며, 보통 ID, 열(column) 매핑, 관계 매핑 등을 포함한다.
2. **도메인 (Domain)**:
   - 보다 넓은 의미에서, 도메인은 특정 비즈니스 로직이나 요구사항을 모델링하는 객체의 집합을 의미한다.
   - 도메인 모델은 해당 비즈니스의 규칙, 작업, 엔티티, 관계 등을 포함하여, 비즈니스의 문제 영역을 표현한다.
   - 때때로, "도메인 객체"는 엔티티와 동일한 의미로 사용되기도 하지만, 좀 더 넓은 범위의 비즈니스 로직과 규칙을 포함할 수도 있다.

### 요약

- **엔티티**: 데이터베이스 테이블과 직접적으로 매핑되는 Java 클래스, 주로 데이터의 저장과 검색에 사용된다.
- **도메인**: 비즈니스 로직과 요구사항을 모델링하는 더 넓은 개념으로, 엔티티를 포함할 수 있다.  
   
   <br>

_따라서, 특정 컨텍스트(예: JPA 사용 시)에서 엔티티와 도메인은 같은 클래스를 가리키는 경우가 많지만, 도메인은 보다 광범위한 비즈니스 로직과 규칙을 표현하는 데 사용되는 개념이라고 할 수 있다._

<br>
<br>

# JPA 사용 vs 사용하지 않았을 때의 차이점


## JPA 사용

1. **객체-관계 매핑 (ORM)**: <U>JPA는 ORM(Object-Relational Mapping)을 제공 하여, 객체와 관계형 데이터베이스 테이블 간의 매핑을 쉽게 해준다.</U>  
이를 통해 개발자는 객체 중심적으로 데이터를 다룰 수 있게 되며, SQL을 직접 작성하는 <U>복잡성을 줄일 수 있다.</U>

2. **데이터베이스 독립성**: JPA는 데이터베이스에 <U>독립적인 프로그래밍</U> 이 가능하게 해주어, 다양한 데이터베이스 시스템에 적응할 수 있다.

3. **표준화된 API**: JPA는 Java의 표준 ORM API로, 다양한 JPA 구현체를 사용할 수 있으며 이식성이 높다.

4. **자동화된 데이터 처리**: CRUD 작업과 같은 기본적인 데이터 처리 로직을 JPA가 자동으로 처리해 준다.

## JPA를 사용하지 않았을 때

1. **직접적인 SQL 작업**: 데이터베이스와의 모든 상호 작용을 SQL을 통해 직접 처리해야 한다.  
이는 데이터베이스 스키마와 밀접하게 연관되어 있어, <U>객체 중심의 개발이 어려울 수 있다.</U>

2. **데이터베이스 종속성**: 특정 데이터베이스에 맞춘 SQL 작성이 필요하므로, <U>다른 데이터베이스로의 전환 시 코드의 변경이 필요할 수 있다.</U>

3. **수동적인 데이터 처리**: CRUD 및 트랜잭션 관리 등을 모두 <U>개발자가 수동으로 처리해야 한다.</U>  

<br>

# 회고
너무 기본적인 구조이고, 개발자라면 필수적으로 알아야 할 부분이라고 생각한다.  
프로젝트형 학습만 하다 보니, 세세한 키워드 하나하나가 어떤 의미로 어떤 기능을 수행하는지 해당 스터디를 하면서 학습 할 계획이다.  
데이터 흐름도를 아키텍쳐로 그려보면서 학습하다 보니 기억에도 더 오래 남는 것 같아서 좋았다.  
또 JPA 프로젝트에서의 쓰이는 domain과 entity 의 차이점을 명확하게 알게된 부분이 기억에 남는다. 


<br>

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}