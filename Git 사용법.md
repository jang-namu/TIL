## git

git : 소스코드 버전관리 시스템, github : git으로 관리하는 프로젝트를 올려둘 수 있는 git 호스팅 사이트
Git Bash : 리눅스 기반 터미널. 리눅스, 맥과 같은 운영체제의 터미널과 같은 명령어로 git을 관리할 수 있게 해준다.
.git : 최초 'git init' 실행 시 자동 생성되는 파일로 git으로 생성한 버전들의 정보와 원격저장소의 주소 등을 저장한다. 로컬저장소라고 부른다.
커밋(commit) : git(버전관리)을 통해 생성된 각각의 버전.


## Setting
* git init : 현재 폴더를 git 로컬저장소(Local repository)로 만든다 or 초기화시킨다.
* commit을 생성하기 전에, 먼저 사용자 정보를 등록해야 한다. (각 버전의 생성자를 명시)
  * git config --global user.email "example@email.com"
  * git config --global user.name "exampleName"
  * config는 환결설정이라는 의미. 일반적으로 프로그램의 설정을 저장하는 파일을 의미한다.


## Using Local
* git add [fileName] : commit에 추가할 파일을 선택한다
* git commit -m [message] : 해당 버전에 대한 설명이나 추가/수정된 내용 등. commit에 상세설명을 작성한다.
* git log : 현재까지 만들었던 commit을 확인한다. 최신 commit부터 보여준다.

* git checkout [commitId] : 해당 commit으로 버전을 되돌린다. commitId는 전체를 써도되지만, 일반적으로 앞에서부터 7글자를 사용한다.
* git checkout - : 최신 commit으로 버전을 되돌린다. '-' 가 최신 commit을 의미한다.


## Using with Github
* Local to Remote repository
 * 로컬저장소에 commit을 원격저장소(Remote repository)에 올리기 위해선, 먼저 로컬저장소에 원격저장소 주소를 알려야한다.
 * git remote add origin "https://github.com/username/example" : Github 등에서 생성한 원격저장소의 주소를 로컬저장소에 알린다.
 * git push origin master : 로컬저장소의 commit을 원격저장소에 올린다.

* Remote to Local repository
 * 클론(Clone) : 원격저장소의 코드와 버전 전체를 내 컴퓨터로 내려받는 것. 최시 버전뿐 아니라 이전 버전들과 원격저장소의 주소 등이 내 컴퓨터에 로컬저장소에 저장된다.
 * 원격저장소의 정보를 클론해오고 싶을 경우, 먼저 내려받을 위치(로컬저장소)를 지정해줘야 한다. 
 * Download ZIP : Github에서 지원하는 다운로드 기능. 원격저장소 주소와 버전정보가 제외된다.
 * git clone [Github 주소] . : 해당 Github 페이지에 저장소를 내려받는다. 주소 이후 한칸 공백 후 '.'은 현재 위치에 내려받으라는 것. 미작성 시 해당 저장소 이름으로 폴더가 생성된다.
 * git pull origin master : 원격저장소의 커밋을 로컬저장소에 내려받는다.


## Using GUI sourcetree
* [Sourcetree-Atlassian] (https://www.sourcetreeapp.com/)
sourcetree는 git을 이용한 프로젝트 관리를 GUI환경, 다시말해 클리과 화면조작으로 가능하게 만듭니다.
sourcetree는 직관적이며, 아름답기 때문에 많은 개발자들이 사용합니다.
sourcetree를 이용하면 commit을 그래프로 확인하고, 생소한 명령어 대신 누구나 쉽게 프로젝트 관리가 가능합니다.

* Get Remote Repository
  * Github의 아이디로 로그인하면 원격저장소가 자동으로 sourcetree에 올라옵니다.
  * Clone을 선택하여 원격저장소에 있는 프로젝트를 로컬에 내려받을 수 있습니다.
* Get Local Repository
  * 로컬저장소는 자동으로 추가되지 않아 수동으로 추가해줘야 합니다.
  * Add 기능을 이용하여 직관적으로 경로를 추가할 수 있습니다.
  * Create 기능은 내 컴퓨터에 새로운 로컬저장소를 생성합니다. 이것은 Git Bash에서 'git init' 명령을 수행한 것과 같은 의미를 갖습니다.

* about Local repository
  * sourcetree는 커밋을 그래프로 보여줍니다. 이는 .git 폴더에 저장된 정보를 사용합니다.
  * .git 폴더는 최초 'git init' 초기화 시, 생성되며 git은 .git폴더에 버전 관리한 데이터와 이를 올릴 원격저장소의 주소 등을 저장합니다.