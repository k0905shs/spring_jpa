# JPA
### JPA
Java Persistence API  
자바 ORM 기술 표준    
어플리케이션과 JDBC사이에서 동작  
<img src="/img/JPA_1.PNG" alt="propagation" border="0">  
특정 DB에 종속되지 않는다.  

  
### ORM
Object-relational mapping  
객체와 관계형 데이터베이스 중간에서 서로 매핑 시켜주는 기술  


### Entity
- 데이터 모델링에서 사용되는 객체, 즉 테이블  
- Entity가 영속되기 위해서는 식별자가 필요하다.  
- 테이블내 각 Row가 Entity 객체이며, 각 Column이 Filed  
- 테이블 내의 모든 Column은 반드시 Field로 정의 해야함  
- Entity클래스는 다른 클래스를 상속받거나 인터페이스의 구현체로 사용이 불가  


### Entity의 라이프 사이클 
- #### 비영속(new/transient) : 
영속성 컨텍스트와 전혀 관계가 없는 새로운 상태  

- #### 영속(mamaged) :   
영속성 컨텍스트에 관리되는 상태  

- #### 준영속(detached) :   
영속성 컨텍스트에 저장되어있다가 분리된 상태  

- #### 삭제(removed) : 
삭제된 상태  

<img src="/img/entity_lifecycle.PNG" alt="propagation" border="0">  


### Persistence Context (영속성 컨텍스트)
* 엔티티를 영구 저장하는 환경  
* 눈에 보이지 않는 논리적인 개념  
* 엔티티 매니저를 통해 접근한다.  


### 영속성 컨텍스트의 이점  
- #### 1차 캐시 :  
 -영속성 컨텍스트에 있는 엔티티를 보관하는 저장소  
 -일반적으로 트랜잭션을 시작하고 종료할 때 까지만 1차 캐시가 유효하다.  
 -같은 엔티티가 있으명 객체 동일성을 보장한다.(== 비교)  
 -네트워크를 통해 DB에 접근하는 시간 비용 보다 훨씬 저렴하다.  



- #### 영속성 엔티티의 동일성 보장 :  
 -1차 캐시로 반복 가능한 읽기(REPEATABLE READ) 등급의 트랜잭션 격리 수준을 DB가 아닌 애플리케이션 차원에서 제공  

- #### 트랜잭션을 지원하는 쓰기 지연 :  
 -한 트랜잭션안에서 이뤄지는 UPDATE나 SAVE의 쿼리를 **쓰기지연 저장소**에 가지고 있다가 트랜잭션이 커밋되는 순간 한번에 DB에 날린다.  
 -DB커넥션 시간을 줄일 수 있고, 한 트랜잭션이 테이블에 접근하는 시간을 줄일 수 있다는 장점이 있다.  

- #### 변경 감지 (Dirty Checking) :
<img src="/img/dirty_checking.png" alt="propagation" border="0">  

- #### 지연 로딩 (Lazy Loading) :  
  
  
### 플러시  
영속성 컨텍스트의 변경내용을 DB에 동기화  
영속성 컨텍스트를 비우지는 않는다.  
**em.flush()**(직접호출), **트랜잭션 커밋**(자동 호출), **JPQL쿼리 실행**(자동 호출) 시점에 플러시가 발생한다.    

### 준영속 상태
영속 상태의 엔티티가 영속성 컨텍스트에서 분리(detached)  
영속성 컨텍스트가 제공하는 기능 사용 불가  
* 준영속 상태로 만드는 방법  
    + em.detach(entity) - 특정 엔티티만 준영속 상태로 전환
    + em.clear() - 영속성 컨텍스트를 완전히 초기화  
    + em.close() - 영속성 컨텍스트 종료  
 

### 엔티티 매핑  
 - @Entity가 붙은 클래스는 JPA가 관리, 엔티티라 한다. 
 - JPA를 사용해서 테이블과 매핑할 클래스는 @Entity 필수
 - 기본 생성자 필수 (public 보다는 protected **지향**)
 - 무분별한 setter 사용 **지양**
 - final 클래스, enum, interface, inner 클래스 사용X
 - 저장할 필드에 final 사용 X
 
### 엔티티 protected 생성자와 setter
* setter 사용 **지양**  :   
    + 사용한 **의도**를 쉽게 파악하기 어렵다.  
    + 여러곳에서 사용될 경우 객체의 **일관성을 유지**하기 어렵다.  
* 해결 방안 :   
    + 사용한 의도나 의미를 알 수 있는 메서드를 작성  
    + 생성자를 통해 값을 넣어 일관성 유지 (ex. @Builder)
  
  
* 기본생성자 protected **지향**  
    + 엔티티에서 setter 를 사용하는 것보다 생성자를 통해 파라미터를 넘기는 것이 추천된다.  
(위의 setter 사용 지양 이유과 동일)  
* 해결 방안 :   
    + protect로 기본 생성자를 생성해서, 아무곳에서나 객체가 생성되는 것을 막아야 한다.  


### JPA의 엔티티에 기본 생성자
