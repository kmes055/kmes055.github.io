---
title: "[교육후기]Bash Tutorial"
categories:
  - review
tags:
  - 교육
  - Linux
---

# Bash Tutorial

`교육 내용 출처: NHN 기술교육 - bash tutorial (조영일 이사님)`

Bash를 이용한 linux programming은 현업에서 일하면서 꾸준히 배워야 할 일이고, 지금 잘 배워둬서 앞날을 도모합시다!

## 소개
### Unix Shell
* kernel을 둘러싸고 있는 껍데기
* 사용자가 제어할 수 있는 명령창
* Windows에서는 cmd창
+) sh는 787년, bash는 89년에 만들어짐

## 커서 이동
* 문장의 처음 끝
    * Ctrl-a / Ctrl-e
* 단어 단위 이동
    * Ctrl-방향키
    * 환경의 영향을 많이 받아서, 안 될 확률이 높음
* 화면 정리
    * Ctrl-l
* 스크롤 중단 및 재개
    * Ctrl-s / Ctrl-Q

## 서버 접속 및 종료
* 접속
    * ssh hcon.nhnent.com
    * rlogin -l 아이디 호스트명
* 종료
    * logout / exit
* 조회
    * last

## 기본 명령
### pwd
* 현재 디렉토리를 확인하는 명령
### id
* 자신의 로그인 아이디를 확인하는 명령
### uname
* 현재 접속해있는 서버의 이름/OS정보를 확인하는 명령
* uname
* uname -a
### 비슷한 명령
* hostname
### echo
* 문자열을 출력하는 명령
* echo -n "hello world"; echo "java coffee"
    * -n은 마지막 newline을 없애줌
### ls
* 디렉토리와 파일의 목록을 출력하는 명령
* Windows cmd의 dir
* -1: 파일 하나당 1줄
* 그 외 a(숨김파일도), l(list format으로), F(특수기호를 써서?), t(시간순 정렬) 등의 옵션이 있음

### Globbing & Wildcard
* 임의의 길이를 가지는 문자열: *
* 임의의 한 글자: ?
* 글자 집합: [abcw-z] // regex보단 저레벨

## File System
### File
* OS에서 데이터를 디스크와 같은 저장소에 둘 때의 단위 형태
* 넓은 의미에서는 파일, 디렉토리, 입출력 장치까지 파일로 간주
### Directory
* Windows의 폴더. 파일을 담아두는 공간
* 트리 구조로 형상화
### 디렉토리 이동
* cd \<path>
* 사용자 홈 디렉토리: cd ~/.vim
* 특정 사용자의 홈 디렉토리: ls ~\<username>

## History
### 조회
* history
### 12번 명령 재실행
* !12
### 커맨드 prefix로 재실행
* !find
* pwd, cd 사용 후 !pw 를 입력하면 pwd가 실행됨
### 바로 직전 명령 재실행
* !!
### 바로 직전 명령의 마지막 argument
* !$
* ls로 특정 위치 찾은 뒤 cd !$하면 이동 가능
### 직전 명령 편집
* echo "hello world"
* ^hello^java

### 파일 다루는 기본 명령
* touch : 새로운 파일 만들어줌
* mkdir/rmdir : 디렉토리 생성/삭제
* rmdir은 빈 디렉토리만 삭제
* cp : 파일 복사
* mv : 파일 이동
* 이때 파일 1, 2, 3, 4, 5와 디렉토리 d에 대해 `cp 1 2 3 4 5 d` 이런 식으로 적을 수 있음
* rm
    * rm -f test2.txt (false)
    * rm -rf mydir2 (recursive false)
    * rm이 워낙 위험하다보니 alias로 `rm='rm -i'` 가 걸려있음
    * rmdir로 안 지워지는 디렉토리도 rm -rf로 지워짐
* file
    * 파일의 종류를 보여주는 명령
    * 기본적으로 종류별로 매직 넘버가 존재?
    * `file sample.txt` 등의 방법으로 사용

여기까지가 기본 지식

## 변수
* 사용자 변수
    * 보통 소문자로 명명
    * equal 앞뒤로 공백이 없는 거 주의
    * x=1
    * x="hello world"
    * x=hello world // world: command not found
* 값을 꺼내쓰기
    * echo x    x
    * echo $x   hello world
    * echo ${x} hello world  // recommended
    * echo "$x" hello world
    * "$x"는 $x를 double quote로 감싸는 뿐이라서, 틀린 건 아닌데 dereferencing 입장에서는 ${x}가 명확
* Full Quotation
    * single quote as literal
    * x="java"; echo '$x is noun'   $x가 출력됨
* Partial Quotation
    * double quote as variable substitution
    * x="java"; echo "$x is noun"   Java가 출력됨
    * 내부에 있는 문자를 해석하냐 마냐가 full quotation과의 차이
### Evaluation
* 명령 실행 결과를 문자열 변수로 대임
    * backquote 또는 $()로 둘러싸기
    * data_str=\`"date +"%Y%m%d"\`
    * data_str=$(date +"%Y%m%d")
    * data +... 공백 주의
* 명령 문자열을 실행하기
    * eval \<command>
    * `cmd="ls -alF"`
    * `eval $cmd`
### Arithmetic: 권장되진 않음
* `a=13`
* `b=$((a % 3))`
* `i=$((2 ** 60))`
* `echo $RANDOM`
### Type : 권장되지 않음
* read-only
    * declare -r var1=1
    * var1=2
* Integer
    * declare -i n=3
    * n="hello"
    * echo $n
### 환경 변수 (중요!)
* 환경
    * 프로세스를 둘러싼 주변 정보
    * env
* exporting
    * sub-shell에서 상속받아 사용할 수 있도록 변수를 공개
    * 사용자 변수를 환경 변수로 전환
    * export나 declare -x로 지정
    * `export REACT_NATIVE_PACKAGER_HOSTNAME=<your_ip_address>`
    * var1=2; bash; echo $var1; exit
    * export var1=2; bash; echo $var1; exit
    * 위 두 개 비교해보기
* 주요 환경 변수
    * HOME PATH LANG LC_ALL TERM SHELL HOSTNAME LONGNAME USER USERNAME ID UID EDITOR VISUAL PS1 PS2
    * 많다...
    * 그 외 개발자에게 유용한 환경 변수
    * SHLVL PPID SECONDS FUNCNAME TMOUT


## 파일 permission
`ls -al` 을 실행해보면 drwxrwxrwx부터 ---------- 까지 존재(실제로 drwxrwxrwx가 있지는 않음). 첫 문자는 -dlbcps 등이 있고 파일의 종류를 나타냄.
이후 3개의 문자별로, owner, group, others에 대한 read/write/execute 권한이 rwx로 나타남

* vi test1.sh
* ls -al !$
* chmod 0644 !$
* ls -al !$
* chmod u+x test1.sh

* umask
    * 파일에 부여되는 permission의 초기값에서 제와할 권한 지정
    * 이후 생성되는 파일에 대해 해당 권한들이 제외됨
* 소유자 변경하기
    * chown 소유자아이디 파일

## 제어문
* Boolean expression
    * &&, ||, !, -a -o
    * (( 0 && 1 )) && echo true || echo false
    * (( 0 || 1 )) && echo true || echo false
* Comparison operator
    * Integer
    * -eq -ne -gt -ge -lt -le
    * string
    * < <= > >= == = != -z(zero length) -n(null)
* File Test
    * -e    exist?
    * -s    size not zero?
    * -f -d -p -L   file, directory, pipe, symlink?
    * -r -w -x -u -g -k     Permission? (setuid, setgid, sticky)
    * -nt -ot   newer than, older than?
* 여기 나오는 제어문들은 다 괴랄함...
* 제어문
    * if [condition]; then; 실행문; fi
    * 주의: -eq는 정수 연산자, 정의되지 않은 변수나 공백을 포함할 수 있으므로 quotation 필요. "$x" 안해주면 x가 공백을 포함한 문자열이면 에러남
    * while [condition]; do 실행문; done
    * while :; do 실행문 date; sleep 5; done
    * for 변수명 in 리스트; do 실행문; done
    * switch case 변수 in case1) 실행문;; case2) 실행문;; *) 실행문;; esac
    * switch는 너무 어려워서 필요할 때만 찾아서 쓰자... unix 기본 패턴(glob pattern과 regex 사이쯤?)이 case에 들어갈 수 있음
    * break continue는 다 알테니 넘김

## 입출력 리다이렉트
* 표준 입력(standard input) - 0번
    * 키보드 입력이 프로세스로 들어가는 통로. stdin
* 표준 출력(standard output) - 1번
    * 프로세스가 출력한 내용이 화면으로 나가는 통로. stdout
* 표준 에러(standard error) - 2번
    * 프로세스에서 발생한 에러가 화면으로 나오는 통로. stderr
* bash 내에서 <, >로 사용
* 2> 를 쓰면 stderr로 들어감
+) more/less는 cat하고 비슷한데, 페이지 단위 뷰어? 임
## 필터
* 프로세스의 출력을 다른 프로세스의 입력으로 전달
    * Pipe(|) 기호를 사용
    * ls | head
    * cat text.txt | sort | uniq -c | sort -n

## 파일 다루기
### 파일 내용 보기
* 파일의 앞쪽 일부만 보기
    * head -10 test1.txt
* 파일의 뒤쪽 일부만 보기
    * tail -f /var/log/lastlog
### 파일에서 문자열 찾기
* 찾기
    * grep 문자열 파일
    * grep "export" /etc/profile
    * grep -r "export" /etc

* 잘라내서 쓰기
    * cut -d 구분자-f 필드번호 파일
    * grep some_pattern /sample/path | cut -d: -f1,3-4,7
+) bash와 같이 서브루틴, 서브프로세스 등을 만들어서 커맨드를 실행시켜주는 걸 glue language라 부름? // 찾아보기
## Alias
* 별명
    * alias
    * alias grep="egrep"
    * alias diffs="diff -ignore-all-space"
    * alias l="some_text"

## 함수
* Bash는 function과 subroutine을 구분하지 않음
* func1() { echo "abc $1" }
* func1 "def"
* function func2 { return $(($1 + $2)); }
* func2 3 4
* echo $? // 커맨드 실행 결과를 봄
* 프로그래밍 언어론적으로 바람직하진 않음
* 변수는 기본적으로 전역
* 함수 내부에서만 사용하려면 local로 지정해야 함
* function test1() { local int_val=3; str_val="value of global variable"; }
* test1
* echo $int_val; echo $str_val;
## Script File
* Shebang(sharp bang의 줄임말?)
    * #!: 어떤 인터프리터 프로그램을 사용할 것인가를 지정
    * #!/bin/bash
    * #!/usr/bin/env python     권장됨. 환경 변수에서 찾아서 쓰는 명령어
    * #!/bin/awk -f     옵션을 붙여야 할 경우, env를 사용할 수 없음
* Command-line arguments
    * $1, $2, ...
    * $#
    * $?    리턴값
    * Function과 완전히 일치
## Job Control
* Foreground job
    * 기본적으로 프로그램은 foreround에서 실행됨
    * 한 명령의 실행이 끝날 때까지 다음 명령은 기다려야 함
* Background job
    * &를 붙여서 실행하면 background에서 실행됨
    * 다음 명령을 바로 실행하거나 프롬프트가 뜸
* sleep 500 &
* jobs
* fg %1
* Ctrl-z
* bg
* wait
## 설정파일
### 사용자 설정
* Bourne shell
    * ~/.profile
* BASH
    * login shell: .bashrc      로그인 이후 동작을 정의해두면 좋음
    * non-login shell: .bash_profile    실행 이후 동작을 정의해두면 좋음
* 파일 변경 후에는 재접속하거나 source ~/.bashrc ? 실행해주면 적용됨
* 추천 설정... 자료 찾아보기
## 자주 사용하는 명령(시간 남으면)
* 디렉토리 이름 구하기: `dirname`
* 파일 이름 구하기: `basename`
* 화면 청소하기: `clear`
* 실행 가능한 프로그램 확인: `which` (PATH환경변수에 포함돼야 탐색 가능)
* `locale`: 지역 특성에 따르는 언어/날짜 포맷 등을 통틀어서 부르는 용어?
* `iconv` : 옵션 필수. 
* 정렬: sort
* 워드수: wc
* 파일 탐색: find. 조건이 and인데, or로 하려면<br>
 `find ... \(-name "python*" -o -name "pgsql*" \) -ls` 이런 식으로 -o를 넣어서 넣어주면 됨
 * 입력을 개별 처리하기: xargs
    * for 반복문은 공백이 포함된 입력을 처리하기 곤란함
    * xargs를 이용하여 행 단위로 처리하기
    * `find /etc/init -print0 | xargs -0 wc -l`
    * -print0는 공백을 제거하기 위함?
* 사용자 변경 : `su`
* 루트 권한 실행: `sudo`: 1-1024 포트를 잡거나 할 때는 80, 443 등의 포트도 잡아야 해서 root 권한이 필요함
* 패스워드 변경: `passwd`
* 원격 제어
    * rsh -l
    * rcp, scp
    * ftp, sftp, rsync
* 파일 묶기
    * tar
    * ctxvfzj 등 옵션 다양
* 파일 압축
    * zip, unzip
    * unix에선 zip 안 쓸때도 많음
* 프로세스 관련 명령
    * `ps aux / pstree`
    * `pidof`
    * `kill`
* 시스템 관리 명령
    * 시스템 자원 사용현황 조회: `top`
    * 디렉토리의 크기 조회: `du -s`
    * 디스크 전체 사용현황 조회: `df -k`
* 네트워크 관련 명령
    * ifconfig
    * ping
    * nslookup => getent hosts
* 매뉴얼 페이지
    * man wc
    * man 2 open
    * man 3 opendir
* 특수 명령
    * 회사나 단체체별로 특수한 명령이 있을 수 있음
    
