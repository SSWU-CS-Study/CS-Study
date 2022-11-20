# 2.  소프트웨어 개발 프로세스

분야: 개발상식
생성 일시: 2022년 11월 20일 오전 9:23

# 학습 목표

- 소프트웨어 프로세스 개념과 프로세스 모형의 중요성
- 폭포수형모형
- 프로토타입 모형
- 점증적모형과 나선형모형
- V모형
- 소프트웨어의 품질

# 2 소프트웨어 개발 프로세스

## 2.1 소프트웨어 생명주기 (SDLC)

<img src = "https://s3.us-west-2.amazonaws.com/secure.notion-static.com/b836918c-5db6-41c1-947b-3e8e5f150d4f/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-10-29_11.47.00.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221120T004443Z&X-Amz-Expires=86400&X-Amz-Signature=7f3325b3d6d62229484f8e8d0fd1aba8232980692634ae1178b9c33fbc3384db&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA%25202022-10-29%252011.47.00.png%22&x-id=GetObject">
- SW공학에서는 SW 개발을 SW 개발 생명주기에 근거하여 접근함
- SW 개발 생명주기는 단계(phase)와 절차에 의해 구성됨
- 다양한 시각의 개발 단계 구분

## 2.2 프로세스

### 즉흥적 개발 Code-and-fix

> 프로세스의 개념이 없는 경우 발생
> 
<img src = "https://s3.us-west-2.amazonaws.com/secure.notion-static.com/23ffda78-5e64-4025-b8f9-a611ab45cd12/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-10-29_11.32.26.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221120T004628Z&X-Amz-Expires=86400&X-Amz-Signature=a47332c90fa8ef2f09e31d8ac34e8b2f73ef86b3172b5d4ef6b689bf5caa21d2&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA%25202022-10-29%252011.32.26.png%22&x-id=GetObject">



- SW개발과 유지보수 비용의 증가 원인이 됨
- 요구나 설계 작업의 중요성을 깨닫지 못함
    - 즉흥적인 방법은 사용자의 높은 요구 수준에 도달 어려움
    - 계속 고치는 작업이 필요함
- 신중하게 잘 설계하지 않으면 소프트웨어 구조가 나빠짐
    - 즉흥적인 방법은 설계를 해도 되고 안 해도 되는 작업이므로
    올바른 설계활동이 이루어지지 못함
- 계획이 없으므로 작업의 목표가 없음
- 체계적인 테스트 작업이나 품질 보증 차원의 활동에 대한 인식 부재
    - 발견되지 않은 결함이 남아 사용 중에도 계속 수정하게 됨

### 소프트웨어 프로세스


소프트웨어 개발에 대한 기술적, 관리적 이슈를 다루는 작업

- SW의 특성에 따라 다양한 형태의 프로세스가 존재함
- 단계(phase)는 고유한 목적과 활동(activity)를 가짐
- 세부 단계는 서로 협력하여 전체 목적을 만족

### 2.2.1 프로세스 종류

- 상품 엔지니어링 프로세스
    - 개발 프로세스
        - 수행해야 할 개발 목록 작업
        - 품질 보증 작업
    - 관리 프로세스
        - 비용, 품질 , 기타 목표를 맞추기 위한 계획, 제어 작업
    - SW 형상관리 프로세스
- 프로세스를 관리하는  프로세스

### 2.2.2  프로세스 정의

프로세스 : 어떤 일을 하기 위한 특별한 방법, 일반적으로 단계나 작업으로 구성됨

프로젝트에서 수행해야 할 작업과 이들의 수행 순서를 정의하는 것임
프로젝트 성공을 위해 최적화되어 있으며 프로젝트마다 다른 프로세스를 가짐

### 2.2.3 좋은 프로세스의 특성

- **`예측가능성`**
    - 비용 예측 vs 품질 예측
    - 과거 프로젝트의 진행 과정에서 수집된 데이터를 통해 프로젝트 성공 여부를 예측 가능할 수 있게 함
- **`테스팅과 유지보수 용이성`**
    - 유지보수가 쉬운 프로세스를 개발해야 함
    - 유지보수, 테스팅 비용을 절감하는 것이 실질적인 절감 ( 테스트와 유지보수에 드는 노력을 낮추는 것)
    - 쉽게 테스트하고 수정할 수 있게 만드는 것
- **`변경용이성`**
    - 사용자 요구의 변경, 조직의 변경, 시장 수요의 변경, 개발환경의 변경이 발생함. 
    좋은 프로세스는 변경을 쉽게 반영할 수 있어야 함
- **`결함 제거 용이성`**
    - 개발 단계에서 생기는 오류는 그 단계에서 수정되어야 하며 구현 후 오류를 찾는 테스팅 단계까지 기다리지 말아야 함
    - 오류탐색과 수정은 소프트웨어 개발 전체 단계에 지속적인 프로세스가 되어야 한다

## 2.3 프로세스 모델

### 2.3.1  정의

프로세스 모델은 개발 단계를 단순화해서 표현한 것이다
프로세스 모델은 SW개발에 대한 다양한 접근 방식을 설명하는 데 사용할 수 있음

### 2.3.2  폭포수 모형
<img src = "https://s3.us-west-2.amazonaws.com/secure.notion-static.com/1428a98d-42da-4b53-ad89-25c990c8a083/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-10-29_11.42.07.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221120T004713Z&X-Amz-Expires=86400&X-Amz-Signature=3f5e1f774d20c937a051e21c35b76a5203d6517517cdd308b752e361dd593a2d&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA%25202022-10-29%252011.42.07.png%22&x-id=GetObject">


<img src = "https://s3.us-west-2.amazonaws.com/secure.notion-static.com/9a5420d2-e7aa-49a0-aba8-e5f279b1cadb/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-10-29_12.49.38.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221120T004758Z&X-Amz-Expires=86400&X-Amz-Signature=8f0aeaebb1475aeae5f2336db302c8ec6278b0e6417462ba929dc692d4c432b3&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA%25202022-10-29%252012.49.38.png%22&x-id=GetObject">

[폭포수 모형의 프로세스와 결과물]



- 각 단계가 다음 단계 시작 전에 끝나야 함
    - 순서적 - 각 단계에 중복이나 상호작용이 없음
    - 각 단계의 결과는 다음 단계가 시작되기 전에 점검
    - 바로 전단계로 피드백
- 단순하거나 응용 분야를 잘 알고 있는 경우 적합
- 결과물 정의가 중요
- 장점
    - 프로세스가 단순하여 초보자가 쉽게 적용 가능
    - 중간 산출물이 명확, 관리하기 좋음
    - 코드 생성 전 충분한 연구와 분석 단계
- 단점
    - 처음 단계의 지나치게 강조하면 코딩, 테스트가 지연
    - 각 단계의 전환에 많은 노력
    - 프로토타입과 재사용의 기회가 줄어듦
    - 소용 없는 다종의 문서를 생산할 가능성 있음

### 2.3.3 프로토타이핑 모형 (Rapid Prototyping Model(RAD))

<img src = "https://s3.us-west-2.amazonaws.com/secure.notion-static.com/ec724d37-e07d-40ff-a814-0cdc6484f900/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-10-29_13.25.41.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221120T005043Z&X-Amz-Expires=86400&X-Amz-Signature=49284c7bacf5dcff9cc061c67b4bc73cc8bdab79771af0ea3500d510f4316529&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA%25202022-10-29%252013.25.41.png%22&x-id=GetObject">

- 한번에 완전한 요구를 추출할 수 없을 경우, 사용자의 요구를 더 정확히 추출 가능
- 알고리즘의 타당성, 운영체제와의 조화, 인터페이스의 시험 제작
- 일반적으로 프로토타입은 작동 가능한 소프트웨어를 의미하나 최종 프로덕트의 일부분으로 사용되지는 않음
- 개발자와 사용자간 공동의 참조 모델로 활용됨
- 화면생성기, 시각적 프로그래밍 언어, 4세대 언어 등을 활용해 만듦
- 종류
    - 진화적(evolutionary) 프로토타입 → 프로토타입을 계속 발전시켜 원래 수준까지 만들어 나감
    - 사용후 폐기 (throwaway) 프로토타입 →  문제를 명확히 이해하는 용도로 사용
- 장점
    - 사용자의 의견 반영이 잘 됨
    - 사용자가 더 관심을 가지고 참여할 수 있고 개발자는 요구를 더 정확히 도출할 수 있음
- 단점
    - 오해, 기대심리 유발 , 관리가 어려움
- 적용
    - 개발 착수 시점에 요구가 불투명할 때
    - 실험적으로 실현 가능성을 타진해 보고 싶을 때 , 혁신적인 기술을 사용해 보고 싶을 때

### 2.3.4  점증적 모형

- **개발 사이클이 짧은 환경**
    - 빠른 시간 안에 시장에 출시해야 이윤에 직결되는 경우
    - 개발 시간을 줄이는 법 → 시스템을 나누어 릴리즈함
- **단계적 개발**
    - 처음 시장에 내놓는 SW는 시장을 빨리 형성
    - 자주 릴리스하면 가동 중인 시스템에서 일어나는 예상하지 못한 문제를 신속 꾸준히 고쳐나갈 수 있음
    - 개발 팀이 릴리즈마다 다른 전문 영역에 초점을 둘 수 있음

<img src ="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/4612aa08-a708-4b73-87a4-c3750c30d029/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-10-29_14.02.38.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221120T005133Z&X-Amz-Expires=86400&X-Amz-Signature=2d45a4a61c9337167bfb941a914e6ae05fad17a0927bc5f357f50291eae2847e&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA%25202022-10-29%252014.02.38.png%22&x-id=GetObject">

<aside>

🌱 **릴리스 구성 방법**

- **`점증적 (incremental) 방법`**
    
    기능별로 릴리스함
    요구분석서의 내용을 기능별로 서브 시스템으로 분할 후 각 서브시스템을 릴리즈함
    
    <img src = "https://s3.us-west-2.amazonaws.com/secure.notion-static.com/08da660b-4b9a-4df0-92f9-e1290d60dc9e/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-10-29_14.12.33.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221120T005208Z&X-Amz-Expires=86400&X-Amz-Signature=e6b02c3b3abd247905bf27dc4228e212906fc320e299078bc5d2977c1383a1c3&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA%25202022-10-29%252014.12.33.png%22&x-id=GetObject">
    

- **`반복적 (iterative) 방법`** 
 전체 시스템을 대상으로 릴리스 할 때마다 기능의 완성도를 높임

<img src = "https://s3.us-west-2.amazonaws.com/secure.notion-static.com/44feaf65-a30d-4c41-854d-e396e6e4a810/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-10-29_14.06.46.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221120T005250Z&X-Amz-Expires=86400&X-Amz-Signature=9d2bf2c42720bdfc139e4c6fbdff0e017d68196575bd18a1fafaa029fb30efa9&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA%25202022-10-29%252014.06.46.png%22&x-id=GetObject">

</aside>

### 2.3.5  나선형(spiral) 모형

- 소프트웨어의 기능을 나누어 점증적으로 개발
    - 실패의 위험을 줄임
    - 테스트, 피드백에 용이
- 여러 번의 점증적인 릴리즈 (incremental release)
- 진화 단계
    - 계획 수립(Planning) : 목표 , 기능 선택, 제약 조건의 결정
    - 위험 분석 (risk analysis) : 기능 선택의 우선순위, 위험요소의 분석
    - 개발(engineering) : 선택된 기능의 개발
    - 평가 (evolution) : 개발 결과의 평가

<img src = "https://s3.us-west-2.amazonaws.com/secure.notion-static.com/45f57dca-acae-4a39-9dd0-5f13958f55f5/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-10-29_14.15.08.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221120T005312Z&X-Amz-Expires=86400&X-Amz-Signature=945c16fe0ba850ab69fca0b7333b4679e93fb0418c5290db567a026c2e2a8579&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA%25202022-10-29%252014.15.08.png%22&x-id=GetObject">

- 장점
    - 대규모 시스템 개발에 적합 risk reduction mechanism
    - 반복적인 개발 및 테스트 ( 강인성 향상)
    - 한 사이클에 추가되지 못한 기능은 다음 단계에 추가 가능
- 단점
    - 관리가 중요
    - 위험 분석이 중요 , 새로운 분석
- 적용
    - 재정적 또는 기술적으로 위험 부담이 큰 경우
    - 요구 사항이나 아키텍처 이해에 어려운 경우

### 2.3.6  V 모형

<img src = "https://s3.us-west-2.amazonaws.com/secure.notion-static.com/49ada224-f711-4429-90fb-f575205f53ff/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-10-29_14.23.06.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221120T005343Z&X-Amz-Expires=86400&X-Amz-Signature=8d000e59e682f8c8412512fd33fd04a6468dd767b4f862b4f184fca5d5dcabf8&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA%25202022-10-29%252014.23.06.png%22&x-id=GetObject">

- 폭포수형 모형을 근간으로 테스트 작업을 강조한 모형
    - 감추어진 반복과 재작업을 드러냄
    - 폭포수 → 문서, 결과물에 중점
    v모형 → 작업의 결과와 검증에 중점
- 코딩을 중심으로 각 단계가 V자 모형의 대칭을 이루어 개발작업과 검증 작업의 관계를 명확히 드러냄
- 점선 : 각 테스트 단계의 오류 발견시 돌아갈 수 있는 단계 지정

- 장점
    - 오류를 줄일 수 있음
    - 높은 신뢰성이 요구되는 분야 (의료제어, 원전제어 시스템 등)에 잘 적용됨
- 단점
    - 생명주기의 반복을 허용하지 않아 변경을 다루기 어려움
    - 이상적인 모델로 요구가 정확하게 정의되고 개발 기간동안 변경되지 않을 경우만 적합

### 2.3.7  객체지향 모형 (Unified Process)

<img src = "https://s3.us-west-2.amazonaws.com/secure.notion-static.com/2bb8f500-a7a2-4be9-a381-d22cc3e176c5/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-10-29_14.30.54.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221120T005413Z&X-Amz-Expires=86400&X-Amz-Signature=54e5a36dbbc2227acba32f6431bab9dd0fcd09d23f842916edc7ca358b41bc4f&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA%25202022-10-29%252014.30.54.png%22&x-id=GetObject">

- 반복적이고 검증적인 개발을 지원
- 빠른 피드백으로 요구 추가/ 변경에 유연하게 대응이 가능
- 도입(inception)
    - 간단한 유스케이스(혹은 사용사례) 모델, 프로젝트 계획을 수행하며, 1,2 회정도 반복함
- 정련(elaboration)
    - 대부분의 유스케이스 모델이 완성되며, 주요 유스케이스에 대한 아키텍처 설계가 이루어짐
- 구축(construction)
    - 유스케이스에 대한 구현이 이루어지며 시스템의 통합이 수행됨
- 전환(tranition)
    - 시스템을 배치(deploy)하는 작업이 수행됨

### 2.3.8  애자일 (Agile) 모형

- 프로젝트 개발 초기에 고객은 원하는 요구사항을 정확히 표현하지 못함
- 폭포수 모형의 단점인 문서화 오버헤드를 감소시킴
- 팀워크, 사용자와의 협력, 변경의 수용, 짧은 주기의 반복을 통해 빠른 속도로 자주 출시하는 접근 방법

<img src = "https://s3.us-west-2.amazonaws.com/secure.notion-static.com/914e51ad-bb98-466b-8fe2-7473289be0b1/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-10-29_14.36.29.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221120T005451Z&X-Amz-Expires=86400&X-Amz-Signature=1dca940d60b345c8430c7258fdb4bf640c21edf51d146ae025025b6c6e1c48a7&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA%25202022-10-29%252014.36.29.png%22&x-id=GetObject">

## 2.4 지원 프로세스

개발을 수행하기 위해서는 프로젝트 관리 프로세스, 형상 관리 프로세스, 품질 관리 프로세스, 프로세스 관리 프로세스 등도 필요함
이러한 작업들은 개발 과정 중 어느 한 단계에만 필요한 작업이 아니라 개발 전 과정에 필요한 작업. 이들을 **우산 프로세스 / 지원 프로세스** 라고 함

### 2.4.1  관리 프로세스

개발 프로세스는 관리 프로세스가 필요한 정보를 제공하고 정보의 해석은 모니터링과 제어 작업에서 한다

- 계획
    - 가장 어렵고 중요한 작업, 프로젝트 목적에 맞는 소프트웨어 개발 계획을 개발하는 것
    - 개발 작업이 시작되기 전에 작성하고, 개발하면서 변경
    - 계획 단계의 주된 작업은 비용과 일정 예측, 중간점검에 대한 결정
    - 모니터링과 제어 작업을 위한 기준이 됨
- 모니터링
    - 개발하는 동안 수행하는 모든 작업이 프로젝트의 목적에 부합되며 개발이 계획대로 진척되는지 확인하기 위해 데이터 수집
    - 비용 , 일정 , 품질 등을 모니터링
- 제어/분석
    - 모니터링에 의해 확인된 사실 분석 후 계획과 차이나는 부분에 대해 조정하고 조치함

### 2.4.2  품질 보증 프로세스

소프트웨어 프로젝트의 프로세스와 프로덕트에 대한 품질을 관리하고 향상시키는 것

- 인스펙션 프로세스
    - 정의된 프로세스에 따라 동료 그룹이 작업 결과를 검사하는 것
    - 결함을 발견하여 작업 결과의 품질을 향상시키는 것이 목표
    - 해결 방법이 아니라 문제 자체에 집중
    - 생명주기 초기에 유입된 결함을 결과물 안에서 찾을 수 있음
    
- 프로세스 관리 프로세스
    - 프로세스를 개선하여 생성된 결과의 품질과 생산성을 향상시키는 것
    - 프로젝트가 진행되는 기간을 초과하는 긴 시간동안 이루어짐

### 2.4.3  형상관리 프로세스 (변경 관리)

- 개발 중에 발생하는 변경을 체계적으로 컨트롤 (개발 프로세스와는 별개)
- 소프트웨어 형상 관리는 개발 작업과 독립적인 작업
- 어떤 자원 하나라도 빠지지 않고 문서의 정확한 버전을 원시코드와 함께 제공하려는 활동

## 2.5 방법론

> How ? 프로세스의 구현 , 개발 방법론
> 

### 2.5.0  방법론?

소프트웨어 프로세스의 각 작업을 어떨게 수행하느냐를 정의한 것 즉, 프로세스의 구현
프로세스는 개발할 때 해야 할 작업만을 명시하고 어떤 관계에 있는지는 나타내지 않음→ 소프트웨어 개발 조직이 방법론을 선택하도록 자유를 줌
방법론은 각 단계와 입력 자료 및 산출물, 이들의 표현 방법까지 명시

방법론은 패러다임에 의해 좌우됨→ 방법론은 프로세스가 구체적으로 구현된 것이며 소프트웨어 프로세스는 하나 이상의 방법론으로 구현할 수 있음

### 2.5.1 구조적 방법론

실세계의 문제를 처리 (process) 하는 관점으로 모델링

모델링은 그래프의 일종인 자료 흐름도 (Data Flow Diagram) 을 사용한다. 각 노드에는 외부 엔티티, 프로세스, 자료 저장소를 표현하며 이들 사이의 자료 흐름 관계를 간선으로 나타낸다. ( 복잡한 프로세스→ 단순하게 분할)

### 2.5.2 정보 공학 방법론

주로 데이터와 관련되어 있음

### 2.5.3 객체지향 방법론

비즈니스를 유스케이스로 분석하고 자료와 함수를 묶은 클래스를 파악하여 상호작용하는 객체들로 시스템을 구성하는 방법
