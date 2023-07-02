# Raspberry Pi
* 라즈베리 파이4에 톰캣, mariaDB를 설치하여 원격 웹 서버로 구성합니다.
* 원격에서 접속하기 위해 ssh를 사용할 예정이며, 운영체제는 Ubuntu Server로 GUI를 이용하지 않습니다.

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

* ubuntu를 설치하고 최초 라즈베리 파이를 부팅 시, default 유저의 정보는 다음과 같습니다.
```
login: ubuntu 
password: ubuntu
```
* 최초 로그인 시, 비밀번호를 변경합니다.

* 우선 기존 패키지들을 update/upgrade 시켜줍니다.
```
sudo apt-get update : 설치 되어있는 패키지들의 새로운 버젼이 있는지 확인합니다.
sudo apt-get upgrade : update에서 확인한 패키지들을 최신버전으로 업그레이드 합니다.
```

* apt (advanced packaging tool) : 데비안 계열에서 사용하는 패키징 툴. APT는 이진 파일로부터나 소스 코드 컴파일을 통하여 sw 패키지의 확인·구성·설치를 자동화한다. 


