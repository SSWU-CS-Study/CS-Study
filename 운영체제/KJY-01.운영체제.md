## 1.1 운영체제란?


컴퓨터 사용자와 컴퓨터 하드웨어 사이에서 중재자 역할을 하는 프로그램

넓은 의미로는 운영체제를 실행하기 위해 필요한 모든 것으로 윤영체제와 함께 실행되는 시스템 프로그램이나 응용 프로그램을 말하기도 한다. 좁은 의미로서의 운영체제는 컴퓨터에서 언제나 작동하고 있는 프로그램, 일명 kernel을 뜻한다.

- 커널Kernel
    
    컴퓨터의 자원을 관리하는 역할을 수행하는 운영체제의 핵심 프로그램
    
- 쉘Shell

![https://velog.velcdn.com/images/qwe2487/post/566d9247-6387-4274-889c-70d6ad6574fd/image.png](https://velog.velcdn.com/images/qwe2487/post/566d9247-6387-4274-889c-70d6ad6574fd/image.png)

![https://velog.velcdn.com/images%2Fhoyun7443%2Fpost%2Fee8c83e9-a818-41a8-95af-7ad854735db7%2Fimage.png](https://velog.velcdn.com/images%2Fhoyun7443%2Fpost%2Fee8c83e9-a818-41a8-95af-7ad854735db7%2Fimage.png)
   


## 1.2 목적


- 컴퓨터 시스템을 사용자가 **편리**하게 이용할 수 있는 환경을 제공하기 위해서
    - 사용자 프로그램을 실행
- 컴퓨터 하드웨어를 **효율**적으로 사용하기 위해서
    - 자원을 관리하고 할당
    - 프로그램의 실행을 제어



## 1.3 역할


- 자원할당자
    - 모든 자원을 관리한다.
    - 효율적이고 공정한 자원 사용에 대한 상반된 요청들을 결정한다.
- 프로그램 제어
    - 오류 및 컴퓨터의 부적절한 사용을 방지하기 위해 프로그램 실행을 제어한다.
    - I/O기기들을 제어한다.



## 1.4 기능

- 프로그램 실행
    - 프로그램을 메모리에 로드
    - 프로그램 실행
    - 실행 종료
- 사용자 인터페이스
    - Batch Interface
    - Command-Line Interface
    - Graphical User Interface
- 파일 시스템 조작
    - 파일 또는 폴더 읽고 쓰기
    - 파일 또는 폴더 생성, 삭제, 검색
    - 파일 정보 나열
    - 허가 관리
- I/O 작업
    - 파일이나 I/O 기기 포함
    - 효율성 증진 또는 보호
- 통신
    - 한 컴퓨터 내에서의 통신
    - 네트워크를 통한 다른 컴퓨터들과의 통신
- 오류 탐지
    - CPU, 메모리 하드웨어, I/O 기기 또는 사용자 프로그램에서 발생할 수 있는 오류
    - 정확하고 일관된 컴퓨팅을 보장하기 위해 적절한 조취를 취해야 함





## 1.5 운영체제의 분류

- 동시 작업 가능 여부
    - 단일 작업
        
        한 번에 하나의 작업만 처리 가능
        
        ex) MS-DOS
        
    - **다중 작업**
        
        동시에 두 개 이상의 작업을 처리 가능
        
        ex) UNIX, MS Windows
        
- 사용자 수
    - 단일 사용자
        
        ex) MS-DOS, MS Windows
        
    - **다중 사용자**
        
        ex) UNIX, NT server
        
- 처리 방식
    - 일괄처리
        
        작업을 모아서 한 번에 처리
        
    - **시분할**
        
        여러 작업을 수행할 때 일정한 시간 단위로 분할하여 사용
        
    - 실시간
        
        정해진 시간 안에 작업 종료가 반드시 보장됨
        
        특정한 목적을 가진 시스템에서 사용
        
        ex) 미사일 제어, 원자로 제어
        

→ 현대 대부분의 운영체제는 다중 작업, 다중 사용자, 시분할 방식으로 운영





## 1.6 부팅

운영체제를 메인 메모리에 적재하는 과정



1. BIOS (basic input output system)
    
    CPU가 ON되면, CPU는 ROM에 있는 BIOS 데이터를 읽어온다.
    
2. POST (post on self test)
    
    BIOS는 POST를 진행하여 하드웨어의 정상적인 작동을 검사한다.
    
    ![https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fk.kakaocdn.net%2Fdn%2FdIrhAm%2FbtrbosNj0bn%2F0BodE2QKi7GPppSRV49w8K%2Fimg.jpg](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fk.kakaocdn.net%2Fdn%2FdIrhAm%2FbtrbosNj0bn%2F0BodE2QKi7GPppSRV49w8K%2Fimg.jpg)
    
3. Bootstrap
    
    POST에 이상이 없으면 BIOS는 부트스트랩을 실행하여 부팅 정보를 메모리로 읽어 온다.
    
4. Bootloader
    
    Bootloader는 Disk에 있는 OS 코드를 메모리로 읽어 온다. 즉, 3에서 읽어온 부팅 정보는 Bootloader이다. ~~운영체제는 메모리에 상주하지 않지만 커널은 메모리에 상주한다.~~
    
    ![https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fk.kakaocdn.net%2Fdn%2FbxKVzz%2Fbtrbq4rdwM3%2FrNvk4kXrR42eMJwauKt2B0%2Fimg.jpg](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fk.kakaocdn.net%2Fdn%2FbxKVzz%2Fbtrbq4rdwM3%2FrNvk4kXrR42eMJwauKt2B0%2Fimg.jpg)
    
5. OS
    
    읽어 온 운영체제 명령에 의해 CPU는 첫 프로세스(Demon)을 즉시 실행한다. 이후, 인터럽트가 발생하면 CPU는 각종 작업을 처리한다.
    
    ![https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fk.kakaocdn.net%2Fdn%2FwWSp6%2FbtrbrUPOFqa%2FKGuKcKNNbRwSdda1KMjWk0%2Fimg.jpg](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fk.kakaocdn.net%2Fdn%2FwWSp6%2FbtrbrUPOFqa%2FKGuKcKNNbRwSdda1KMjWk0%2Fimg.jpg)
    
- 부트스트랩
    
     ROM 또는 펌웨어에 저장되어 있는 소규모 프로그램으로 컴퓨터가 처음 켜질 때 가장 처음 실행되는 프로그램
    
- 부트로더
    
    하드디스크와 같은 보조기억장치에 저장된 운영체제를 메인 메모리에 적재하는 ROM에 고정시킨 소규모 프로그램
    
    
    
    

## 면접 예상 질문

- 운영체제란 무엇인가?
- 운영체제의 역할은 무엇인가?





## 참고

> [https://justzino.tistory.com/m/2](https://justzino.tistory.com/m/2)
>
