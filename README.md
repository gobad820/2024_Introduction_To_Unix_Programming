# Chapter07

## 파일 내용 검색

### grep [옵션] 패턴 파일명

grep: Global Regular Expression Print

```Bash
echo 'test' > test.txt
grep 'test' test.txt
rm test.txt
```

#### 옵션

- -i:  case Insensitive(대소문자 구분 x)
- -l: 해당 패턴이 들어있는 파일 이름을 출력
- -n: 각라인이ㅡ 번호도 함꼐 출력
- -v: 명시된 패턴과 일치하지 않는 줄을 출력
- -c: 패턴과 일하는 라인수 출력
- -w: 패턴이 하나의 단어로 된 것만 검색

#### 정규표현식

정규표현식은 특정한 규칙을 가진 문자열의 집합을 표현하는데 사용하는 언어이ㅏㄷ.

**정규 표현식의 구성요소**

- 앵커: 검색시 한 줄에서 패턴의 위치를 표현. e.g. ^, $
- 문자 집합: 하나이상의 문자들을 표현. e.g. 알파벳, 숫자, ., []..
- 변환자: 이전 문자 집합의 반복 횟수 지정

**정규 표현식을 표현하는 특수 문자**

- ^: 라인의 시작
- $: 라인의 끝
- .: 한 글자
- []: 괄호 안의 글자중 하나
- [^]: 괄호 안에 있는 글자가 아닌 글자
- *: 앞의 항목이 없거나 여러번 반복하는 것을 의미

**정규표현의 예**

```Bash
echo 'UNIX\t12345\nunix+\t123\nsystem\tadmin\nNetwork\t5\nroot\tother sh\nsjyoun\tprof ksh\njongwon\tprof KSH\nROOT\tother csh\nck07555\tstudent ksh\nCK08777\tstudent bash' > grep.dat
```

```Bash
grep '^root' grep.dat
```

_정규표현의 경우 꼭 '(작은따옴표)로 감싸 주어야 한다!_

^root라는 정규 표현식을 보면
앵커 ^로 시작한다. ^는 문자열의 첫부분을 의미한다. 그 후 문자 집합 root가 나오고 변환자는 없다.
따라서 해당 명령어의 의미는 h.txt파일에서 문장 시작이 root로 시작하는 부분을 검색하라는 의미이다.

```Bash
grep 'sh$' grep.dat
```

sh\$는 문자열 sh가 먼저 나오고 그 후 앵커 `$` 가 나온다. '$' 는 문장의 끝을 의미하므로 문장이 sh로 끝나는 모든 문장을 검색하게 된다.
따라서 아래와 같이 검색 결과가 나온다. 모든 문장이 sh로 끝남을 알 수 있다.

    root    other sh
    sjyoun  prof ksh
    ROOT    other csh
    ck07555 student ksh
    CK08777 student bash

```Bash
grep 'r..t' grep.dat
```

r..t는 문자열로만 이루어져 있고 .는 한 글자를 의미함을 알 수 있다. 따라서 r로 시작하고 t로 끝나는 글자 중에 r과 t를 포함 4글자인 모든 글자를 검색하게 된다.

    root    other sh

r..t에 만족하는 root가 들어있는 문장이 결과로 출력되게 된다.

```Bash
grep 'oo*' grep.dat
```

oo`*`의 의미를 조금 더 정확히 파악하기 위해 괄호를 이용하여 표현하면 o(o`*`)가 된다. 즉 o 다음에 o가 0번 이상 오는 모든 글자를 찾는 것이다.
따라서 o 한 글자만 있어도 검색이 되고 oo 또는 ooo, oooo, ...와 같이 o뒤에 o가 n번 오는 모든 글자가 검색 된다.

    Network 5
    root    other sh
    sjyoun  prof ksh
    jongwon prof KSH
    ROOT    other csh

여기서 ROOT는 위의 명령어에서 걸린 것이 아니다. grep은 대소문자 구분을 하는 case sensitive이기 때문에 other이 검색 된 것이다. 만약 옵션으로 -i를 넣어주면 ROOT도 검색 대상이 된다.

```Bash
grep '[0-9].*' grep.dat
```

[0-9].\*도 괄호를 이용해서 다시 표현하면 [0-9](../documents/final_test)\*이 된다. 첫글자는 0~9인 숫자이고 뒤에 글자는 아무 글자 한글자가 0번 이상 올 수 있음을 의미한다. 또한 앵커가 없으므로 문장 처음 중간 끝 중
어디에 있어도 상관없다.
따라서 숫자로 시작하는 모든 것들이 검색 대상이 된다.

UNIX    **12345**

unix+   **123**

Network **5**

ck**07555 student ksh**

CK**08777 student bash**

위의 결과에서 볼드처리 한 부분이 [0-9].\*에 해당되는 부분이다. 이를 통해 .는 공백도 포함할 수 있음을 알 수 있다.

```Bash
grep '[^c]sh' grep.dat
```

[^c]sh에서 ^는 앵커가 아니라 c를 제외함을 의미한다. 따라서 c가 아닌 한글자뒤에 sh로 끝나는 문자들이 검색 대상이다.

root other **sh**

sjyoun prof **ksh**

ck07555 student **ksh**

CK08777 student b**ash**

결과에서도 볼 수 있듯이 bash에서 ash만 검색됨을 알 수 있다. [^c]sh는 c가 아닌 한글자로 시작하고 sh로 끝나는 것이기 때문에 bash에서 b는 제외되고 ash만 검색 대상이 된다.

### egrep

egrep: Extended Global Regular Expression Print

grep에서 더 확장된 패턴 표현식 및 특수 문자를 사용할 수 있다.

- +: 앞의 글자가 하나 이상 나옴을 의미한다. 즉 앞의 예제 oo\*는 o+와 동일하다.
- ?: 해당 글자가 없거나 한번 나올 수 있음을 의미한다.
- x|y: x나 y중 하나가 한번 등장한다.
- ( | ): 문자열 그룹을 의미한다.

#### 명령어 예시

```Bash
egrep '[78]+' grep.dat
```

[78]+는 7 또는 8이 한번 이상 등장하는 문자를 검색함을 의미한다. 따라서 결과는 아래와 같다.

ck0**7**555 student ksh

CK0**8777** student bash

첫번째 문장에서는 7만이 검색 대상이 되었고, 두번째 문장에서는 7또는 8이라는 조건에 맞게 8777이 검색 대상이 됨을 알 수 있다.

```Bash
egrep 'csh|bash' grep.dat
```

csh|bash는 csh 또는 bash를 의미하므로 csh, bash가 들어간 부분이 검색 대상이 된다.

ROOT other **csh**

CK08777 student **bash**

### fgrep

egrep이 기본 grep의 정규 표현식을 extend하는 개념이면 fgrep은 정규표현식을 사용하지 않음을 의미한다. 따라서 앞서 봤던 정규표현식의 특수기호들이 일반 문자로 처리된다.

```Bash
fgrep unix+ grep.dat
```

를 해보면 unix+ 123이 출력됨을 알 수 있다. 이 때 검색 대상은 unix가 아닌 unix+까지임을 알 수 있다. +와 같은 특수기호들이 일반 문자로 취급된다는 뜻이다.

## find

    find 경로 검색조건 [동작]

- 경로: 상대경로 또는 절대경로
- 검색조건: 파일을 찾기 위한 검색 기준, and or을 이용하여 결합도 가능하다.
- 동작: 팡리의 위치를 찾은 후 수행할 동작

### 검색 조건

- -name file_name: 이름이 file_name인 파일을 검색한다. 메타문자(\*,?)도 사용이 가능하나 '"' 안에 있어야 한다.
    - 앞의 egrep과 다르게 '(작은따옴표)가 아니라 "(큰따옴표)안에 있어야 한다.
- -type: 이름이 file_name인 파일들 중에 타입이 특정 타입인 파일들만 검색한다.
    - 파일 종류
        - d: 디렉토리 파일
        - f: 일반 파일
        - l: 심볼릭 링크 파일
        - b: 블록장치특수파일
        - c: 문자장치특수파일
        - s: 소켓파일
- **mtime [+|-]n, atime [+|-]n: 수정(접근)시간이 +n일보다 오래되었거나, -n일보다 짧거나 정확히 n일에 일치하는 파일을 검색한다.**([+|-]는 -mtime +8,-mtime -8
  그리고 -mtime 8과 같이 선택할 수 있음을 의미한다.)
    - **mtime = modification time**
    - **atime = access time**
- user my_user: 사용자 계정 my_user가 소유한 모든 파일 검색
- size [+|-]n: +n보다 크거나, -n보다 작거나 정확히 n인 파일을 검색한다.
- newer: 기준시간보다 이후에 생성된 파일을 검색한다.
- perm: 사용 권환과 일치하는 파일을 검색한다.

검색 조건의 결합 기호에는 -a, -o, !가 있고 각각 -a(and), -o(or), !(not)을 의미한다.

### 동작

- -exec 명령 {} ₩;
    - exec 옵션은 무조건 ₩;으로 끝난다. 검색된 파일은 {} 위치에 적용된다.
- -ok 명령 {} ₩;
    - exec의 확인 모드 형태이다. 사용자의 확인을 받아야 명령을 적용할 수 있다. 즉 실행 전 yes/no를 물어본다.
- -print: 화면에 경로명을 출력한다.
- -ls: 긴 목록 형식으로 검색 결과를 출력한다.

### find 검색 조건 예시

**name 옵션**

```Bash
find . -name grep.dat
```

**type 옵션**
find의 기초적인 형태로 find 경로 검색조건으로 이루어져 있으며 후행 동작은 없다.
먼저 경로는 ~로 되어 있고 이는 홈 디렉토리를 의미한다. 따라서 홈 디렉토리에서 -name 파일 이름이 grep.dat인 파일을 검색한다.

```Bash
mkdir testDirectory
find . -type d
```

현재 디렉토리에서 -type d 디렉토리를 모두 찾는 명령어이다. 이 때 testDirectory라는 이름을 가진 디렉토리만 검색하고 싶다면 -name 옵션을 이용하여 검색 조건을 추가해야 한다.

```Bash
find . -type d -name testDirectory
```

**mtime 옵션**

```Bash
find . -mtime -1 -name grep.dat
```

시간 표현 방법은 현재 시각을 기준으로 한다.

- n: n일 전, 즉 (n+1)`*`24 시간 전부터 n`*`24시간전까지 최종 수정 또는 접근된 파일
- -n: n일 이내, n\*24시간 전부터 현재까지 마지막으로 수정 또는 접근된 파일
- +n: n일 이후, (n+1)\*24시간 이전에 마지막으로 수정 또는 접근된 파일을 의미한다.

```Bash
find . -mtime +1 -name grep.dat
```

mtime 옵션을 이용했고 값으로 +1을 넣었으므로 (1+1)\*24시간 이전에 마지막으로 **수정**된 파일 중 이름이 grep.dat인 파일을 검색한다.

```Bash
find . -mtime 1 -name grep.dat
```

mtime의 값이 그냥 1이므로 (1+1)\*24시간 전부터 1\*24시간 전까지 최종 수정된 파일을 중 이름이 grep.dat인 파일을 검색한다.

```Bash
find . -mtime +1 -name grep.dat
```

mtime의 값이 +1이므로 (1+1)\*24시간 이전에 수정된 파일 중에서 이름이 grep.dat파일을 검색한다.

즉 시간 표현을 아래와 같다. 인덱스 0이 과거이고 끝이 현재라면

- n: [ (n+1)x24 : nx24 ]
- +n: [ 과거 :(n+1)x24 ]
- -n: [ nx24 : 현재 ]

**검색 조건의 결합**
```Bash
find . -type d -o -name grep.dat
```

현재 디렉토리에서 타입이 -d(디렉토리)**이거나(Or)** 이름이 grep.dat인 파일을 모두 검색한다. 

```Bash
find . -type d -a -name testDirectory
```

현재 디렉토리에서 타입이 디렉토리**이고(And)** 이름이 testDirectory인 파일

```Bash
find . -type d ! -name grep.dat
```

현재 디렉토리에서 타입이 디렉토리이고 이름이 grep.dat이 **아닌** 파일을 검색한다.

나머지 검색 옵션들은 실습시간에 하지 않았다!

### find 후행 동작 예시
```Bash
echo 'hello World!' > testfile.txt
find . -user $USER -a -name testfile.txt -exec cat {} \;
```

hello World!라는 텍스트를 testfile.txt에 넣어 생성하고 find 명령어를 통해 생성자가 $USER 이고 이름이 testfile.txt인 파일을 검색 후 -exec 옵션을 통해 cat {} 즉 해당 파일을 cat하도록 하였다.
{}에는 find로 검색한 파일의 이름이 대입됨을 알 수 있다.
따라서 결과는 

    hello World!

가 된다.

## which
    which 명령

명령어 파일의 위치를 차장서 경로나 alias를 출력해준다. 

```Bash
which ls
```# Chapter08

## 프로세스의 개념과 종류


## 프로세스 관리 명령어

## foreground와 background의 프로세스

## 사용자 정보 보기
