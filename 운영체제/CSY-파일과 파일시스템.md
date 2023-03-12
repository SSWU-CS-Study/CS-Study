# 파일과 파일 시스템 (File System)

## 정의

**파일**(File)은 논리적인 저장 단위로, 관련된 정보 자료들의 집합에 이름을 붙인 것이다. 컴퓨터 시스템의 편리한 사용을 위해 정보 저장의 일괄된 논리적 관점을 제공한다.

일반적으로 레코드(Record) 혹은 블록(Block) 단위로 비휘발성 보조기억장치에 저장된다.

**파일 시스템**은 컴퓨터에서 파일이나 자료를 쉽게 발견 및 접근할 수 있도록 보관 또는 조직하는 체제이다.

파일 및 파일의 메타데이터, 디렉터리 정보 등을 관리한다.

 

**파티션(Partition)**은 연속된 저장 공간을 하나 이상의 연속되고 독립적인 영역으로 나누어 사용할 수 있도록 정의한 규약이다.

![https://upload.wikimedia.org/wikipedia/commons/thumb/3/3a/Operating_system_placement_kor.svg/270px-Operating_system_placement_kor.svg.png](https://upload.wikimedia.org/wikipedia/commons/thumb/3/3a/Operating_system_placement_kor.svg/270px-Operating_system_placement_kor.svg.png)

### **특징**

- 파일 CRUD 기능을 원활히 수행하기 위한 목적
- 계층적 디렉터리 구조를 가짐
- 디스크 파티션 별로 하나씩 둘 수 있음

## 접근 방법

시스템이 제공하는 파일 정보의 접근 방식으로는 순차 접근(Sequential Access)과 직접 접근(Direct Access, Random Access), 색인 접근(Index Access)으로 나뉜다.

### 순차 접근(Sequential Access)

![https://noep.github.io/2016/02/23/10th-filesystem/10.1.png](https://noep.github.io/2016/02/23/10th-filesystem/10.1.png)

디스크에 있는 파일을 마치 테이프를 재생하는 것처럼 접근한다. 여기서의 접근 방법은 저장되어 있는 레코드 순서로 접근함을 의미한다.

현재 위치를 가리키는 포인터에서 시스템 콜이 발생할 경우 포인터를 이동하면서 read와 write를 진행한다. 뒤로 돌아갈 땐 지정한 offset만큼 되감기를 해야 한다.

### 직접 접근(Direct Access, Random Access)

[https://t1.daumcdn.net/cfile/tistory/2205193D55A73DE724](https://t1.daumcdn.net/cfile/tistory/2205193D55A73DE724)

디스크모델에 기반을 하고 있으며 직접 접근을 위해 파일은 일련의 블록 또는 레코드로 간주된다.

무작위 파일 블록에 대한 임의 접근을 허용한다. 따라서 순서의 제약이 없음

대규모 정보를 접근할 때 유용하기 때문에 '데이터베이스'에 활용된다.

### 색인 접근(Index Access)

직접 접근 파일이 있으면 그것을 기반으로 여러 가지 다른 파일 접근 방법을 제공할 수 있다.

![10.3.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/af94d34e-912f-4828-905a-c1f20a3b6e8e/10.3.png)

파일에서 레코드를 찾기 위해 색인을 먼저 찾고 대응되는 포인터를 얻는다. 이를 통해 파일에 직접 접근하여 원하는 데이터를 얻을 수 있다. 따라서 크기가 큰 파일에서 유용하다.

## ****Directory****

디렉터리(Directory)는 파일의 메타데이터 중 일부를 보관하고 있는 일종의 특별한 파일이다. 
(메타데이터 : 파일명, 파일 유형, 접근제어정도, 생성날짜와 시간 등)

해당 디렉터리에 속한 파일 이름과 속성들을 포함하고 있고, 다음과 같은 기능들을 제공한다.

- 파일 찾기(Search)
- 파일 생성(Create)
- 파일 삭제(Delete)
- 디렉터리 나열(List)
- 파일 재명명(Rename)
- 파일 시스템 순회(Traverse)

### **1단계 디렉터리(Single-Level Directory)**

1단계 디렉터리는 **모든 파일들이 디렉터리 밑에 존재하는 형태**이다. 파일들은 서로 유일한 이름을 가지고 서로 다른 사용자라도 같은 이름을 사용할 수 없다.

지원하기도 쉽고 이해하기도 쉽지만, 파일이 많아지거나 다수의 사용자가 사용하는 시스템에서는 심각한 제약을 갖는다.

![https://blog.kakaocdn.net/dn/bbk9x6/btrgpwxekSY/LC5Y2BhsAypMTbdWfMToRK/img.png](https://blog.kakaocdn.net/dn/bbk9x6/btrgpwxekSY/LC5Y2BhsAypMTbdWfMToRK/img.png)

### **2단계 디렉터리(Two-Level Directory)**

2단계 디렉터리는 **각 사용자별로 별도의 디렉터리를 갖는 형태**이다.

![https://blog.kakaocdn.net/dn/lLfBK/btrgxCCjMG6/eRQ02qksvFI7JIGQZ2KZdK/img.png](https://blog.kakaocdn.net/dn/lLfBK/btrgxCCjMG6/eRQ02qksvFI7JIGQZ2KZdK/img.png)

서로 다른 사용자가 같은 이름의 파일을 가질 수 있고 효율적인 탐색이 가능하다. 

하지만 그룹화가 불가능하고, 다른 사용자의 파일에 접근해야 하는 경우에는 단점이 된다.

### **트리 구조 디렉터리(Tree-Structured Directory)**

**사용자들이 자신의 서브 디렉터리(Sub-Directory)를 만들어서 파일을 구성할 수 있다**. 
하나의 루트 디렉터리를 가지며 모든 파일은 고유한 경로(절대 경로/상대 경로)를 가진다. 이를 통해 효율적인 탐색이 가능하고, 그룹화가 가능하다.

디렉터리는 일종의 파일이므로 일반 파일인지 디렉터리인지 구분할 필요가 있다. 이를 bit를 사용하여 0이면 일반 파일, 1이면 디렉터리로 구분한다.

![https://blog.kakaocdn.net/dn/k8pMk/btrgx0JQ4zK/Zw5OniN9knvCQxKTDBJ3P1/img.png](https://blog.kakaocdn.net/dn/k8pMk/btrgx0JQ4zK/Zw5OniN9knvCQxKTDBJ3P1/img.png)

### **비순환 그래프 디렉터리(Acyclic-Graph Directory)**

**디렉터리들이 서브 디렉터리들과 파일을 공유할 수 있도록 한다.** 트리 구조의 디렉터리를 일반화한 형태이다.

단순한 트리 구조보다는 더 복잡한 구조이기 때문에 몇몇 문제가 발생할 수 있다.

파일을 무작정 삭제하게 되면 현재 파일을 가리키는 포인터는 대상이 사라지게 된다. 따라서 참조되는 파일에 참조 계수를 두어서, 참조 계수가 0이 되면 파일을 참조하는 링크가 존재하지 않는다는 의미이므로 그때 파일을 삭제할 수 있도록 한다.

![https://blog.kakaocdn.net/dn/c1f9RF/btrgvUpXqh3/IDPjd6TkDgHQYuURltRmKk/img.png](https://blog.kakaocdn.net/dn/c1f9RF/btrgvUpXqh3/IDPjd6TkDgHQYuURltRmKk/img.png)

### **일반 그래프 디렉터리(General Graph Directory)**

**순환을 허용하는 그래프 구조이다.** 순환이 허용되면 무한 루프에 빠질 수 있다.

따라서, 하위 디렉터리가 아닌 파일에 대한 링크만 허용하거나 garbage collection을 통해 전체 파일 시스템을 순회하고, 접근 가능한 모든 것을 표시한다.

![https://blog.kakaocdn.net/dn/bTWgpK/btrgC55TQPU/eFeUEtjgM8pbkyRsq4eX3K/img.png](https://blog.kakaocdn.net/dn/bTWgpK/btrgC55TQPU/eFeUEtjgM8pbkyRsq4eX3K/img.png)

## 파일 시스템 종류

### ****MS-DOS FAT 파일시스템****

[https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https://blog.kakaocdn.net/dn/bex8kH/btqFkn3lz73/RQ3KsyKp6Vs7s6w2eJB2k1/img.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https://blog.kakaocdn.net/dn/bex8kH/btqFkn3lz73/RQ3KsyKp6Vs7s6w2eJB2k1/img.png)

FAT block을 사용하는 것이 특징이며 FAT block table의 요소 하나하나는 실제 데이터블록과 대응된다. FAT(파일 할당 테이블)라는 영역에 파일의 위치를 기록하고 관리한다.

현재 디렉토리는 데이터 블록의 주소로 기억하며 디렉토리를 만들면 테이블이 만들어지고 root 디렉토리를 제외한 디렉토리에서는 `.`과 `..`이 기본으로 생성된다. 

파일을 찾고자 할 때는 파일시스템이 알고있는 루트 디렉토리를 기준으로 찾아나가면 된다.

### ****UNIX V7 파일시스템****

[https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https://blog.kakaocdn.net/dn/ckKCof/btqFmutsZro/FYCXtGoi8zkklCFbWD5KTK/img.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https://blog.kakaocdn.net/dn/ckKCof/btqFmutsZro/FYCXtGoi8zkklCFbWD5KTK/img.png)

inode를 사용한 것이 특징이고 Super block에는 전체 파일시스템의 형상구조가 담겨있다.

0,1번 inode는 전통적으로 사용하지 않고 2번 inode block부터 root 디렉토리를 기록하면서 사용된다.

## 예상 질문

- 파일시스템이란 무엇인가요?
    
    컴퓨터에서 파일이나 자료를 쉽게 찾을 수 있도록 유지, 관리하는 방법입니다. 
    
    하드디스크와 메인 메모리의 속도 차를 절감하고, 하드디스크를 효율적으로 운용하기 위해 사용합니다.
    
- 파일의 접근 방법을 설명해주세요.
    
    순차 접근은 포인터를 순서대로 움직이면서 read와 write를 하고, 돌아갈 때는 지정한 offset만큼 rewind를 합니다.
    
    직접 접근은 현 위치를 가리키는 변수 cp를 활용하여 특별한 순서 없이 빠르게 레코드를 read, write할 수 있습니다.
    
    색인 접근은 직접 접근 파일에 기반된 인덱스를 구축하여 크기가 큰 파일의 입출력 탐색을 가능하게 합니다.
    
- FAT 파일 시스템과 UNIX계열 파일 시스템의 차이점은 무엇인가요?
    
    FAT 파일 시스템은 파일의 속성을 표시할 때 데이터 블록을 사용하는 반면, 
    Unix 파일 시스템은 inode를 사용하여 파일의 속성을 표시합니다.
    

## 참고 자료

- [https://rebro.kr/181](https://rebro.kr/181)
- [https://sungwookkang.com/199](https://sungwookkang.com/199)
- [https://gyoogle.dev/blog/computer-science/operating-system/File System.html](https://gyoogle.dev/blog/computer-science/operating-system/File%20System.html)
- [https://oliviarla.tistory.com/m/7](https://oliviarla.tistory.com/m/7)
