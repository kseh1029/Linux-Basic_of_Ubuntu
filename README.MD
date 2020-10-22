# 리눅스에 대한 기본적인 사용방법을 정리하였다.

대부분의 리눅스 명령어는 비슷하지만 아래의 명령어는 Ubuntu 기준이다.

우분투는 최초 설치시 보안상의 문제로 root 계정이 비활성화 되어있다!

계정을 활성화 하기 위해서는 터미널에 sudo passwd root 를 입력해서 비밀번호를 설정하자!

# 터미널 기본 명령어

# ls

- list directory contents

디렉토리의 내용 출력

ls -l

파일들의 자세한 내용까지 출력할 수 있다 (long)

ls -a

숨겨진 파일들까지 출력한다 (all)

(파일이름 앞에 .이 붙으면 숨겨진 파일)

* ls -al 로 동시에 사용이 가능.

# cd

change directory

cd ..

상위 디렉토리로 이동한다

cd

홈 디렉토리로 이동한다

cd ~ 도 마찬가지

cd /

최상위 디렉토리로 이동한다 (root directory)

# pwd

- print name of current/working directory

print working directory

현재 디렉토리의 위치 출력

# who

- show who is logged on

현재 접속하고 있는 유저들 출력

# whoami

- print effective userid

자신의 유저명 출력

# clear

- clear the terminal screen

터미널 내용 지우기

# history

- GNU History Library

지금까지 사용한 명령어 출력

프롬프트에서 방향키 위로 불러올 수 있는 명령어 기록들을 출력

# mkdir

- make directories

새로운 디렉토리 생성

# rmdir

- remove empty directories

디렉토리 삭제

# cp

- copy files and directories

파일 복사

# rm

- remove files or directories

파일 혹은 디렉토리 삭제

rm -r

디렉토리와 하위 디렉토리/파일 전부 삭제

rm -f

추가적인 프롬프트 없이 강제 삭제

rm -rf

조합해서 사용 가능

# mv

- move (rename) files

파일 이동/이름 변경

# cat

- concatenate files and print on the standard output

파일 내용 보기/만들기

cat >

파일 만들기

Ctrl + d 로 입력을 중단할 수 있다

cat >>

기존 파일 뒤에 내용 추가

# date

- print or set the system date and time

날짜와 시간을 출력/변경

* date +%A 로 요일만, date +%m 으로 일수만 뽑아낼 수 있음

# cal

- displays a calendar and the date of Easter

달력을 출력

cal <월> <년도> 로

특정 월/년의 달력도 출력이 가능하다

# df

- report file system disk space usage

disk free

디스크 사용량 출력

df -h

용량 단위를 계산해서 사람이 보기 편하게 출력한다 (human)

# free

- Display amount of free and used memory in the system

사용중인/사용가능한 메모리 출력

free -h

마찬가지로 사람이 보기 편하게 계산해서 출력한다

# file

- determine file type

파일의 종류를 출력

# exit

- cause normal process termination

터미널 종료

(맥북은 기본 bash로 가고 거기서 exit하면 마찬가지로 꺼진다)

# more

- file perusal filter for crt viewing

터미널 창 크기에 맞춰서 파일 출력

엔터키로 다음 줄, 스페이스 바로 다음 페이지 확인 가능

# less

- opposite of more

more과 유사하지만 상위호환

more은 아래로만 내려가지만, less는 방향키로 위아래로 이동할 수 있다

more의 메뉴얼을 보면 less의 사용을 권장한다

# ln

- make links between files

파일간의 링크 생성

하드링크가 생성되고, 근본적으로 원본과 같은 파일이다

ln -s

심볼릭 링크 생성

심볼릭 링크는 원본의 바로가기이고 ls -l로 어디를 향하는지 확인할 수 있다

# echo

- display a line of text

텍스트 출력

변수에 값을 넣고 $변수이름으로 변수값을 출력하는 것도 가능하다

변수와 값 사이에 스페이스바가 있으면 에러가 난다

# sort

- sort lines of text files

파일들을 정렬된 순서로 출력

# grep

- print lines matching a pattern

globally search a regular expression and print

패턴에 맞는 내용들을 출력

# shutdown

- Halt, power-off or reboot the machine

리눅스 종료

# head

- output the first part of files

파일의 첫 부분 출력

기본은 10행까지 출력

head -n

n행까지 출력

# tail

- output the last part of files

파일의 끝 부분 출력

기본은 10행까지 출력한다

tail -n

마찬가지로 마지막 n행 출력

# tee

- read from standard input and write to standard output and files

배관작업할 때 사용하는 T-Splitter에서 유래했다고 한다

표준입력을 표준출력과 파일에 쓰기

없는 파일은 생성되서 내용이 쓰여진다

# wc

- print newline, word, and byte counts for each file

word count

글자수를 출력

순서대로 행의 수, 단어의 수, 바이트 수

# man

- an interface to the on-line reference manuals

명령어 메뉴얼 출력
