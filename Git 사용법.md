## git

Git Bash : 리눅스 기반 터미널. 리눅스, 맥과 같은 운영체제의 터미널과 같은 명령어로 git을 관리할 수 있게 해준다.
.git : 최초 'git init' 실행 시 자동 생성되는 파일로 git으로 생성한 버전들의 정보와 원격저장소의 주소 등을 저장한다. 로컬저장소라고 부른다.
commit : git을 통해 생성된 각각의 버전.


### Setting
*git init : 현재 폴더를 git 로컬저장소로 만든다 or 초기화시킨다.
*commit을 생성하기 전에, 먼저 사용자 정보를 등록해야 한다. (각 버전의 생성자를 명시)
  *git config --global user.email "example@email.com"
  *git config --global user.name "exampleName"
  *config는 환결설정이라는 의미. 일반적으로 프로그램의 설정을 저장하는 파일을 의미한다.

### Using Local
*git add [fileName] : commit에 추가할 파일을 선택한다
*git commit -m [message] : 해당 버전에 대한 설명이나 추가/수정된 내용 등. commit에 상세설명을 작성한다.
*git log : 현재까지 만들었던 commit을 확인한다. 최신 commit부터 보여준다.

*git checkout [commitId] : 해당 commit으로 버전을 되돌린다. commitId는 전체를 써도되지만, 일반적으로 앞에서부터 7글자를 사용한다.
*git checkout - : 최신 commit으로 버전을 되돌린다. '-' 가 최신 commit을 의미한다.

### Using with Github
로컬저장소에 commit을 원격저장소(repository)에 올리기 위해선, 먼저 로컬저장소에 원격저장소 주소를 알려야한다.
*git remote add origin "https://github.com/username/example" : Github 등에서 생성한 원격저장소의 주소를 로컬저장소에 알린다.
*git push origin master : 로컬저장소의 commit을 repository에 올린다.
