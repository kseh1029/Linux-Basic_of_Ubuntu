# Ubuntu

어떤 Tweak 을 설치하기 전 apt update 는 습관적으로 하는것이 좋다.

```bash
​sudo apt update
sudo apt upgrade
```

- **Apache2 설치하기**

    ```bash
    apt install apache2
    ```

- **PHP 설치하기**

    ```bash
    sudo apt-get install php libapache2-mod-php php-xml php-gd php-mysql
    sudo apt-get install php7.2-mysql php7.2-curl php7.2-xml php7.2-zip php7.2-gd php7.2-mbstring

    ** 2020년 6월 기준 php7.2 안되고 7.4 깔아야하네
    ```

- **Apache2 를 이용해 WebDAV 서버 활성화**

    ```bash
    sudo apt install cadaver

    WebDAV 에서 사용할 디렉토리 생성 및 소유권 수정, WebDAV Enable
    WebDAV로 사용할 디렉토리는 /var/www/webdav 이라고 할 때
    cd /var/www
    sudo mkdir webdav
    sudo chmod 707 webdav

    sudo a2enmod auth_digest
    sudo a2enmod dav
    sudo a2enmod dav_fs

    sudo htdigest -c /etc/apache2/users.password webdav [WebDAV용 사용자 계정 이름]
    (WebDAV용 계정을 생성하고, 생성한 계정의 비밀번호를 설정한다)

    cd /etc/apache2/sites-available
    sudo gedit 000-default.conf

    아래의 내용으로 편집한다.

    <VirtualHost *:80>
     **ServerName name                               ## << 수정
     ServerAdmin webmaster@localhost**
     DocumentRoot /var/www/html

     ErrorLog ${APACHE_LOG_DIR}/error.log

     CustomLog ${APACHE_LOG_DIR}/access.log combined

    **# 여기부터 - 추가할 부분**

     <Directory /var/www>
      Options Indexes FollowSymLinks Multiviews
      AllowOverride all
      Order allow,deny
      allow from all
     </Directory>

     Alias /webdav /var/www/webdav 
     <Directory /var/www/webdav>
      DAV On
      AuthType Digest
      AuthName "webdav"
      AuthUserFile /etc/apache2/users.password
      Require valid-user
     </Directory>

    **# 여기까지 - 추가할 부분**

    < /VirtualHost>

     

    다 수정 했으면, sudo service apache2 restart 해줄 것.

    외장하드 심볼릭 링크 사용시 media 폴더 권한 바꿔줘야함
    ln -s /media/kseh1029/hdd1 /var/www/webdav
    sudo chown -R kseh1029.kseh1029 /media
    sudo chmod -R /media
    ```

- **File Transfer Protocol (FTP) 설치**

    ```bash
    sudo apt-get install vsftpd

    sudo gedit /etc/vsftpd.conf 에 들어가서
    anonymous_enable=NO (익명접근 막기)
    local_enable=YES (계정 사용자 접속 기능)
    write_enable=YES (업로드 기능)
    chroot_local_user=YES (사용자들의 상위 디렉토리 접근제한)

    특정 사용자만을 접근제한을 설정하고 싶은 경우, chroot_list에 등록되어있는 계정만 chroot가 적용
    chroot_list_enable=YES (계정 홈의 상위 디렉토리 접근권한을 가지는 리스트를 관리)
    chroot_list_file=/etc/vsftpd/chroot_list (상위 디렉토리 접근권한을 가지는 계정 목록)

    service vsftpd start (ftp서버 시작)
    service vsftpd stop (ftp서버 멈춤)
    service vsftpd restart (ftp서버 재시작)
    service vsftpd status (ftp서버 상태확인)
    ```

- **SSH 서버 개방**

    ```bash
    sudo apt-get install openssh-server
    ```

- **Transmission-daemon 설치**

    ```bash
    sudo apt-get install transmission-daemon

    sudo service transmission-daemon stop
    sudo gedit /etc/transmission-daemon/settings.json

    아래 내용을 수정

    “download-dir”: “[다운로드 할 곳의 default]“,
    “rpc-password”: “[transmission에 쓸 계정의 비밀번호. ]“,
    “rpc-username”: “[transmission에 쓸 계정명]“,

    "rpc-whitelist-enabled": false 해야지 다른 곳에서도 접속 가능

    sudo service transmission-daemon start

    설치 후 9091 포트로 접속
    ```

- **Transmission-daemon 에서 Torrent Seeding 자동 삭제하기**

    ```bash
    #!/bin/sh
    SERVER="포트번호 --auth 아이디:비번"
    TORRENTLIST=`transmission-remote $SERVER --list | sed -e '1d;$d;s/^ *//' | cut --only-delimited --delimiter=" " --fields=1`
    for TORRENTID in $TORRENTLIST
    do
        DL_COMPLETED=`transmission-remote $SERVER --torrent $TORRENTID --info | grep "Percent Done: 100%"`
        STATE_STOPPED=`transmission-remote $SERVER --torrent $TORRENTID --info | grep "State: Seeding\|Stopped\|Finished\|Idle"`
        if [ "$DL_COMPLETED" ] && [ "$STATE_STOPPED" ]; then
            transmission-remote $SERVER --torrent $TORRENTID --remove
        fi
    done

    위 내용으로 AutoDelete.sh 를 만들자.
    그리고 해당 sh 스크립트의 chmod 를 777 으로 준다.
    편의상 /Download/Scripts/AutoDelete.sh 으로 저장했다고 가정하면

    sudo /etc/init.d/transmission-daemon stop
    sudo gedit /etc/transmission-daemon/settings.json

    "script-torrent-done-enabled": true,
    "script-torrent-done-filename": "/Download/Scripts/AutoDelete.sh",

    sudo /etc/init.d/transmission-daemon start
    ```

- **MySQL 설치하기**

    ```bash
    apt install mysql-server
    apt-get install mysql-client
    ```

- **MySQL 비밀번호 변경하기**

    ```bash
    최초 mysql 설치시 인증방법이 패스워드가 아닌 auth_socket 이므로, 패스워드 방식으로 변경이 필요

    sudo mysql
    USE mysql;
    UPDATE user SET plugin='mysql_native_password' WHERE user='root';
    FLUSH PRIVILEGES;
    exit;
    sudo systemctl restart mysql.service

    그리고 나서 비밀번호를 설정
    sudo mysql
    USE mysql;
    UPDATE user SET authentication_string=password('원하는비밀번호') WHERE user='root';
    FLUSH PRIVILEGES;
    exit;

    앞으로 MySQL 으로 접근하기 위해서 아래 명령어를 사용하면 된다.
    mysql -u root -p
    ```

- **MySQL Server 에 localhost 가 아닌 외부에서 접속허가**

    ```bash
    우선 MySQL 로 접근을 한다.
    mysql -u root -p

    상황 1) 특정 DB, Table , 사용자, 호스트 설정 시
    grant all privileges on dbname.table to userid@host identified by '비밀번호';

    상황 2 ) 모든 DB 및 모든 사용자 허용
    mysql> grant all privileges on *.* to root@'%' identified by '비밀번호';

    flush privileges;

    외부 허용 설정을 위해
    sudo gedit /etc/mysql/mysql.conf.d/mysqld.cnf
    를 열어서

    [mysqld]
    bind-address            = 0.0.0.0

    로 바꾼다.

    3306 포트 allow 를 해준다.

    sudo ufw allow out 3306/tcp
    sudo ufw allow in 3306/tcp

    service mysql restart
    ```

- **MySQL 5.7 Error this is incompatible with sql_mode=only_full_group_by 오류 해결 방법**

    ```bash
    1. 문제 발생

    실 서버에서는 잘 동작하던 쿼리가 테스트 서버에서는 동작하지 않고 관련 오류를 뿜는것을 확인했다.

    Error this is incompatible with sql_mode=only_full_group_by

    문제는 Mysql 5.7버전 이상에서 Group By 쿼리를 사용하면 발생하는 오류로

    다음과 같이 설정하면 해결 할 수 있다.

     

    해결 방법

    mysql 설정파일인 my.cnf에서 다음과 같이 수정한다.

    [mysqld]

    sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES

    이후 MySql 서버 재시작 진행하면 반영완료
    ```

- **Apache2, MySQL log 안남도록 설정하기**

    ```bash
    /etc/crontab 에 추가

    Mysql 로그파일 자동삭제 설정 : 매월 1일 새벽4에 로그파일 정리작업
    0  4  1  *  * mysql -u root -p102200a -e "PURGE MASTER LOGS BEFORE DATE_SUB(CURRENT_DATE, INTERVAL 30 DAY)"

    /etc/apache2 에서 conf 파일 (site-available 등 여러 conf) 에서
    ErrorLog logs/error_log ---> ErrorLog /dev/null
    CustomLog logs/access_log combined  ---> CustomLog /dev/null combined
    ```

- **JAVA 자동설치 및 수동설치 / update-alternatives 를 이용한 여러 버전 사용**

    ```bash
    1. apt-get 을 이용한 Java 1.8 자동설치
    sudo apt-get install openjdk-8-jdk

    2. 소스파일을 이용한 Java 수동 설치
    https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html 혹은
    https://github.com/frekele/oracle-java/releases 사이트에서

    jdk-8u***-linux-x64.tar.gz 설치 후 압축풀고 나오는 디렉토리
    ex) jdk1.8.0_212 를 /usr/local 로 옮김

    이후, 환경변수 /etc/profile 을 sudo 로 gedit 혹은 vi 하여 최 하단에 다음을 입력
    JAVA_HOME=/usr/local/jdk1.8.0_212
    CLASSPATH=$JAVA_HOME/lib/tools.jar
    PATH=$PATH:$JAVA_HOME/bin
    export JAVA_HOME CLASSPATH PATH

    입력을 하였다면, /usr/profile 을 다시 실행하기 위해 source /etc/profile
    환경변수 설정 확인을 위해
    echo $JAVA_HOME
    echo $CLASSPATH
    echo $PATH

    하지만, 프로그래밍을 하게되면서 한 버전만을 사용할 수 없음.
    그떄마다 환경변수를 변경해주는 것은 매우 번거로우므로 update-alternatives 을 사용하자

    1) 사용가능한 자바목록에 Java Version 추가
    update-alternatives --install /usr/bin/java java /usr/local/jdk1.8.0_212/bin/java 100 

    2) Default Java 변경
    update-alternatives --config java 
    원하는 버전을 선택

    +) 자바목록에서 Java Version 삭제

    update-alternatives --remove java /usr/local/jdk1.8.0_212/bin/java
    ** 환경변수를 건들지 않고 alternatives 기능만 사용하더라도 추가가 가능
    ```

- **JAVA 한글 깨짐 문제 해결**

    ```bash
    /usr/local/jdk1.7.0_80/jre/lib/fontconfig.RedHat.5.properties.src

    안에꺼 수정하자.

    filename.-misc-baekmuk_batang-medium-r-normal--*-%d-*-*-c-*-iso10646-1=/usr/share/fonts/truetype/NanumGothic.TTF

    filename.-misc-baekmuk_gulim-medium-r-normal--*-%d-*-*-c-*-iso10646-1=/usr/share/fonts/truetype/NanumGothic.TTF

    폰트파일도 

    /usr/share/fonts/truetype/ 안에 넣어주고
    ```

- **JAVA classpath, jar 실행 방법**

    ```bash
    java -classpath "dist/blabla.jar" classname

    java -jar "dist/blabla.jar"
    ```

- **zip 파일 압축 해제시 한글 깨짐 문제 해결**

    ```bash
    아래 내용을 etc/profile 에 등록시킬 것.

    export UNZIP="-O cp949"
    export ZIPINFO="-O cp949"
    ```

- **SSL 적용하여 HTTPS 보안 접속 허용**

    ```bash
    sudo apt-get update

    sudo apt-get install letsencrypt

     

    인증서를 받기전 80포트를 비워주기 위해 apache 를 중지한다.

    sudo service apache2 stop

     

    sudo letsencrypt certonly --standalone -d test.com  #test.com 대신 나의 도메인

    이메일을 입력하고 Agree

     

    sudo service apache2 start 로 apache 를 시작한다.

     

    sudo a2enmod ssl  # SSL 사용 명시

    cd /etc/apache2/sites-available 

    cp default-ssl.conf test.com-ssl.conf # 복사

    sudo gedit test.com-ssl.conf

     

    파일 내 에서 수정할 내용들

    ServerAdmin ~~~@naver.com

    ServerName test.com

    ServerAlias test.com

    DocumentRoot 웹문서경로

    SSLEngine on

    SSLCertificateFile /etc/letsencrypt/live/test.com/fullchain pem

    SSLCertificateKeyFile /etc/letsencrypt/live/test.com/privkey pem

     

     

    sudo a2ensite test.com-ssl.conf

    sudo servcie apache2 restart

     

    인증서 기간은 90일 이므로 자동 갱신을 추가해야함.

     

    sudo crontab -e 을 하거나

    sudo gedit /var/spool/cron/crontab/root

     아래 내용을 추가 후 저장

    10 4 */15 * * /usr/bin/letsencrypt renew >> /var/log/letsencrypt/letsencrypt-renew.log

    15 4 */15 * * /usr/sbin/service apache2 restart
    ```