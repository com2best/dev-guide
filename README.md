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

아래의 문서와 동영상도 보고 익히는 것이 좋다. 처음에는 간단히 보고, 아래의 node, express, angular를 배우고 실습을 하기 전에 다시 한번 리뷰하는 것이 좋다.

* [WebStorm - Node.js Basics](https://www.youtube.com/watch?v=EMiU8zACVgA&list=PLQ176FUIyIUYnLuYVKM6JhVd6ukPgzdW7&index=13)
* [WebStorm - Node.js Debugging](https://www.youtube.com/watch?v=mVYJy3C63fw&list=PLQ176FUIyIUYnLuYVKM6JhVd6ukPgzdW7&index=14)
* [WebStorm - Getting Started with Express](https://www.youtube.com/watch?v=wvzZQ05BMmQ&list=PLQ176FUIyIUYnLuYVKM6JhVd6ukPgzdW7&index=15)
* [Debugging Express app](https://www.youtube.com/watch?v=jwANd_CoqRo&list=PLQ176FUIyIUYnLuYVKM6JhVd6ukPgzdW7&index=16)
* [Chrome extension for JetBrains IDEs](https://www.youtube.com/watch?v=kJh9lGbTSGI&list=PLQ176FUIyIUYnLuYVKM6JhVd6ukPgzdW7&index=1)

단축키도 외워 두어야 한다. 

* [WebStorm Keyboard Reference](https://resources.jetbrains.com/storage/products/webstorm/docs/WebStorm_ReferenceCard.pdf)
 
####2.3. Git
Git은 형상관리 도구로서 버젼을 관리해 주고, 다른 개발자들고 소스를 공유하는 툴이다. 기존의 CVS, SVN을 대체하여 많이 사용되는 추세이다. 아래 사이트에서 다운받아 설치 한다. 

* [Git](https://git-scm.com/)


#이 아래는 작성 중
---

* [Installing Node.js via package manager | Node.js](https://nodejs.org/en/download/package-manager/#osx)
> [StrongLoop | Comparison: Tools to Automate Restarting Node.js Server After Code Changes](https://strongloop.com/strongblog/comparison-tools-to-automate-restarting-node-js-server-after-code-changes-forever-nodemon-nodesupervisor-nodedev/)
> [Library of developer :: [Node.js] supervisor,nodemon - javascript 파일 변경시 node 자동으로 재시작하는 모듈](http://leechwin.tistory.com/entry/Nodejs-supervisor-javascript-%ED%8C%8C%EC%9D%BC-%EB%B3%80%EA%B2%BD%EC%8B%9C-node-%EC%9E%90%EB%8F%99%EC%9C%BC%EB%A1%9C-%EC%9E%AC%EC%8B%9C%EC%9E%91%ED%95%98%EB%8A%94-%EB%AA%A8%EB%93%88)

* [MongoDB for GIANT Ideas | MongoDB](https://www.mongodb.org/)
* [Install MongoDB Community Edition on OS X — MongoDB Manual 3.2](https://docs.mongodb.org/manual/tutorial/install-mongodb-on-os-x/)
* [Homebrew — The missing package manager for OS X](http://brew.sh/)
> \$ sudo mkdir -p /data/db
> \$ sudo chown com2best /data/db


###3. MEAN Stack

####3.0. Quick Start

* [Learn to Build Modern Web Apps with MEAN - Thinkster](https://thinkster.io/mean-stack-tutorial)

####3.1. node.js
* [Mobile Convergence :: [Node.js] Node.js, Express, MongoDB를 이용하여 REST API 만들기](http://mobicon.tistory.com/197)
* [Mobile Convergence :: [Node.js] 배우는 방법](http://mobicon.tistory.com/224)

####3.2. express.js

####3.3. angular.js
* [AngularJS](https://angularjs.org/)
 
* [AngularJS: Tutorial: Tutorial](https://docs.angularjs.org/tutorial)
* [AngularJS: Developer Guide: Controllers](https://docs.angularjs.org/guide/controller)
* [AngularJS: API: API Reference](https://docs.angularjs.org/api)
* [Mobile Convergence :: [Yeoman] AngularJS 개발환경 자동 구성하기](http://mobicon.tistory.com/274#recentTrackback)
> npm install -g yo
> npm install -g bower
> npm install -g grunt-cli

* [Mobile Convergence :: 'AngularJS/Concept' 카테고리의 글 목록](http://mobicon.tistory.com/category/AngularJS/Concept)

* [Mobile Convergence :: 'AngularJS/Start MEAN Stack' 카테고리의 글 목록](http://mobicon.tistory.com/category/AngularJS/Start%20MEAN%20Stack)

* [Yeoman-Angular-Express](https://github.com/hmalphettes/yeoman-angular-express-example)

####3. Bootstrap
* [Bootstrap · The world's most popular mobile-first and responsive front-end framework.](http://getbootstrap.com/)
* [Bootply - The Bootstrap Playground](http://www.bootply.com/new?visual=1)
* [Home of free code snippets for Bootstrap | Bootsnipp.com](http://bootsnipp.com/)
* [Font Awesome, the iconic font and CSS toolkit](http://fortawesome.github.io/Font-Awesome/)

###4. MEAN.js

* [MEAN.JS ](http://meanjs.org/docs/0.3.x/#getting-started)
 


