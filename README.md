### 리눅스 명령어와 vim 에디터 매크로 조사

## 리눅스 명령어 top
>실시간으로 CPU 사용률 체크를 해주는 도구
>
>리눅스를 사용하는 디바이스의 성능이나 리눅스 서버의 성능을 체크할 때 매우 유용


시스템의 상태를 전반적으로 가장 빠르게 파악 가능(CPU, Memory, Process)
옵션 없이 입력하면 interval 간격(기본 3초)으로 화면을 갱신하며 정보를 보여줌
|top 실행 전 옵션|
|---|
순간의 정보를 확인하려면 -b 옵션 추가(batch 모드)
-n : top 실행 주기 설정(반복 횟수)

|top 실행 후 명령어|내용|
|---|---|
|shift + p | CPU 사용률 내림차순|
|shit + m |메모리 사용률 내림차순|
|shift + t | 프로세스가 돌아가고 있는 시간 순|
|k | kill. k 입력 후 PID 번호 작성. signal은 9|
|f | sort field 선택 화면 -> q 누르면 RES순으로 정렬|
|a | 메모리 사용량에 따라 정렬|
|b | Batch 모드로 작동|
|1 | CPU Core별로 사용량 보여줌|

![image](https://user-images.githubusercontent.com/106910099/172047687-e554b1f4-9bc0-44ed-9429-bc2940b5b4a0.png)

16:25:10 > 현재 서버의 시간

1user > 한명의 사용자가 접속

load average : 부하율

tasks 에서 259 total은 257개의 프로세스가 가동중

2 running 2개의 프로세스가 실행중

257 sleeping : 257개의 프로세스가 대기중

0 stopped : 0개의 프로세스가 멈춤

0 zombie : 0개의 프로세스가 좀비상태

|CPU|내용|
|---|---|
|%us|  유저 레벨에서 사용하고 있는 CPU의 비중|
|%sy | 시스템 레벨에서 사용하고 있는 CPU비중|
|%id | 유휴 상태의 CPU 비중|
|%wa | 시스템이 I/O 요청을 처리하지 못한 상태에서의 CPU idle 상태인 비중|

## 리눅스 명령어 ps
>현재 실행중인 프로세스 목록과 상태를 보여줌

ps의 옵션은 전통적인 유닉스인 System V,BSD,GNU에 따라 결과가 다르게 나타나고 표기법에도 차이를 보임

옵션 사용시 System V 계열은 -를 사용하고 BSD는 -를 사용하지 않음,GNU에서의 옵션 표기는 -- 를 사용함

### > 원하는 프로세스의 상태를  출력하려면 정확한 옵션 사용이 중요함

|옵션|내용|
|---|---|
|-A|모든 프로세스 출력|
|a(BSD계열)|터미널과 연관된 프로세스를 출력|
|-a|세션 리더를 제외하고 터미널 에 종속되지 않은 모든 프로세스를 |
|-e|커널 프로세스를 제외한 모든 프로세스 출력 |
|-f |풀 포멧으로 보여줌|
|-l|긴 포멧으로 보여줌|
|-o|출력 포멧을 지정함|
|-M|64비트 프로세스들을 보여줌|
|-m|프로세스 뿐만 아니라 커널 스레드들도 보여줌|
|-r|현재 실행 중인 프로세스를 보여줌|
|-u|특정 사용자의 프로세스 정보를 확인함, 지정하지 않으면 현재 사용자를 기준으로정보를 출력|
|-x|로그인 상태에 있는 동안 아직 완료되지 않은 프로세서들을 보여줌|

### ps 명령어 예시

#### $ ps ax
시스템에 동작중인 모든 프로세스를 보고 싶을때 이 명령어를 사용하면 BSD 포멧으로 출력

PID,TTY,STAT,TIME,COMMAND 의 정보가 뜸

#### $ ps aux
시스템에 동작중인 모든 프로세스를 소유자 정보와 함께 BSD 포멧으로 출력
PID,TTY,STAT,TIME,COMMAND를 사용자 기준의 정보로 출력

#### $ ps aux|grep apache
특정 프로세스에 대해서 보고 싶은경우 'grep'명령어 활용

#### $ ps -ef|more
'ps -ef'는 System V 계열 옵션으로 시스템에 동작중인 모든 프로세스를 풀 포멧으로 출력함

## 리눅스 명령어 jobs
> 작업의 상태를 표시하는 명령어
> 
> 현재 쉘 프로세스의 지식 백그라운드 프로세스들을 보여준다고 생각하면 됨

### jobs로 출력되는 백그라운드 작업의 상태값


|상태|내용|
|---|---|
|Running|작업이 계속 진행중임|
|Done|작업이 완료되면 0을 반환|
|Donce(code)|작업이 종료되었으면 0이 아닌 코드를 반환|
|Stopped|작업을 일시 중단|
|Stopped(SIGTSTP)|SIGTSTP 시그널이 작업을 일시 중단|
|Stopped(SIGSTOP)|SIGSTOP 시그널이 작업을 일시 중단|
|Stopped(SIGTTIN)|SIGTTIN 시그널이 작업을 일시 중단|
|Stopped(SIGTTOU)|SIGTTOU 시그널이 작업을 일시 중단|

### 옵션

|옵션|내용|
|---|---|
|-l|프로세스 그룹 IDFMF state 필드 앞에 출력|
|-n|프로세스 그룹 중에 대표 프로세스 ID를 출력|
|-p|각 프로세스 ID에 대해 한 행씩 출력|
|command|지정한 명령어를 실행|


## 리눅스 명령어 kill 
>프로세스를 죽일때 사용함

### 프로세스를 죽이는 법
죽이려는 프로세스의 pid를 얻은 다음 kill명령어의 인자를 넘기면 됨
### 사용자 지정 시그널 전송 방법
kill 명령어의 defalut 시그널은 TERM(15)이지만 -S 명령으로 다른 시그널을 보낼 수 있음

-l 명령어를 통해 지원하는 시그널의 목록을 확인 할 수 있음
![image](https://user-images.githubusercontent.com/106910099/172049265-587fd4f8-52ff-4c17-8871-c9008ad537bf.png)

## vim 에디터에서 매크로 사용방법

#### 매크로 시작 q[name]

#### 매크로 입력

#### 매크로 종료 q

#### 매크로 실행  @[name]

#### 매크로 여러번 실행 [number]@[name] 
