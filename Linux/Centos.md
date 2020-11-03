# CentOS

[http://blog.naver.com/dkzmalfmr/221447382526](http://blog.naver.com/dkzmalfmr/221447382526)

방화벽 풀기

[https://blog.naver.com/res0324/221988642463](https://blog.naver.com/res0324/221988642463)

아파치설치

[https://blog.naver.com/llj2653/221844970828](https://blog.naver.com/llj2653/221844970828)

ntfs 인식

[https://blog.naver.com/speng2/221456432453](https://blog.naver.com/speng2/221456432453)

webdav 설치

- 이런식으로??

    DavLockDB "/tmp/DavLock"

    # 

    Alias /webdav /var/www/webdav/hdd/webdav
    <Location /webdav>
    DAV On
    AuthType Basic
    AuthName webdav
    AuthUserFile /etc/httpd/.htpasswd
    <RequireAny>
    Require method GET POST OPTIONS
    Require valid-user
    </RequireAny>
    </Location>

    Alias /webdav2 /var/www/webdav/hdd/webdav2
    <Location /webdav2>
    DAV On
    AuthType Basic
    AuthName webdav
    AuthUserFile /etc/httpd/.htpasswd
    <RequireAny>
    Require method GET POST OPTIONS
    Require user kseh1029
    </RequireAny>
    </Location>

    Alias /webdav3 /var/www/webdav/hdd/webdav3
    <Location /webdav3>
    DAV On
    AuthType Basic
    AuthName webdav
    AuthUserFile /etc/httpd/.htpasswd
    <RequireAny>
    Require method GET POST OPTIONS
    Require user kseh1029
    </RequireAny>
    </Location>

[https://blog.naver.com/khai01/221647620560](https://blog.naver.com/khai01/221647620560)

permission 문제

[https://blog.naver.com/phongdaegi/221988174349](https://blog.naver.com/phongdaegi/221988174349)

[https://blog.naver.com/slimcdp/221126482353](https://blog.naver.com/slimcdp/221126482353)

Centos7에서 transmission-daemon 패키지로 설치하는 방법

1) yum install epel-release

(EPEL 등록)

2) yum install transmission transmission-daemon

(EPEL을 통해서 다운로드/설치)

3) systemctl start transmission-daemon

systemctl stop transmission-daemon

(초기 settings.json 파일을 생성시키지 위해서 실행/종료)

4) vi /var/lib/transmission/.config/transmission-daemon/settings.json

(설정파일 편집, 편집 전에 반드시 데몬 종료해야 함)

5) vi /사용자/.config/transmission-daemon/settings.json

(사용자 디렉토리 파일도 편집 또는 복사 또는 심볼릭링크 대체)

6) systemctl enable transmission-daemon

(자동 기동 데몬 등록)

7) systemctl start transmission-daemon

(데몬 시작)

=====================

transmission-daemon 실행 계정 변경(아래 예시는 계정 abcd 기준)

1) 서비스 파일 수정

/usr/lib/systemd/system/transmission-daemon.service 파일을 편집기(vi, nano)로 열어 User 항목을 abcd로 수정

2) 위 1번 파일 소유권 수정 (불필요할수도 있지만..)

chown abcd:abcd /usr/lib/systemd/system/transmission-daemon.service

3) 시스템 데몬 리로드

systemctl daemon-reload

systemctl restart transmission-daemon.service

4) settings.json 확인 및 수정

/home/abcd/.config/transmission-daemon/settings.json 파일 필요시 수정

/var/lib/transmission/.config/transmission-daemon/settings.json 파일도 동일하게 수정

5) 데몬 스타트

systemctl start transmission-daemon.service

**[출처]** [Centos7 transmission-daemon 설치](https://blog.naver.com/k000305/221672694018)|**작성자** [샘틀지기](https://blog.naver.com/k000305)