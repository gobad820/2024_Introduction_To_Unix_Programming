# Chapter08 실습

## 프로세스

```Bash
ps -ef
```

모든 프로세스를 보여준다.

```Bash
sleep 200
```

200초동안 sleep을 실행한다.

```Bash
kill -15 'PID"
```

PID에 해당하는 프로세스를 종료한다.

```Bash
sleep 200&
```

200초동안 sleep을 실행한다. 백그라운드로 실행한다.

```Bash
kill -9 'PID"
```

PID에 해당하는 프로세스를 강제 종료한다.

```Bash
./Hello > out &
```

Hello 프로그램을 실행하고 out파일에 출력한다. 백그라운드로 실행한다.
