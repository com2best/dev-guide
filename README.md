##MEAN Stack  개발가이드

###1. 사전지식

웹에 관한 사전지식이 부족하면, 아래 내용을 먼저 학습한다. 이전에 JSP, ASP등 웹개발을 해본 경험이 있으면 이부분은 생략해도 된다 .

####1.2. 기본적인 웹기술

아래 사이트에서 Lean HTML, CSS, Bootstrap, How To, JavaScript, jQuery, JSON 부분만 읽어 본다.

* [W3Schools Online Web Tutorials(영문)](http://www.w3schools.com/)


####1.1. 자바스크립트

자바스크립트와 관련한 부분에서 좀 더 보고 싶으면 아래 내용을 읽어 본다.

* [JavaScript Garden(한글)](http://bonsaiden.github.io/JavaScript-Garden/ko/)
* [JavaScript Guide - JavaScript | MDN(영문)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide) 

###2.개발환경

기본적인 개발환경은 Mac을 기준으로 설명한다. Windows 등 환경의 경우에는 각 설치 프로그램의 OS별 설치 안내에 따라 설치한다. 

####2.1. Sublime Text

가볍고 빠른 에디터로 다양한 플러그인을 지원해서 개발 시에도 많이 사용되는 무료 개발도구 이다. 본 문서의 작성 시점에는 Sublime Text 3 기준으로 설명한다. 아래의 사이트에서 다운받아서 설치 한다. 

홈페이지 첫화면에 맨위에 이미지와 같이 안내를 보고 기본기능을 습득하고, 간단한 단축키는 외우고 있는 것이 좋다.

* [Sublime Text](https://www.sublimetext.com/)

설치 후 아래 의 안내에 따라 설정을 하며, 필요한 경우 검색을 통해 본인에 맞는 방법으로 초기 세팅 한다.

* [Sublime Text 설치 후 초기 세팅하기(기본 플러그인 포함)](http://jos39.tistory.com/243)
* [Sublime Text 3 한글 관련 문제 해결 방법](http://blog.gaerae.com/2015/03/sublime-text-3-korean.html)

####2.2. WebStorm

Javascript 기반 개발에서 가장 많이 사용되는 개발 도구 이다. 유료($120/Year)이고 본격 사용하기 전에는 1달 무료 Trial 을 사용해 볼 수 있다. 

* [WebStorm: The Smartest JavaScript IDE](https://www.jetbrains.com/webstorm/)

아래의 동영상은 반드시 보고, 따라해 보는 것이 좋다. 

* [WebStorm - MEAN Stack Walkthrough and Tips - YouTube](https://www.youtube.com/watch?v=JnMvok0Yks8&list=PLQ176FUIyIUYnLuYVKM6JhVd6ukPgzdW7&index=12)

단축키도 외워 두어야 한다. 

* [WebStorm Keyboard Reference](https://resources.jetbrains.com/storage/products/webstorm/docs/WebStorm_ReferenceCard.pdf)
 
####2.3. Git

git은 형상관리 도구로서 버젼을 관리해 주고, 다른 개발자들고 소스를 공유하는 툴이다. 기존의 CVS, SVN을 대체하여 많이 사용되는 추세이다. 아래 사이트에서 다운받아 설치 한다. 

* [Git](https://git-scm.com/)

아래의 안내를 간단히 보고 익히되, git clone, checkout, pull, commit, push 명령어 위주로 본다. 

* [git - 간편 안내서 - 어렵지 않아요!(한글)](https://rogerdudler.github.io/git-guide/index.ko.html)

아래 정식안내서는 처음부터 전부 읽기 보다는 필요한 부분만 찾아서 보는 것이 좋다. 

* [Git - Book(한글)](https://git-scm.com/book/ko/v2)

github는  git 을 사용하여 소스를 관리해주는 원격저장소 서비스 이다. 많은 오픈 소스 프로젝트가 github에 공개되어 있다. 누구나 가입하여 무료로 사용할 수 있으며, 비공개 프로젝트로 소스를 공개하고 싶지 않을 때는 유료로 사용해야 한다. 아래 사이트에서 회원가입을 해 둔다. 

* [GitHub](https://github.com/)

다음 글을 통해 github 기본 사용법을 익힌다. 

* [완전 초보를 위한 깃허브](http://nolboo.kim/blog/2013/10/06/github-for-beginner/)

####2.4. MongoDB

MongoDB는 데이터를 저장하기 위한 데이터베이스이다.  여기서는 설치만 해놓고 자세한 사용법은 이후에 나오는 강좌에서 학습한다.

* [MongoDB for GIANT Ideas | MongoDB](https://www.mongodb.org/)

무료 버전인 Community Server를 아래주소에서 설치한다. 링크가 깨진 경우에는 위의 홈페이지에서 Download 페이지를 찾아서 설치 한다. 

* [MongoDB Download Center | MongoDB](https://www.mongodb.com/download-center?jmp=nav#community)
 
Mac환경에서의 설치는 아래 설명을 보고 설치한다.  중간과정에 [Homebrew](http://brew.sh/) 설치가 필요하고 안내에 따라 설치 한다. 

* [Install MongoDB Community Edition on OS X — MongoDB Manual 3.2](https://docs.mongodb.org/manual/tutorial/install-mongodb-on-os-x/)

설치후 실행할 때 오류가 나면, Mac 터미널(Terminal) 프로그램에서 아래와 같이 한다. 데이터를 저장하기 위한 폴더가 제대로 생성되지 않았기 때문이다. 아래 명령에서 com2best 는 본인의 아이디로 바꾸어서 입력한다. 

    $ sudo mkdir -p /data/db
    $ sudo chown com2best /data/db

####2.5. Node.js

Node.js는 자바스크립트로 서버를 만들수 있는 프레임워크 이다. 여기서는 설치만 해놓고 자세한 개발방법은 이후에 나오는 강좌에서 학습한다. 

* [Node.js](https://nodejs.org/)

위의 mongodb설치 시에 설치한 [Homebrew](http://brew.sh/)를 사용하여,  터미널(Terminal)에서 아래와 같이 입력하여 설치하는 것이 좋다. Permission Denied 오류가 나오면서 설치가 안되는 경우는 앞에 sudo를 붙여서 관리자 권한으로 설치 한다. 

    $ brew install node 

설치가 잘 안되거나 다른 방법으로 설치하고 싶은 경우 아래 링크의 안내에 따라 설치한다. 

* [Installing Node.js via package manager | Node.js](https://nodejs.org/en/download/package-manager/#osx)

NodeJS가 잘 설치 되었으면, 파일이 변경될 때 마다 node를 다시 시작해 주는 nodemon도 설치해 둔다. 

    npm install -g nodemon


###3. MEAN Stack

최초 학습 시에는 '필수'라고 표시되어 있는 내용 부터 우선 학습한다. 아래 내용을 2번 이상 읽어 보는 것이 좋으며, 어느 정도 개념이 잡힌 것 같으면 '필수'라고 표시되지 않은 것들도 학습한다.

####3.1. Quick Start

처음부터 MEAN 의 각 기술들에 각각 학습하는 것 보다는 전반적인 개발방법을 볼 수 있는 학습 내용 위주로 먼저 살펴보고, 각 세부기술을 학습하는 방법으로 진행한다.

아래 블로그는 개념을 잡기 쉬운 글들로 이루어져 있다. 블로그 목록이라 아래에 있는 글부터 역순으로 읽는다. 'Community 서비스'  제목 부터 있는 예제는 직접 코딩해서 따라해 본다.

* [AngularJS/Start MEAN Stack - Mobile Convergence (한글)](http://mobicon.tistory.com/category/AngularJS/Start%20MEAN%20Stack) - 필수

서버 REST API를 개발하는 예제로 직접 코딩해서 따라해 본다.

* [[Node.js] Node.js, Express, MongoDB를 이용하여 REST API 만들기 - Mobile Convergence (한글)](http://mobicon.tistory.com/197) - 필수

잘 정리되어 있는 기초 Tutorial, 직접 따라서 코딩해 본다.

* [Learn to Build Modern Web Apps with MEAN - Thinkster(영문)](https://thinkster.io/mean-stack-tutorial) - 필수
 
####3.2. [Angular.js](https://angularjs.org/) 

아래 블로그 목록이라 아래에 있는 글부터 역순으로 읽는다.

* [AngularJS/Concept - Mobile Convergence (한글)](http://mobicon.tistory.com/category/AngularJS/Concept) - 필수

아래 Tutorial 을 통하여 개념을 잡도록 한다. 

* [AngularJS: Tutorial: Tutorial(영문)](https://docs.angularjs.org/tutorial) - 필수

아래 Developer Guide를 통하여 심화학습을 한다. 

* [AngularJS: Developer Guide: Controllers(영문)](https://docs.angularjs.org/guide/controller)

개발 시에 API Reference를 찾아가면서 개발한다. 

* [AngularJS: API: API Reference(영문)](https://docs.angularjs.org/api)

AngularJS 개발환경과 관련하여 하래 문서를 참고한다.

* [AngularJS 개발환경 자동 구성하기](http://mobicon.tistory.com/274#recentTrackback) - 필수

####3.3. [Node.js](https://nodejs.org/en/)

초심자 가이드를 통하여 개념을 잡도록 한다. 

* [The Node Beginner Book (한글)](http://www.nodebeginner.org/index-kr.html) - 필수
* [Felix's node.js Beginners Guide(한글)](http://pismute.github.io/nodeguide.com/beginner.html) - 필수

아래 온라인책은 상세한 Node.js 학습서로 가볍게 읽어보고 추후에 필요한 부분을 찾아볼 수 있도록 한다.

* [Professional Node.js(영문)](http://htchttp.s3.amazonaws.com/books/professional_node.js.pdf)

Node.js는 전반적인 개념을 이해하는 것이 중요하며, 웹 개발인 경우 Node.js위에서 동작하는 Express.js API를 주로 사용하므로, 처음에 Node.js가 잘 이해 안가더라도 Express.js 부분을 상세히 보도록 한다. 

####3.4. [Express.js](http://expressjs.com/)

Express.js 홈페이지에서 상단의 메뉴에서 Getting Started로 기본 개념을 익힌다. 목차가 나온 페이지가 없어서 상단 메뉴에서 Getting Started 안에 있는 내용을 선택해서 하나씩 읽어야 한다. 

* [Getting Started](http://expressjs.com/en/starter/installing.html) - 필수
 
Guide를 통해 조금 더 상세한 내용을 익힌다. 목차가 나온 페이지가 없어서 상단 메뉴에서 Guide 안에 있는 내용을 선택해서 하나씩 읽어야 한다. 

* [Guide](http://expressjs.com/en/guide/routing.html)

개발 시에 API Reference를 찾아가면서 개발한다. 

* [Express 4.x - API Reference](http://expressjs.com/en/4x/api.html)

####3.5. [MongoDB](https://www.mongodb.com/)

Mongodb는 깊이 공부해야 될 내용이 많으므로, 처음에는 가볍게 보고 추후에 깊이 공부하도록 한다.

* [The MongoDB 3.2 Manual — MongoDB Manual 3.2](https://docs.mongodb.com/manual/)

Mongoose는 node.js 서 mongodb를 사용할 수 있는 library로 필수적으로 사용하므로 아래 문서를 읽고 숙지한다.

* [Mongoose Schemas v4.5.3](http://mongoosejs.com/docs/guide.html) - 필수
 
####3.5. [Bootstrap](http://getbootstrap.com/)

Bootstrap은 현시점에서 가장 인기있는 반응형 웹 및 모바일 디자인을 위한 UI Framework이다. 개발자도 쉽게 멋진 디자인을 할 수 있는 버튼, Input Form 등 다양한 UI Compnent들을 제공한다. 홈페이지에서 아래 내용을 읽고 숙지 한다.

* [Getting started · Bootstrap](http://getbootstrap.com/getting-started/) - 필수
* [CSS · Bootstrap](http://getbootstrap.com/css/) - 필수
* [Components · Bootstrap](http://getbootstrap.com/components/) - 필수
* [JavaScript · Bootstrap](http://getbootstrap.com/javascript/) - 필수

Bootstrap을 사용한 디자인을 웹에서 직접 코딩해보고 바로 Preview할 수 있는 기능을 제공한다. 또한 Template에 보면 기본 페이지등과 메뉴 디자인 템플릿을 제공한다.

* [Bootply - The Bootstrap Playground](http://www.bootply.com/new?visual=1)

Bootscrap 기반 무료 디자인 템플릿을 제공하는 사이트이다. 화면전체, 부분템플릿, 버튼 디자인 등을 제공하고, 다양한 Tool도 제공하고 있다. 필요한 디자인이 있을 경우 가져다 쓰도록 한다.

* [Home of free code snippets for Bootstrap | Bootsnipp.com](http://bootsnipp.com/)

다양한 무료 아이콘을 제공하는 toolkit이다. 아래 홈페이지에서 Getting Started, Icons, Examples 메뉴 아래의 내용을 읽고 숙지 한다.

* [Font Awesome, the iconic font and CSS toolkit](http://fortawesome.github.io/Font-Awesome/)

###4. MEAN.js

MEAN.js 는 MongoDB, ExpressJS, AngularJS, NodeJS를 사용한 MEAN Stack을 모두 사용한 Template 프로젝트로 로그인, 게시판 등 기본 기능이 구현되어 있다. 서비스 개발 시에는 기존 구현된 게시판(Articles)를 참고하면 빠르게 개발을 시작할 수 있다. 이 글을 쓰는 시점에는 0.3.0 Stable 버전을 사용한다.

* [MEAN.JS](http://meanjs.org/)

####4.1. 학습하기 
아래의 동영상을 보고 따라해 본다.  

* [MeanJS - MEAN stack Yeoman Generator Tutorial - YouTube](https://www.youtube.com/watch?v=XHpMH_5n2fQ) - 필수
 
 프로젝트의 내부 구조 등이 간단하게 설명되어 있다. 아래 문서를 읽고 숙지하며, 직접 프로젝트를 받아 실행해서 정상 동작하도록 확인하여야 한다.

* [MEAN.JS - Documentation 0.3.x Stable](http://meanjs.org/docs/0.3.x/) - 필수

####4.2. MEANJS 실행해 보기

먼저 터미널(Terminal)을 실행하고, bower와 grunt를 미리 설치한다. 설치는 한번만하고 프로젝트를 다시 생성할 때는 하지 않아도 된다.

    $ npm install -g bower
    $ npm install -g grunt-cli

프로젝트 시작 전에  MongoDB를 실행한다.

    $ mongod

새로운 터미널을 열고 (Command+N), 작업할 폴더로 이동한(cd 이동할 폴더명) 후 아래와 같이 프로젝트를 다운로드 받는다. 마지막의 meanjs는 본인이 원하는 이름으로 바꾸어도 된다.

    $ git clone https://github.com/meanjs/mean.git meanjs
    $ cd meanjs

다음명력을 사용하면 프로젝트에서 사용하는 node module을 자동으로 다운로드 설치한다. 개발을 진행하면서 라이브러리를 추가한 경우로 아래 명령어를 다시 실행한다.

    $ npm install

프로젝트 생성이 끝나면 Terminal에서 아래 와 같이 MongoDB를 실행한 후,

    $ mongod

아래와 같이 입력해서 mean.js프로젝트를 실행한다.

    $ grunt

또는 node 명령어로 직접 실행할 수 도 있다.

    $ node server.js

크롬브라우저에서 다음의 주소를 입력해서 다음과 같은 화면이 브라우져에 나오면 정상적으로 설정 된 것이다.

    http://localhost:3000

![MEAN.js](https://github.com/com2best/dev-guide/blob/master/images/mean.js.png?raw=true)

위 예제는 기능이 동작하는 예제로서 Sign Up(회원가입), Sign In(로그인), Articles(게시판)이 동작한다.

종료시에는 터미널에서 [Ctrl+C]로 종료한다.

####4.3. MeanJS로 개발해 보기

새로운 기능을 추가할 때는 기본이 되는 Articles나 비슷한 기능의 부분을 복사해서 시작하는 것이 편리하다.

여기서는 articles를 복사하여 tests 라는 기능을 만들어 보기로 한다. 

왼쪽 WebStorm Project View에서 modules > articles 를 복사[Command+C] 하여, modules에 포커스를 두고 붙여넣기[Command+V]를 한다.이 때 복사할 폴터 이름(New Name) 에 tests라고 입력하고 복사한다. 이때 폴더명은 복수로 한다. 

![folder copy](https://github.com/com2best/dev-guide/blob/master/images/copy1.png?raw=true)

복사한 폴더의 내부는 article 기준으로 되어 있으므로 article를 test로 Article를 Test로  tests 폴더 안의 모든 파일 안을 Replace한다. Project View 에서 tests 폴더에 포커스를 두고 Replace in Path[Command+Shift+R]을 실행한다. 이때 Case sensitive를 체크하고, Scope는 Directory를 선택하고 지정할 디렉토리가 tests로 되어 있어야 한다. Whole project를 선택하면 전체 프로젝트가 바뀌게 되므로 반드시 Directory만 선택되어 있도록 매우 주의하여야 한다. 

![folder copy](https://github.com/com2best/dev-guide/blob/master/images/replace1.png?raw=true)

![folder copy](https://github.com/com2best/dev-guide/blob/master/images/replace2.png?raw=true)

아직 파일이름들은 article로 되어 있으므로, 파일명을 모두 바꾸어 주어야 하는데 터미널에서 moduels > tests 내부로 이동한 후, 아래와 같이 입력 실행한다. 아래 내용은 article이 들어 있는 파일명을 test로 바꾸어 준다. 이 명령은 현재 폴더 아래이 파일명에 article이 들어있는 것을 모두 변경하므로, 반드시 tests 폴더 아래로 이동하여 실행해야 한다.

    $ find . -name '*article*' | while read f; do mv "$f" "${f//article/test}"; done;

여기까지 되었으면 브라우져에서 다음 주소로 접속 하여 오류 없이 실행되는지 확인해 본다.

    http://localhost:300/tests

이제 tests에서 구현하고 싶은 기능을 변경하여 구현하면 된다. 일단 Model을 바꾸고, 그 Model에 맞게 Controller를 바꾸어 주는 것이 기본 절차 이다. 

###5. 개발 디버깅 환경

####5.1. Chrome 개발도구

웹개발에 있어서 Chrome 개발도구는 필수로 사용되는 도구 이므로 잘 숙지하고 있어야한다. 파이어폭스 개발도구도 많이 사용되었었으나, 현재 시점에서는 Chrome 개발도구가 더욱 많이 사용되고 있다. Jaavascript 개발 뿐만 아니라 웹디자인 퍼블리셔 에게도 Chrome 개발도구는 필수이다.

아래 동영상을 보고 숙지하도록 한다.

* [둘러보기 - 생활코딩](https://opentutorials.org/course/580/2865)
* [Elements - 생활코딩](https://opentutorials.org/course/580/2866)
* [Sources - 생활코딩](https://opentutorials.org/course/580/2869)
* [Console - 생활코딩](https://opentutorials.org/course/580/2872)
* [리모트 디버깅 - 생활코딩](https://opentutorials.org/course/580/4152)

AngularJS Extension도 설치해 두고 사용법을 알아두면 좋다.

* [AngularJS Batarang - Chrome Web Store](https://chrome.google.com/webstore/detail/angularjs-batarang/ighdmehidhipcmcojjgiloacoafjmpfk)

####5.2. WebStorm 개발환경

WebStorm 을 사용하는 경우는 아래와 같이 개발환경을 셋팅하고 디버깅이 되는 지 확인한다.

개발에는 크롬을 기본 브라우져로 사용하므로, chrome extension을 설치한다.

* [JetBrains IDE Support - Chrome 웹 스토어](https://chrome.google.com/webstore/detail/jetbrains-ide-support/hmhgeddbohgjknpmjagkdomcpobmllji?hl=ko) 

아래의 동영상을 보고 개념을 잡는다. 

* [WebStorm - Node.js Basics](https://www.youtube.com/watch?v=EMiU8zACVgA&list=PLQ176FUIyIUYnLuYVKM6JhVd6ukPgzdW7&index=13)
* [WebStorm - Node.js Debugging](https://www.youtube.com/watch?v=mVYJy3C63fw&list=PLQ176FUIyIUYnLuYVKM6JhVd6ukPgzdW7&index=14)
* [WebStorm - Getting Started with Express](https://www.youtube.com/watch?v=wvzZQ05BMmQ&list=PLQ176FUIyIUYnLuYVKM6JhVd6ukPgzdW7&index=15)
* [Debugging Express app](https://www.youtube.com/watch?v=jwANd_CoqRo&list=PLQ176FUIyIUYnLuYVKM6JhVd6ukPgzdW7&index=16)

WebStorm 메뉴에서 VCS >  Checkout from Version Control 을 실행하고 아래 그림과 같이 입력하여 프로젝트를 다운로드 한다. Parent Directory는 프로젝트 폴더를 모아놓은 상위 디렉토리명으로 하고, Directory Name은 원하는 프로젝트 이름으로 바꾼다.

![git clone](https://github.com/com2best/dev-guide/blob/master/images/meanjs-download.png?raw=true)

Run > Edit Configuration에서 +로 추가를 선택하여 다음과 같이 설정한다.

![Node.JS Debug](https://github.com/com2best/dev-guide/blob/master/images/edit-config-1.png.png?raw=true)

![Browser/LiveEdit](https://github.com/com2best/dev-guide/blob/master/images/edit-config-2.png?raw=true)

![Node.JS Remote Debug](https://github.com/com2best/dev-guide/blob/master/images/edit-config-3.png?raw=true)

![Javascript Debug](https://github.com/com2best/dev-guide/blob/master/images/edit-config-4.png?raw=true)

위와 같이 설정하고 Debug [Command+D]를 실행해서 브라우저가 뜨면서 예제 화면이 나오는 지 확인한다.

유의할 점은 WebStorm 디버깅 모드에서 Chrome 개발도구를 열면 디버깅 모드가 꺼지는 현상이 있다. 디버깅 실행할 때 열린 Chrome Tab말고 새로운 Tab을 추가하여 같은 주소로 접속한 후 개발도구를 열면 Chrome 디버깅을 사용할 수 있고, 디버깅이 되는 본래 Tab에서는 WebStorm 디버깅 기능을 사용할 수 있다.

다음은 Break Point 가 동작하는 지 확인해 본다. 아래와 같이 modules > articles > server > controllers > articles.server.controller.js 84번째 줄에 Break Point를 설정한다. Break Point 설정 방법은 해당 줄에서 [Command+F8]을 누르거나, 동그라미 위치를 더블클릭 하면 된다. 

![debug1](https://github.com/com2best/dev-guide/blob/master/images/debug1.png?raw=true)

아래와 같이 modules > articles > client > controllers > list-articles.client.controller.js 11번째 줄에 Break Point를 설정한다.

![debug2](https://github.com/com2best/dev-guide/blob/master/images/debug2.png?raw=true)

Break Point 위치는 MeanJS 버젼에 따라 달라질 수 있고 상황에 따라 서버와 클라이언트 Break Point 가 동작되는지 확인할 수 있는 위치로 하면 된다. 

실행한 예제 화면을 브라우져에서 접속하여 상단의 articles 메뉴를 클릭하여, WebStorm에서 위 두가지 Break Point 에서 중단되는 지 확인한다.
계속 실행하기는 [Command+Alt+R]이고, 한줄씩 실행하기는 [F8] 이다.

###5. Mobile App 개발

###6. 배포하기

####6.1. Heroku

####6.2. Linux 서버 직접 배포



