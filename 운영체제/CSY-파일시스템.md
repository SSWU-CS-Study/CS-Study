# 파일 시스템(2)

## 파일 시스템

파일 시스템은 보조기억장치에 상주하며 블록 I/O를 수행한다.

- 저장소에 대한 사용자 인터페이스 제공
- 디스크에 대한 효율적이고 편리한 접근 제공
- 디렉토리 구조는 파일을 정렬
- File Control Block은 파일에 대한 자세한 정보를 포함

## FAT(File Allocation Table)

FAT 파일 시스템은 DR-DOS, 프리도스, MS-DOS, OS/2, 마이크로소프트 윈도우즈(ME까지)를 포함한 다양한 운영체제를 위한 주된 파일 시스템이다

![https://www.mdpi.com/applsci/applsci-09-05522/article_deploy/html/images/applsci-09-05522-g002.png](https://www.mdpi.com/applsci/applsci-09-05522/article_deploy/html/images/applsci-09-05522-g002.png)

### 예약 영역

- 부트레코드 : 컴퓨터를 처음 켰을 때 동작하는 프로그램 저장, 디스크에 저장되어 있는 운영체제를 주기억장치로 올리는 역할
- 부트스트랩 : 부팅에 사용되는 파티션일 경우 수행하는 부분, 부팅시 동작해야할 명령 코드가 들어있는 부분

### FAT 영역

[https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https://t1.daumcdn.net/cfile/tistory/274E35395513847A29](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https://t1.daumcdn.net/cfile/tistory/274E35395513847A29)

파일이나 디렉터리에 할당 유무가 기록되는 부분

데이터 영역의 어느 부분이 사용되고 있는지 나타낸다. 한 테이블에 오류가 났을 경우 백업을 위해 테이블이 두 개 존재한다.

### 데이터 영역

일반적인 파일을 저장하는 영역. 클러스터 단위로 나뉘어 관리된다.

데이터 영역의 시작은 Root Directory가 할당 받는다.

## Unix file system

- UNIX 파일 시스템에서 디스크는 512, 1024, 2048 바이트 등의 크기를 가지는 블록들로 구성되며,Boot Block, Super Block, I-Node Block, Data Block 4개의 영역을 가진다.
    
    ![https://user-images.githubusercontent.com/48278519/145079342-ee31059f-162a-4a49-aa42-1b957fd42665.png](https://user-images.githubusercontent.com/48278519/145079342-ee31059f-162a-4a49-aa42-1b957fd42665.png)
    

**1) 부트 블록** : 부팅시 필요한 코드를 저장하고 있는 블록

**2) 슈퍼 블록** : 전체 파일 시스템에 대한 정보를 저장하고 있는 블록

- 파일 시스템의 총 블록 개수, 사용 가능한 I-node, 사용 가능한 디스크 블록 개수, 블록 크기 등
- File 시스템마다 각각의 슈퍼블록을 가지고 있음

**3) 데이터 블록** : 디렉터리별로 디렉터리 엔트리와 실제파일에 대한 데이터가 저장된 블록

**4) I-node 블록** : 각 파일이나 디렉터리에 대한 모든 정보를 저장하고 있는 블록

- 파일 소유자의 사용자 번호(UID) 및 그룹 번호(GID), 파일크기, 파일 타입, 생성 시기, 최종 변경시기, 최근 사용 시기, 파일의 보호권한, 파일 링크수, 데이터가 저장된 블록의 시작 주소 등
- **참고 : I-Node 란?**

![https://user-images.githubusercontent.com/48278519/145080710-7c0b72b2-cb4e-4009-8d85-dfea1286bae1.png](https://user-images.githubusercontent.com/48278519/145080710-7c0b72b2-cb4e-4009-8d85-dfea1286bae1.png)

![https://upload.wikimedia.org/wikipedia/commons/thumb/0/09/Ext2-inode.svg/462px-Ext2-inode.svg.png](https://upload.wikimedia.org/wikipedia/commons/thumb/0/09/Ext2-inode.svg/462px-Ext2-inode.svg.png)

- 파일에 대한 정보(메타 데이터)를 가지는 고정 크기를 가진 구조체(1개의 inode는 64byte로 이루어짐)
- i-node, i-node table, i-number, addressing으로 구성
    - **i-node** : 하나의 파일/디렉토리의 모든 정보를 가진 구조체
    - **i-node table** : 전체 i-node를 가지고 있는 테이블 (주기억장치에 저장됨)
    - **i-number** : i-node가 i-node table에 등록되는 entry number
    - **addressing** : 블록 위치 관리(12개의 직접 데이터 블록, 1개의 간접 데이터 블록)

예를 들어, 어떤 한 파일이나 디렉토리를 만들게 되면 1개의 inode가 만들어진다. 그 inode가 inode Table에 등록이되고, 등록되는 entry-number를 그 inode에 대한 inumber라고 한다.

![https://user-images.githubusercontent.com/48278519/145080698-e3179426-cfba-4006-acab-a64405be14bd.png](https://user-images.githubusercontent.com/48278519/145080698-e3179426-cfba-4006-acab-a64405be14bd.png)

- **Direct Block & Indirect Block** : 기존 Direct Block만 존재했을 땐, 4KB 크기 데이터 12개만 처리 가능했음. 하지만 더 큰 크기의 데이터를 내포해야 할 필요가 발생했고, Single Indirect는 한 깊이 들어가서 Direct block을 연결한 것. 여기엔 4KB/4byte = 1024개 즉 4MB 만큼의 data를 포함할 수 있게 된다. 동일한 원리로 Double indirect는 두 깊이 들어가고 Triple indirect는 3깊이 더 깊게 연결되어 더 큰 용량의 데이터를 포함시킬 수 있다.

## 예상 질문

- FAT 파일 시스템과 UNIX계열 파일 시스템의 차이점은 무엇인가요?
    
    FAT 파일 시스템은 파일의 속성을 표시할 때 데이터 블록을 사용하는 반면, 
    Unix 파일 시스템은 inode를 사용하여 파일의 속성을 표시합니다.
    

## 참고 자료

- [https://gedor.tistory.com/5](https://gedor.tistory.com/5)
- [https://www.mdpi.com/2076-3417/9/24/5522](https://www.mdpi.com/2076-3417/9/24/5522)
