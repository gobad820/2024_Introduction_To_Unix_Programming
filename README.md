# ch01

## 유닉스의 개요

### 유닉스의 특징
**대화형 시스템이며 계층적 트리 파일 시스템이다.**
또한 다중 사용자 시스템, 다중 작업 시스템을 지원하며 높은 이식성과 확장성 그리고 다양한 부가 기능을 제공한다.

### 유닉스의 구조

**하드웨어 < 커널 < 셸 < 유틸리티**
순서대로 올라간다. 커널은 유틸리티와 셸 모두 접근이 가능하다. 

#### 커널
커널은 운영체제의 핵심이며 프로세스, 메모리, 파일 시스템, 장치 등을 관리한다.

#### 셸
셸은 사용자 인터페이스를 제공하며 명령을 입력받아 처리한 후 결과를 출력한다.

#### 유틸리티
각종 개발 도구 및 편집 도구 등이 된다. 사용자 프로그램?

## 유닉스 접속 방법과 명령 사용법
```Shell
ssh unix061_202155643@164.125.18.136 -p 8022 
```
위의 코드를 통해 접속이 가능하다.

### 명령 사용법
```
ctrl + w -> 명령에서 한 단어 삭제
ctrl + u -> 명령 한 줄 전체 삭제
```
### 명령어의 구조
**명령 [옵션] [인자]** 의 구조를 가지고 있다. 아래는 기초 명령 사용법들이다.
```
banner
date
Clear
man
ls
```
이 명령어들은 모두 옵션과 인자를 넣을 수 있다. 예를 들어서
```Bash
ls -alrth ./
```
라는 명령어는 리스트를 보여주는 명령어에 옵션인 all, list, reverse, time, humanreadable을 의미하는 옵션들이다. 또한 인자는 현재 디렉토리이다.



# CH02

## 유닉스 파일 시스템

### 파일의 종류
1. 일반파일
   1. 텍스트 파일
   2. 바이너리 파일(실행 파일)
       
2. 디렉토리 파일

3. 심몰릭 링크 파일
    1. 소프트 링크 파일
   2. 하드 링크 파일

### 절대 경로와 상대 경로

#### 절대 경로
**절대경로는 루트 디렉토리를 기준으로 하여 현재 워킹 디렉토리사이의 모든 디렉토리의 이름을 표시한다. 항상 /로 시작한다.**

#### 상대 경로
상대 경로는 워킹 디렉토리의 위치를 기준으로 하여 하위로 내려갈 때는 이렉토리의 이름을 상위로 올라갈 때는 .. 로 이동한다. 슬래시 이외의 문자로 시작한다. 

### 디렉토리 명명 규칙 
**/는 루트 디렉토리 이므로 사용이 불가하다.** 알파벳과 숫자, 하이픈, 밑줄 그리고 점은 사용이 가능하다. 공백과 특수기호들은 사용이 가능하다 해당 기호 앞에\를 추가해야 되므로 자제해야 한다.

#### 현재 워킹 디렉토리 확인
```Bash
pwd
``` 
pwd는 presnet working directory를 의미하며 현재 디렉토리를 출력한다.

#### 디렉토리 이동
디렉토리 이동은 cd<sup>[1](#footnote_1)</sup>[디렉토리명]으로 된다. 따라서 디렉토리 명은 상대 경로 및 절대 경로 모두 될 수 있으며 상대 경로의 경우 ../이나 하위디렉토리 명으로 이동할 수 있고 절대 경로의 경오 /directory/name/.../ 등을 통해 원하는 디렉토리로 이동할 수 있다.

<a name="footnote_1">1</a>: cd는 current directory의 약자이다.
```Bash
cd ../
```
위의 명령어는 상위 디렉토리로 이동하는 명령어이다.

### 디렉토리 파일 목록 확인
파일 목록은 ls<sup>[2](#footnote_2)</sup> [option] [file or directory]로 출력할 수 있다.
```Bash
ls
```
ls는 옵션이 여러가지가 존재한다.
* -a(all), 해당 디렉토리에 존재하는 숨김 파일까지 포함하여 모든 파일을 출력한다. 
  ```Bash
    ls -a
    ```
* -l(long), 파일의 상세 정보를 길게 출력한다.
  ```Bash
    ls -l
    ```
* -d(directory) 지정한 디렉토리 자체의 정보를 출력한다.
  ```Bash
    ls -d
    ```
* -R(Recursive) 하위 디렉토리의 파일까지 모두출력한다.
  ```Bash
    ls -R
    ```
* -r(reverse)  역순, 파일을 default의 역순으로 출력한다.
  ```Bash
    ls -r
    ```
* -F(File) 파일의 종류를 표시하여 출력한다.
  ```Bash
    ls -F
    ```
* -u 파일, 디렉토리의 이름을 최종 접근 시간 순으로 화면에 출력한다.
  ```Bash
    ls -u
    ```
* -h(human), 사람이 읽기 편하게 단위들을 이용하여 크기를 출력한다.
  ```Bash
    ls -h
    ```
* -t(time & date), 순서를 생성일을 기준으로 출력한다.
  ```Bash
    ls -t
    ```
위의 명령어들의 옵션은 섞어서 사용이 가능하다.
```Bash
ls -alrth
```
위 명령의 경우는 파일 목록을 출력하되 모든 파일을 상세 정보를 사람이 읽기 편하게 생성일의 역순으로 출력하라는 뜻이다.  

또한 인자에는 원하는 디렉토리를 넣을 수 있다.
```Bash
ls -alrth ../ch02
```
<a name="footnote_2">2</a>: ls는 list의 약자이다.

#### 디렉토리 생성
make directory를 의미하는 mkdir명령어를 사용한다. mkdir [option] [directory name(args)]의 구조로 사용된다. 옵션으로는 -p를 이용하여 디렉토리 생성에 필요한 하위 디렉토리도 함게 생성하는 옵션이 있다.

```Bash
mkdir -p ch02_1/ch02_1_1
tree
```

#### 디렉토리 삭제
remove directory의 의미로 rmdir명령어를 이용한다. rmdir [option] [directory name(args)]의 구조로 사용한다. directory name에 있는 디렉토리를 삭제하며 **디렉토리가 비어있어야 삭제가 가능하 git pull --rebase다.** mkdir와 똑같이 -p 옵션을 통해 부모 디렉토리까지 같이 삭제할 수 있다.

```Bash
rmdir -p ch02_1/ch02_1_1
tree
```

# ch03

## 파일 내용 보기

### cat
> cat [option] file name(args)

간단한 파일 보기 명령어이다. 옵션으로는 -n을 이용하여 행 번호를 옆에 붙여서 출력시킬 수 있다.

```Bash
echo "Hello\nDear\nUnix" > test.txt
cat -n test.txt
rm test.txt
```

### more
> more [option] filename

파일의 내용을 한 화면씩 출력한다. 옵션으로는 + 행 번호를 통해 지정한 행부터 출력할 수 있다.

#### more 명령어
* 다음 페이지 : space
* 이전 페이지 : b
* 종료 : q
* 문자열 찾기 :/문자열

```Bash
more +2 ch03.md
```

### tail
> tail [option] filename

파일의 마지막 부분을 출력하는 명령어이다. 옵션은 아래와 같다. 

* +행 번호: 지정한 행부터 끝까지 출력
* -숫자: 화면에 출력할 행 수
* -f: 파일 출력이 종료되지 않고 주기적으로 반복 출력

```Bash
tail +32  ch03.md 
```
+행번호와 나머지 옵션은 혼용이 불가능하다.
```Bash
tail -5 ch03.md
```
# CH06

## 파일의 속성

### 파일의 종류와 속성
<img src="../file.png" alt="IMAGE">

### 파일 접근 권한
접근 권한은 파일 권한과 디렉토리 권한으로 나뉜다. 이 둘의 기본 권한은 읽기, 쓰기, 실행 권한이다.

#### 접근 권한 표기 방법
> rw-|r--|r--

첫 번째 부분은 소유자, 두번째는 그룹 사용자, 마지막은 기타 사용자의 접근 권한을 나타낸다.
> rwxr--r--

위의 접근 권한은 소유자는 읽기, 쓰기, 실행 모두 가능하고 그룹 사용자와 기타 사용자는 읽기만 가능함을 나타낸다.

### 기호를 이용한 파일 접근 권한 변경
> chmod [ option ] mode filename

change mode, 자신이 소유한 파일의 사용 권한을 변경한다. 옵션으로는 -R(Recursive)를 이용해 하위 디렉토리까지 모두 변경하는 기능이 있다.

mode는 사용자 카테고리, 연산자, 권한 3 부분으로 나뉜다. 
사용자 카테고리
* u : 소유자
* g : 그룹
* o : 기타 사용자
* a : 모든 사용자

연산자 기호
* + : 허가권 부여
+ - : 허가권 제거
- = : 특정 사용자에게 허가권 지정

권한 기호
* r : 읽기 허가
* w : 쓰기 허가
* x : 실행 허가


### 숫자를 이용한 파일 접근 권한 변경
rwx를 111, ---을 000으로 생각한다면 이를 8진수로도 나타낼 수 있다. rw-는 110이므로 7로 나타낼 수 있다. 이러한 표기법으로 rwxr--r--을 아래와 같이 접근 권한을 나타낼 수 있다.
>844

### 기본 접근 권한 설정
> umask [마스크값]

기본 접근 권한을 출력하거나 변경할 수 있따. 마스크 값은 부여하지 않을 권한을 지정한다. 기본 사용 권한을 XOR마스크이다. 즉 부여하고 싶지 않은 권한을 이진수 자릿수에 1을 넣으면 0이 된다.


## 시험 출제 유형
1. 퍼미션을 보고 어떤 명령어를 수행할 수 있는지
2. 어떠한 명령어를 수행하기 위해서 퍼미션 어떻게 수정해야 하는지
3. umask에 대한 이해
