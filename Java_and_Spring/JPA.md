# JPA

분야: Spring

생성 일시: 2022년 11월 20일 오전 9:26



</aside>

# What is  JPA

## 1-1 JPA의 개념 & 사용하는 이유

> Javva Persistence API 자바 ORM 기술 표준으로 채택된 인터페이스
> 

<aside>

💡 **WHY JPA?**
JPA는 내부적으로 JDBC를 사용한다.  개발자가 직접 JDBC를 구현하면 SQL에 의존하게 되는 문제 등이 있어 개발의 효율성이 떨어지는데 JPA는 이 같은 문제점을 보완하여 개발자 대신 적절한 SQL을 생성하고 데이터베이스를 조작해서 객체를 자동으로 매핑한다.

</aside>

- `Spring Data JPA`

    - JPA를 편리하게 사용할 수 있도록 지원하는 스프링 하위 프로젝트 중 하나
    - CURD 처리에 필요한 인터페이스를 제공하며 Entity Manager를 직접 다루지 않고 레포지토리를 정의해 사용함으로써 스프링이 적합한 쿼리를 동적으로 생성하는 방식으로 데베 조작
- `Entity`
    - Spring Data JPA를 사용하면 데이터베이스에 테이블을 생성하기 위해 직접 쿼리를 작성할 필요가 없음 → Entity가 대신
    - `@Entity` : 해당 클래스가 엔티티임을 명시하기 위한 어노테이션. 클래스 자체는 테이블과 일대일로 매칭되며, 해당 클래스의 인스턴스는 매핑되는 테이블에서 하나의 레코드를 의미
    - `@Table` : 엔티티 클래스는 테이블과 매핑됨.
    - `@Id` : 테이블의 기본키
    - `@GeneratedValue` : 해당 필드의 값을 자동으로 생성할지 결정해줌
    - `@Column` : 테이블 칼럼과 매핑 (자동으로 설정됨)
    - `@Transient` : 엔티티클래스에는 선언돼 있는 필드지만 데이터베이스에서는 필요 없을 경우 사용

## 1-2 ORM

> Object Relational Mapping 객체 관계 맵핑
> 
- 자바와 같은 객체지향 언어에서 의미하는 객체(클래스)와 RDB(Relational Database)의 테이블을 자동으로 매핑하는 방법
    - 클래스는 데이터베이스의 테이블과 매핑하기 위해 만들어진 것이 아니기 때문에 RDB테이블과의 불일치가 존재 → ORM이 제약상황을 해결

               

        
- ORM을 이용하면 쿼리문 작성이 아닌 코드( 메서드)로 데이터 조작 가능
- **`ORM의 장점`**
    - 데이터베이스 쿼리를 객체지향적으로 조작할 수 있다
    - 재사용 및 유지보수 용이
        - 매핑된 객체는 모두 독립적으로 작성되어 있어 재사용 용이
        - 객체들은 각 클래스로 나뉘어져 있어 유지보수가 수월
    - 데이터베이스에 대한 종속성이 줄어듦
    
- **`ORM의 단점`**
    - ORM만으로 온전한 서비스를 구현하기에는 한계 존재
        - 복잡한 서비스의 경우 쿼리를 구현하지 않고 코드로만 하기에는 어려움
        - 속도 저하 등의 성능 문제가 생길 수 있음
    - **애플리케이션의 객체 관점과 데이터베이스의 관계 관점의 불일치 발생**
        - 세분성 : ORM의 자동 설계 방법에 따라 데이터베이스에 있는 테이블의 수와 애플리케이션의 엔티티 클래스의 수가 다른 경우 ( 클래스 > 테이블)
        - 상속성 : RDBMS는 상속성이 없음
        - 식별성 : RDBMS는 기본키로 동일성을 정의함. 하지만 자바는 두 객체의 값이 같아도 다르다고 판단할 수 있음
        - 연관성 : 객체지향 언어는 객체를 참조하지만 RDBMS에서는 외래키를 참조함, 또한 객체지향 언어에서는 객체를 참조할 때 방향성이 존재하지만, RDBMS에서는 외래키를 삽입하는 것은 양방향의 관계를 가지기 때문에 방향성이 X
        - 탐색 : 자바에서는 특정 값에 접근하기 위해 객체 참조 같은 연결수단 활용 . RDBMS 는 쿼리를 최소화하고 조인을 통해 여러 테이블을 로드하고 값을 추출하는 접근방식
    

## 1-3 @Transactional

> 트랜젝션: 모든 작업들이 성공적으로 완료되어야 작업 묶음의 결과를 적용하고, 오류가 발생했을 때는 이전에 있던 모든 작업들이 성공적이었더라도 없었던 일처럼 완전히 되돌리는 것
> 
- 데이터베이스를 다룰 때 트랜잭션을 적용하면 데이터 추가, 갱신, 삭제 등으로 이루어진 작업을 처리하던 중 오류가 발생했을 때 **모든 작업들을 원상태로 되돌릴 수 있다**. 모든 작업들이 성공해야만 최종적으로 데이터베이스에 반영하도록 한다.
- `@Transactional`
이 붙은 메서드는 메서드가 포함하고 있는 작업 중에 하나라도 실패할 경우 전체 작업을 취소한다
- 메서드 내 작업을 마칠 경우 자동으로 flush() 메서드를 실행한다. 이 과정에서 변경이 감지되면 대상 객체에 해당하는 데이터베이스의 레코드를 업데이트하는 쿼리가 실행된다
- `테스트 케이스` 에서 활용시 이 애노테이션이 있으면, 테스트 시작 전에 트랜잭션을 시작하고, 테스트 완료 후에 항상 롤백한다. 이렇게 하면 DB에 데이터가 남지 않으므로 다음 테스트에 영향을 주지 않는다.
    
    ```java
    //db까지 연결된 테스트
    @SpringBootTest
    @Transactional
    class MemberServiceIntegrationTest {
        @Autowired MemberService memberService;
        @Autowired MemberRepository memberRepository;
    
        @Test
        void 회원가입() { 
            //given 주어지는 데이터
            Member member = new Member();
            member.setName("nayeon");
    
            //when 무엇을 검증하는지
            Long saveId = memberService.join(member);
    
            //then 검증부
            Member findMember = memberService.findOne( saveId).get();
            assertThat(member.getName()).isEqualTo(findMember.getName());
        }
    
        @Test
        public void 중복_회원_예외(){
            ,,,
            //then
            assertThat(e.getMessage()).isEqualTo("이미 존재하는 회원입니다");
    
        }
        ,,,
    }
    ```
    

## 1-4 추가로 정리한 심화내용

### a. JPA Repository

> 레포지토리 : Spring Data JPA 가 제공하는 인터페이스
> 
- 레포지토리를 생성하기 위해서는 접근하려는 테이블과 매핑되는 엔티티에 대한 인터페이스를 생성하고, JPA 레포지토리를 상속받으면 됨
    
    ```java
    public interface ProductRepository extends JpaRepository<Product,Long> {
    
    }
    ```
    
    `ProductRepository` 가 `JpaRepository`를 상속받을 때는 **대상 엔티티**와 **기본키** 타입을 지정해야 한다.
    
    생성된 리포지토리는 `JpaRepository` 를 상속받으면서 별도의 메서드 구현 없이도 많은 기능을 제공한다. → CRUD
    
    - Spring Data JPA가 제공하는 기본 메서드
        - `save(S)` : 새로운 엔티티는 저장하고 이미 있는 엔티티는 병합한다.
        - `delete(T)` : 엔티티 하나를 삭제한다. 내부에서EntityManager.remove() 호출
        - `findById(ID)` : 엔티티 하나를 조회한다. 내부에서 EntityManager.find() 호출
        - `getOne(ID)` : 엔티티를 프록시로 조회한다. 내부에서 EntityManager.getReference() 호출
        - `findAll(…)` : 모든 엔티티를 조회한다. 정렬( Sort )이나 페이징( Pageable ) 조건을 파라미터로 제공할수 있다
        

### b. JPQL

> JPA Query Language : JPA 에서 사용할 수 있는 쿼리
> 
- 엔티티 객체를 대상으로 수행하는 쿼리이기 때문에 매핑된 엔티티의 이름과 필드의 이름을 사용
    
    ![Untitled](JPA%207449b4d56c79470498dee524656498eb/Untitled%201.jpeg)
    
- 쿼리 메서드는 크게 동작을 결정하는 주제 (Subject)와 서술어 (Predicate)로 구분한다. `find...By` , `exist...By` 와 같은 키워드로 쿼리의 주제를 정한다.
    - `By` : 서술어의 시작을 나타내는 구분자 역할
- 쿼리 메서드의 **주제 키워드**
    - 조회  `find...By` `read...By`  `get...By` 등
        - `‘…’` 에는 엔티티 표현 , 이미 엔티티를 설정한 후에 메서드를 사용하기 때문에 중복으로 판단해 생략하기도 함
        - 리턴타입으로는 Collection 이나 Stream에 속한 하위 타입을 설정할 수 있다
        
        ```java
        //find...by
        Optional<Product> findByNumber(Long number);
        List<Product> findAllByName(String name);
        Product queryByNumber(Long number)
        ```
        
    
    - `existBy` : 특정 데이터가 존재하는지 확인하는 키워드. 리턴타입으로는 Boolean을 사용
    `boolean existByNumber(Long number);`
    - `countBy` : 조회 쿼리를 수행한 후 쿼리 결과로 나온 레코드의 개수 리턴
    `long countByName(String name);`
    - 삭제 `deleteBy`, `removeBy` : 리턴 타입이 없거나 삭제한 횟수를 리턴
        
        ```java
        void deleteByNumber(Long number);
        long removeByName(String name);
        ```
        
    - `First<number>…` , `…Top<number>` : 쿼리를 통해 조회된 결과값의 개수를 제한, 여러건을 조회할 때 사용되며, 단건을 조회하기 위해서는 <number> 생략
        
        ```java
        List<Product> findFirstByName(String name);
        List<Product> findTop10ByName(String name);
        ```
        
- 쿼리메서드의 **조건자** 키워드 **(서술어)**
    - `Is` : 값의 일치를 조건으로 사용하는 조건자 , `Equals` 와 동일
    `Product findByNameEquals (Long number)`;
    - `Not` : 값의 불일치를 조건으로 사용하는 조건자
    `Product findByNameNot (Long number)`;
    - `Null, NotNull` : 값이 Null인지 조사
    - `True` `False` : boolean타입으로 지정된 칼럼값을 확인하는 키워드
    `Product findByisActiveTrue ()`;
    - `And` , `Or` : 여러 조건을 묶을 때 사용
    `Product findByNumberAndName(Long number, String name);`
    - `GreaterThan` `LessThan` `Between` : 숫자나 datetime칼럼을 대상으로 한 비교 연산에 사용할 수 있는 조건자 키워드 ( 초과/ 미만의 개념이므로 경계값을 포함하려면 Equal 붙여야함)
        
        `List<Product> findByPriceIsGreaterThanEqual(Long price);`
        
- 정렬하기
    
    ```java
    //Asc : 오름차순 , Desc : 내림차순
    List<Product> findByNameOrderByNameAsc(String name);
    List<Product> findByNameOrderByNameDesc(String name);
    
    //price를 기준으로 오름차순 정렬 후 재고수량을 기준으로 내림차순
    List<Product> findByNameOrderByPriceAscStockDesc(String name);
    ```
    
    - 쿼리 메서드의 이름에 정렬 키워드를 삽입해서 정렬을 수행하는 것도 가능하지만, 메서드의 이름이 길어질수록 가독성 저하
    - 매개변수 활용하기
        
        `List<Product>findByName(String name, Sort sort);`
        
    

### c. 연관관계 맵핑

- 특징
    - 어떤 엔티티를 중심으로 연관 엔티티를 보느냐에 따라 연관관계의 상태가 달라진다
    - 엔티티간 참조 방향 설정 가능
        - **`단방향`** → 두 엔티티의 관계에서 한쪽의 엔티티만 참조
        - **`양방향`** → 두 엔티티의 관계에서 각 엔티티가 서로의 엔티티를 참조하는 형식
    - **`주인(Owner)`** → 다른 테이블의 기본키를 외래키로 가진 테이블 , 주인은 외래키를 사용할 수 있으나 상대 엔티티는 읽는 작업만 수행할 수 있음
- 종류
    - **`One to One : 일대일 (1:1 )`**
        
        ex) 하나의 상춤당 하나의 상품정보만 매칭되는 구조
        
        - 단방향 매핑
            
            ```java
            
            public class ProductDetailEntity{
            ...
            @OneToOne
            //@OneToOne(Optional=false) product가 null인 값을 허용하지 않겠다는 뜻
            @JoinColumn(name="pruduct_number")
            private Product product;
            
            }
            ```
            
            @JoinColumn 어노테이션을 이용해서 매핑할 외래키를 설정한다. 만약 @JoinColumn을 설정하지 않으면 엔티티를 매핑하는 중간 테이블이 생기면서 관리포인트가 늘어나 좋지 않음
            
            ```java
            @SpringBootTest
            class ProductDetailRepositoryTest{
            		// 상품과 상품정보에 패핑된 레퍼지토리에 대한 의존성을 주입받아야 함
            		@Autowired
            		 ProductDetailRepository  productDetailRepository;
            
            		@Autowired
            		 ProductRepository  productRepository;
            
            		@Test
            		public void 저장과_조회하기(){
            			//저장
            			Product product = new product();
            			product.setName("상품1");
            			productRepository.save(product);
            
            			ProductDetail productDetail = new ProducrDetail();
            			productDetail.setProduct(product);
            			productDetail.setName("상품1입니다");
            			productDetailRepository.save(productDetail);
            			
            			//상품조회
            			System.out.println(productDetailRepository.findById(productDetail.getId()).get().getproduct());
            			
            			//상품설명조회
            			System.out.println(productDetailRepository.findById(productDetail.getId()).get());
            	}
            }
            ```
            
        - 양방향 매핑
            - 양쪽에서 단방향으로 서로를 매핑하는 개념
            - 한쪽의 테이블에서만 외래키 변경할 수 있도록 허용 (주인 개념)
            - `mappedBy` : 어떤 객체가 주인인지 표시하는 속성
                
                ```java
                @OneToOne(mappedBy="product")
                @ToString.Exclude
                private ProductDetail productDetail;
                ```
                
                `mappedBy` 에 들어가는 값은 연관관계를 갖고 있는 상태 엔티티에 있는 연관관계 필드의 이름이 된다. 
                **`PruductDetail`** 엔티티가 `Product` 엔티티의 주인이 되는 것
                
            
    - **`Many to One : 다대일 (N : 1)`**
        
        ex) 하나의 공급업체와 여러개의 상품 관계
        
        - 단방향 매핑
            
            ```java
            @Entity
            public class Product{
            	,,,
            
            	@ManyToOne(name = "provider_id")
            	@ToString.Exclude
            	private Provider provider;
            
            }
            ```
            
            외래키를 갖는 쪽이 주인의 역할을 하기 때문에 , 상품 엔티티가 공급업체의 주인이 됨
            
            ```java
            @SpringBootTest
            class ProviderRepositoryTest{
            	@Autowired
            	ProductRepository productRepository;
            
            	@Autowired
            	ProviderRepository providerRepository;
            
            	@Test
            	void 관계테스트(){
            	//생성
            	Provider provider = new Provider();
            	provider.setName("공급업체1");
            	providerRepository.save(provider);
            
            	Product product = new product();
            	product.setName("상품1");
            	product.setProvider(provider);
            	productRepository.save(product);
            
            	}
            }
            ```
            
        - 양방향매핑
            - 공급업체 (Provider)를 통해 등록된 상품을 조회하기 위한 일대다 연관관계
                
                ```java
                @Entity
                public class Provider{
                	,,,
                
                	@OneToMany(mappedBy = "provider",fetch = FetchType.EAGER)
                	@ToString.Exclude
                	private List<Product> productList = new ArrayList<>();
                }
                ```
                
                - 일대다 연관관계의 경우 여러 상품 엔티티가 포함될 수 있어 컬렉션 형식으로 필드 생성
                - @OneToMany가 붙은 쪽에서 @JoinColumn어노테이션을 사용하면 상대 엔티티에 외래키가 설정된다
                - 롬복의 ToString에 의해 순환참조가 발생할 수 있어 `@ToString.Exclude`를 해주는 것이 좋음
                
                ```java
                @SpringBootTest
                class ProviderRepositoryTest{
                	@Autowired
                	ProductRepository productRepository;
                
                	@Autowired
                	ProviderRepository providerRepository;
                
                	@Test
                	void 관계테스트(){
                	//생성
                	Provider provider = new Provider();
                	provider.setName("공급업체1");
                	providerRepository.save(provider);
                
                	Product product1 = new product();
                	product1.setName("상품1");
                	product1.setProvider(provider);
                
                	Product product2 = new product();
                	product2.setName("상품2");
                	product2.setProvider(provider);
                
                	productRepository.save(product1);
                	productRepository.save(product2);
                
                	List<Product> products = providerRepository.findById(provider.getId()).get()
                		.getProductList();
                	
                	for(Product product : products){
                			System.out.println(product);
                		}
                	}
                }
                ```
                
            - 주의
                - Provider는 Product의 주인이 아니기 때문에 외래키를 관리할 수 없음
                - `~~provider.getProductList().add(product1);~~` (X)
    - **`One to Many : 일대다 (1:N)`**
        
        ex) 상품분류와 상품간의 관계
        
        - 단방향
            
            ```java
            @Entity
            public class Category{
            	@OneToMany(fetch = fetchType.EAGER)
            	@JoinColumn(name = "category_id)
            	private List<Product> products = new ArrayList<>();
            
            }
            ```
            
            - @OneToMany , @JoinColumn 을 사용하면 일대다 단방향 관계가 설정됨
            - **일대다 단방향 관계는 매핑의 주체가 아닌 반대 테이블에 외래키가 추가됨**
            - 이 방식은 다대일 구조와 다르게 외래키를 설정하기 위해 다른 테이블에 대한 update 쿼리를 발생시킴
                
                ```java
                class CategoryRepositoryTest{
                	@AutoWired
                	ProductRepository peoductRepository;
                	
                	@AutoWired
                	CategoryRepository categoryRepository;
                	
                	@Test
                	void 일대다테스트(){
                	Product product = new product();
                	product.setName("상품1");
                	productRepository.save(product);
                
                	Category category = new Category();
                	category.setName("도서");
                	category.getProducts().add(product);
                	
                	categoryRepository.save(category);
                
                	//조회
                	List<Product> products = categoryRepository.findById(1L).get().getproducts();
                
                	for(Product foundProduct : products){
                			System.out.println(product);
                		}
                	}
                
                }
                ```
                
    - **`Many to Many : 다대다 ( N : M)`**
        
        실무에서 잘 활용하지 않으므로 생략
