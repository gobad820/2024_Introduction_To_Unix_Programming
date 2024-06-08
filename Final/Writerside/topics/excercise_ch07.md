# ch07 실습

### egrep

```Bash
egrep '(naver|daum)\.com' grep.dat
```
정규식에서 "."는 한글자를 의미하므로 \를 붙여 문자로 인식하게 해야 한다. 이 때 \ 없이도 www.naver.com이 검색되는데 이것은 문자 "." 한 글자를 .가 인식하기 때문이다. 따라서 www.naverAcom도 똑같이 검색이 된다.

```Bash
egrep '^ftp' services
```
services문서에서 ftp로 시작하는 문자열을 검색한다. ^는 시작을 의미한다.

```Bash
egrep 'ftp$' services
```
services문서에서 ftp로 끝나는 문자열을 검색한다. $는 끝을 의미한다.


```Bash
egrep '^ftp[a-z]' services
```
services문서에서 ftp 그리고 알파벳 소문자 한 글자로 이루어진 문자열로 시작하는 부분을 검색한다. ^는 시작을 의미한다.

```Bash
egrep 'x11.*tcp' services
```
services문서에서 x11이라는 문자열이 나오고 그 뒤에 어떤 문자열이든 나오고 그 뒤에 tcp가 나오는 문자열을 검색한다. .*는 0회 이상 반복을 의미한다.

```Bash
find ~ -type f -mtime -8
```
현재 디렉토리 및 하위 디렉토리에서 8일부터 현재까지 이내에 수정된 파일을 검색한다. -mtime은 수정 시간을 의미하고 -8은 8일 이내를 의미한다.
### -exec


```Bash
find t3 -exec rm {} \;
```
t3 디렉토리의 파일을 모두 삭제한다. -exec는 명령어를 실행하라는 옵션이다. {}는 검색된 파일을 의미하고 \;는 명령어의 끝을 의미한다.

```Bash