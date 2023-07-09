# Raspberry Pi
* 라즈베리 파이4에 톰캣, mariaDB를 설치하여 원격 웹 서버로 구성합니다.
* 원격에서 접속하기 위해 ssh를 사용할 예정이며, 운영체제는 Ubuntu Server로 GUI를 이용하지 않습니다.
* 준비물: 라즈베리 파이 4 Model B (8GB), HDMI, HDMI to micro HDMI 컨버터, c타입 충전기, microSD 카드, sd카드 리더기

* 주의: 고정 IP할당과 포트포워딩은 다루지 않습니다. 공유기 설정에서 하셔야하는데 제공업체마다 대쉬보드가 다르고 자세한 방법도 상이합니다.

## 부록. sd카드 파티션 초기화, 포맷
* 제가 사용하고 있는 라즈베리 파이4에는 microSD가 들어갑니다. 저는 삼성 EVO PLUS를 사용중입니다.
* 여기서는 윈도우 운영체제에서 sd카드의 파티션을 다시 통합하고, 포맷하는 방법을 알아봅니다.
++ 사진 추가 필요.
1. 작업은 cmd에서 진행합니다.
2. diskpart : diskpart는 MS에서 제공하는 CLI 기반 디스크 파티셔닝 유틸리티 입니다.
3. list disk : 연결된 디스크의 정보를 보여줍니다.
4. select disk ? : 대상 디스크를 결정합니다. 저의 경우 디스크 2가 라즈베리 파이를 위한 microSD 입니다.
5. clean : 선택된 디스크의 파티션 정보를 전부 초기화합니다.
6. create partition primary : 주 파티션을 생성합니다.
7. format fs=ntfs quick : 현재 선택되어 있는 '파티션'을 ntfs로 빠른 포맷한다. (디스크가 아니다. 우리는 primary밖에 존재하지 않는다.)

* ntfs (New Technology File System) : 윈도우에서 주로 사용하는 파일 시스템, 기존 FAT 32 안전성과 보안성 및 파일 크기도 혁신적으로 증가했지만 윈도우를 제외한 다른 운영체제와는 호환성이 문제입니다.


## 라즈베리 파이에 ubuntu 설치.
* [Raspberry Pi Imager](https://www.raspberrypi.com/software/)
* 초기 라즈베리 파이에는 운영체제가 깔려있지 않으므로 설치해줘야 합니다.
* 라즈베리 파이에 OS를 설치하기 위해 위의 링크에서 라즈베리 파이 이미저를 다운받습니다.
* 파이에 기본 운영체제인 라즈비안과 리눅스-우분투는 이미저를 통해 설치가 가능합니다.
* microSD에 운영체제 이미지를 내려받습니다. Ubuntu Server 24.04 LTS를 설치하겠습니다.

* 이제 microSD를 라즈베리 파이에 꽂고, 전원과 화면 등을 연결합니다. os는 자동으로 설치가 됐을 겁니다.
* ubuntu를 설치하고 최초 라즈베리 파이를 부팅 시, default 유저의 정보는 다음과 같습니다.
    ```
    login: ubuntu 
    password: ubuntu
    ```
* 최초 로그인 시, 비밀번호를 변경합니다.

## 웹서버 환경 구성, 톰캣 설치.
* 우선 기존 패키지들을 update/upgrade 시켜줍니다.
    ```
    sudo apt-get update : 설치 되어있는 패키지들의 새로운 버젼이 있는지 확인합니다.
    sudo apt-get upgrade : update에서 확인한 패키지들을 최신버전으로 업그레이드 합니다.
    ```
* apt (advanced packaging tool) : 데비안 계열에서 사용하는 패키징 툴. APT는 이진 파일로부터나 소스 코드 컴파일을 통하여 sw 패키지의 확인·구성·설치를 자동화한다. 

* 이제 JDK와 톰캣, 데이터베이스를 설치해서, 웹서버를 구축합니다.
* 우선 JDK를 설치합니다. 저는 Java 11을 사용합니다.
    ```
    sudo apt-get install openjdk-11-jdk
    ```

* Java가 설치되었다면, 어디서든 Java를 찾을 수 있도록 환경변수로 설정해줘야 합니다.
    ```
    which javac
    ```
* which는 명령어의 위치를 찾습니다. 해당 경로에 Java가 설치되어 있습니다. 엄밀히는 \bin 이전까지가 JAVA_HOME이 됩니다.
* 이제 이를 환경변수로 등록합니다. '.profile' 사용자 설정파일을 수정합니다.
    ```
    sudo vi ~/.profile
    ```
* 파일에 아래 내용을 추가합니다.
    ```
    export JAVA_HOME=/usr~  (which의 실행결과 \bin 이전까지의 경로를 작성합니다.)
    export PATH=$JAVA_HOME/bin:$PATH
    ```
* 변경된 사용자 설정 파일의 내용을 갱신합니다.
    ```
    source ~/.profile
    ```

* 톰캣을 설치하겠습니다.
    ```
    sudo apt-get install tomcat9
    ```
* 톰캣은 설치와 동시에 실행됩니다. 다음의 명령어로 톰캣의 상태를 확인하겠습니다.
    ```
    sudo systemctl status tomcat9
    ```
* 톰캣의 로그는 'var/log/tomcat9' 경로에서 확일할 수 있습니다.

* 통신에 사용될 포트를 개방해야합니다. 방화벽을 쉽게 관리할 수 있는 ufw를 사용합니다.
    ```
    sudo apt-get install ufw
    ```
* ufw는 기본적으로 비활성화 되어있습니다. ufw를 활성화시키고 톰캣의 기본 포트인 8080을 개방하겠습니다.
    ```
    sudo ufw enable
    sudo ufw allow 80
    ```

* 현재까지의 진행상황을 확인하겠습니다. ifconfig로 현재 IP를 알아낸 후, xxx.xxx.xxx.xxx:8080 (ip:8080)에 접속합니다.
* 톰캣의 고양이를 만난다면 성공입니다.

## 원격 접속을 위한 SSH
* 하루빨리 라즈베리 파이에 모니터와 키보드, 마우스 선을 뽑아버리기 위해 SSH를 설치하겠습니다.
* SSH(Secure SHell): 원격지 호스트에 접속하기 위해 사용하는 프로토콜로, 암호화 기능을 추가하여 기존에 사용하던 안전하지 않은 텔넷 방식을 완전히 대체했다.

* 라즈베리 파이를 SSH 서버로 만들기 위해 OpenSSH Server를 설치합니다.
    ```
    sudo apt-get install openssh-server -y
    ```
* -y 옵션: 설치 시 묻는 사항에 대해 더 이상 묻지않고 모두 yes로 처리한다.
* SSH 서버가 설치되고 나면 해당하는 포트를 개방해야 합니다. ssh는 기본적으로 22번 포트를 사용합니다.
    ```
    sudo ufw allow 22
    ```

* 이제 모든 준비는 끝났습니다. 이제 Client에서 접속을 시도합니다. 
* 최신 윈도우, 리눅스 운영체제에는 기본적으로 openssh-client가 지원됩니다. 다음과 같이 접속할 수 있습니다.
    ```
    ssh [user]@[라즈베리파이 IP] -p [외부포트]
    ```
* '-p [외부포트]'는 내부 22번 포트와 대응되는 외부포트가 다를 때 사용합니다.
* 물론 다른 네트워크에서 원격으로 접속하기 위해서는 공인 ip를 사용해야 합니다. ip 대신 도메인을 사용할 수 있습니다.

## 데이터베이스 MariaDB 설치.
* 우리는 MariaDB를 사용합니다. MariaDB는 MySQL 개발진이 회사를 나와 만든 오픈소스 기반의 DBMS입니다.
* 또한, MariaDB는 MySQL 보다 성능상으로 조금 더 우세하고 MySQL에서 지원하지 않는 몇몇 기능이 포함되어 있습니다.

* MariaDB를 설치합니다. MariaDB의 설치는 매우 간답합니다.
    ```
    sudo apt-get install mariadb-server -y
    ```
* MariaDB에 접속하겠습니다.
    ```
    sudo mariadb
    sudo mysql
    ```
* 위에 두 방법 모두 MariaDB에 접속하기 위한 합법적인 방법입니다.
* 이제 MariaDB를 사용할 수 있습니다.


## MySQL Workbench로 클라이언트에서 MariaDB 사용하기
* 앞서 MariaDB를 사용할 수 있게 되었지만, ssh로 접속하여 CLI상에서 DBMS를 사용하는 것은 너무 불편합니다.
* client 사이드에서 MySQL Workbench를 써서 GUI 상에서 조작 가능하도록 만들겠습니다.
* 원격접속을 위해 우선 포트를 개방해야할 것 입니다. 
* MariaDB에 접속하여 아래에 SQL문을 입력하면 MariaDB가 사용중인 포트정보를 알 수 있습니다.
    ```
    SHOW GLOBAL VARIABLES LIKE 'PORT';
    ```
* 기본 설정은 3306 포트를 사용할 것입니다. 이 포트를 ufw를 통해 허용합니다.
    ```
    sudo ufw allow 3306
    ```

* 이제 계정 비밀번호도 바꿔봅시다.
    ```
    set password for 'root'@'localhost' = password('1234');
    ```

* 사용자 계정에 권한을 부여해야 합니다.
    ```
    GRANT ALL PRIVILEGES ON *.* TO root@'%' IDENTIFIED BY 'password';
    flush privileges;
    ```
* 'flush privileges;' 는 사용자, 권한에 관련된 변경사항을 즉시 적용시키기 위함입니다.

* 이제 마지막입니다. MariaDB의 환경설정 파일인 '/etc/mysql/my.cnf'를 수정해줍니다.
    ```
    # Instead of skip-networking the default is now to listen only on
    # localhost which is more compatible and is not less secure.
    bind-address = 127.0.0.1
    ```
* ubuntu에서 MariaDB는 기본적으로 localhost만 접속이 허용된 상태입니다. 이를 변경합니다.
* bind-address를 주석처리하거나, 값을 0.0.0.0으로 수정하면 됩니다.

## 무선랜 연결하는 방법.
* 연결하려는 무선랜의 SSID와 패스워드를 알아야 합니다.
* SSID는 다음과 같은 방법으로 찾을 수 있습니다.
    ```
    sudo iwlist scan
    ```
* 이제 무선 네트워크 인터페이스 카드(NIC)의 이름을 알아야 합니다.
* 여기에서는 다음과 같은 방법을 사용합니다.
    ```
    ls /sys/class/net
    ```
* 많은 경우 wlan0은 무선 NIC를 나타냅니다.
* 네트워크 구성 파일의 내용을 수정합니다.
    ```
    sudo nano /etc/netplan/50-cloud-init.yaml
    ```
* 다음과 같이 수정합니다.
    ```
    network:
        ethernets:
            eth0:
                dhcp4: true
                optional: true
        version: 2
    wifis:
        wlan0:
            optional: true
            access-points:
                [SSID 이름]:
                    password: [비밀번호]
            dhcp4: true
    ```
* 이제 파일을 저장하고 나와, 변경사항을 적용시켜 줍니다.
    ```
    sudo netplan apply
    ```
* 여기까지 했으면 이제 라즈베리 파이를 reboot 하시길 바랍니다. 다시 켜지고 무선랜 연결을 확인해보세요.
