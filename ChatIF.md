다음의 기능들을 테스트해서 작성해 보기 바랍니다. bot-web을 실행한 터미널에서 로그를 확인한다. 스크립트 파일은 첨부한 instructions.top으로 테스트 한다.    

    cd bot-web //bot-web폴더로 이동
    git pull
    npm install
    node server.js

아래와 같은 경고 메세지가 나오면 
node-java mac "no java runtime present"

이글을 참고하여 아래와 같이 처리한다. 
https://github.com/joeferner/node-java/issues/90#issuecomment-45613235

    sudo vi /Library/Java/JavaVirtualMachines/jdk1.8.0_72.jdk/Contents/Info.plist
    <key>JVMCapabilities</key>
     <array>
       <string>CommandLine</string>
       <string>JNI\</string>
     </array>
                
기존 머니봇 계좌조회는 빼놓았으므로, 서버에 올리지는 말고 로컬 PC에서만 개발 해봐해야 합니다.

####1. 자연어처리
자연어를 입력하면 아래와 같이 로그를 통해 자연어 구문 분석이 된것을 확인할 수 있다. 기존과 다르게 기본 keyword만 입력하여 스크립트를 작성할 수 있다.

    사용자 입력>> 내가 좋아하는 상품 좀 추천해봐
    자연어 처리>> 내 가 좋아하다 상품 좀 추천 해보다

####2. 포맷 인식
위와 같이 테스트 해 본다.

    입력> 2016-08-01 100-111-1111 계좌조회 해줘
    출력> 조회결과 100-111-1111의 2016-08-01 일자 내역을 조회 합니다.

위 대화는 아래와 같이 스크립트를 작성하면 된다. +account+등이 포맷 인식이 된 부분이다. 출력할 문장에도 인식된 값을 사용할 수 있다. 

    u: (<<조회 +date+ +account+>>) 조회결과 +account+의 +date+ 일자 내역을 조회 합니다. 
    
현재는 아래와 같은 포맷이 추가되어 있으며, 향후 더 추가 예정이다.

* +amount+: 금액, 십백천만억원 으로 끝나는 말
* +mobile+: 휴대폰 번호
* +phone+: 전화번호
* +date+: 날짜
* +time+: 시간
* +account+: 계좌번호, 숫자와 -로 이루어진 문자열

####3. MongoDB 연동
기존에 상품추천이나 FAQ와 같이 MongoDB를 연동하는 부분은 별도로 연동 개발 필요없이, 스크립트에서 바로 호출할 수 있다.  
아래와 같이 테스트해 본다.

    입력> 신용 상품
    출력> 신용대출 상품을 추천해드리겠습니다
        1. 우리은행 위비모바일대출 (5.75%)
        2. 신한은행 원클릭 급여이체 스마트론 (5.34%)
        3. 신한은행 샐러리론 (4.34%)
        상세한 정보를 보고싶은 상품을 선택해주세요. 
        
위 동작의 스크립트는 아래와 같다.

    u: (<<상품 신용>>) {\
        "module": "mongo",\
        "action": "find",\
        "save": true,
        "mongo": {"model": "Product", "query": {"category": "credit"}, "sort": "-rate", "limit": 3},\
        "content": "신용대출 상품을 추천해드리겠습니다%0a##+index+. +title+ (+rate+\%)%0a#상세한 정보를 보고싶은 상품을 선택해주세요."}

chat script에서 한줄이상 길어질 때는 끝에 \를 붙이면 된다. 

위에서 db에서 조회해 오는 부분을 먼저 보면, 기존의 DB조회를 mongo 안에 있는 부분으로 내부에서 처리해 준다 . 아래 부분이 db에서 목록을 조회하는 부분이다. 이 부분이 잘 이해가 안되면 개발자(권태현)에서 물어본다.

        "mongo": {"model": "Product", "query": {"category": "credit"}, "sort": "-rate", "limit": 3}

위 처리결과 아래와 같은 내용이 조회되는 것을 로그를 통해 확인할 수 있다.

    mongo:find>> [{"_id":"578c816d71045c125931557c","bankCode":"88","category":"credit","rate":5.75,"title":"우리은행 위비모바일대출","content":"ㅇㅇㅇ",},{"_id":"5787a264c4444c3d20e62609","bankCode":"88","category":"credit","rate":5.34,"title":"신한은행 원클릭 급여이체 스마트론","content":"ㅇㅇㅇ"},{"_id":"5787a229c4444c3d20e62608","bankCode":"88","category":"credit","rate":4.34,"title":"신한은행 샐러리론","content":"신ㅇㅇㅇ"}]

이렇게 조회해 온 데이터를 표시하는 부분은 content 에서 확인해야 하는데 그 중에 아래 부분을 주목해서 본다.

    ##+index+. +title+ (+rate+\%)%0a#
    
\#으로    둘러싸인 부분이 array형태로 자료가 반복되는 것을 의미하고, 안쪽의 +title+등은 위에서 읽어온 값의 title 등으로 바뀌어 진다. 

일반적인 경우 목록을 읽어온 후 선택하면 상세안내를 보여주므로, 그다음은 다음과 같이 입력한다. 


    입력> 신용 상품
    출력> 신용대출 상품을 추천해드리겠습니다
        1. 우리은행 위비모바일대출 (5.75%)
        2. 신한은행 원클릭 급여이체 스마트론 (5.34%)
        3. 신한은행 샐러리론 (4.34%)
        상세한 정보를 보고싶은 상품을 선택해주세요. 
        
    입력> 2
    출력> 신한은행 원클릭 급여이체 스마트론
            인터넷뱅킹에 가입한 12개월 이상 급여이체 거래 고객이 영업점에 직접
            방문하지 않고 인터넷 상에서 대출약정이 가능한 상ㅍ

위 구현을 위한 스크립트 부분은 아래와 같다.

    a:(_~num *) {
        "module": "mongo",\
        "action": "selectByIndex", \
        "index": %22 '_0 %22, \
        "content": "+title+%0a+content+"\
    }

기존 DB에서 읽어온 데이터를 그대로 사용하며, 위에서 save: true가 이 다음에 선택하기 위한 flag이다.

####4. JSON 연동
외부에 있는 데이터를 읽어와서 챗봇으로 바로 연동이 가능하다. 

    입력> 주차장
    출력> 주차장 위치를 입력해주세요.
    입력> 여의도
    출력> 공통 JSON 
        1. 국민일보사옥앞(구) 요금: 10분당 1000원
        2. 세우빌딩 앞노상주차장(구) 요금: 10분당 1000원
        3. 국민일보사옥앞 노상주차장(구) 요금: 10분당 1000원
        4. 렉싱턴호텔뒤 노상주차장(구) 요금: 10분당 1000원
        5. 동아빌딩앞 노상주차장(구) 요금: 10분당 1000원

위 데이터는 서울시 공공데이터에서 조회해 온것으로 아래 url을 브라우저에서 조회해 보면 데이터가 나온다. 

http://openapi.seoul.go.kr:8088/5462796a6a636f6d35334e6d736163/json/GetParkInfo/1/5/%EC%97%AC%EC%9D%98%EB%8F%84

데이터의 정보는 아래에 있다. 
 
* [서울시 주차장 정보 - 서울 열린 데이터 광장](http://data.seoul.go.kr/openinf/openapiview.jsp?infId=OA-13122)

아래와 같이 스크립트로 구성한다. 

    u: (주차장) 주차장 위치를 입력해주세요.
        a: (_*) { \
            "module": "http", \
            "action": "json", \
            "content": "공통 JSON %0a#GetParkInfo.row#+index+. +PARKING%5fNAME+ 요금: +ADD%5fTIME%5fRATE+분당 +ADD%5fRATES+원%0a#", \
            "url": "http://openapi.seoul.go.kr:8088",\
            "path": %22 /5462796a6a636f6d35334e6d736163/json/GetParkInfo/1/5/ '_0 %22,\
            "save": "true"\
        }


위 데이터를 읽어 오기 위한 부분이 module: http, action: json 이다. 

content의 값이 위에서 읽어온 값으로 변환된다. 이때 +PARKING%5fNAME+ 와 같이 바뀔 값을 넣어줄 수 있고, 주의할 점은 스크립트에 바로 쓸 떄는 '_'  기호는 %5f로 바꾸어야 한다.

아래는 주요한 공개DB 데이터로 무료로 다양한 데이터를 사용할 수 있고, 이외에도 JSON이나 XML로 웹으로 읽어올 수 있는 데이터를 연동해서 챗봇을 구성할 수 있다.

* [공공데이터 | 서울시 정보소통광장(정보공개)](http://opengov.seoul.go.kr/data?searchKeyword=OPEN+API)
* [DATA STORE – 데이터 오픈마켓](https://www.datastore.or.kr/product/categorylist.do)

####5. 웹페이지 연동
웹페이지에서 읽을 수 있는 데이터를 사용해서 바로 챗봇으로 연동이 가능하다.

    입력> 영화 공통
    출력> 공통 XPATH
        1. 수어사이드 스쿼드 감독: 데이비드 에이어 출연진: 윌 스미스...
        2. 마이펫의 이중생활 감독: 크리스 리노드 출연진: 루이스 C.K....
        3. 인천상륙작전 감독: 이재한 출연진: 이정재,이범수,리암 니슨,진세연 
        4. 덕혜옹주 감독: 허진호 출연진: 손예진,박해일 
        5. 부산행 감독: 연상호 출연진: 공유,정유미,마동석,김수안...
        상세한 정보를 보고싶은 영화를 선택해주세요.

위 데이터는 네이버 영화에서 조회해온 것으로, 아래 url을 브라우저에서 조회해 보면 데이터가 나온다.

http://movie.naver.com/movie/running/current.nhn?view=list&tab=normal&order=reserve

스크립트는 다음과 같이 구성한다. 

    u: (<<영화 공통>>) { \
        "module": "http", \
        "action": "xpathRepeat", \
        "content": "공통 XPATH%0a##+index+. +title+ 감독: +director+ 출연진: +actors+ %0a#상세한 정보를 보고싶은 영화를 선택해주세요.", \
        "url": "http://movie.naver.com",\
        "path": "/movie/running/current.nhn?view=list&tab=normal&order=reserve", \
        "save": "true",\
        "xpath": {\
            "repeat": "//ul[@class='lst%5fdetail%5ft1']/li", "limit": "5", \
            "doc": {"title": "//dt[@class='tit']/a/text()", "path": "//dt[@class='tit']/a/@href", "director": "//dl[@class='info%5ftxt1']/dd[2]//a/text()", "actors": "//dl[@class='info%5ftxt1']/dd[3]//a/text()"}}}

module: http, action: xpathRepeat인 것을 확인한다. url과 path는 위 브라우저에 입력한 주소와 동일하다.

위 스크립트에서 xpath 아래 부분을 주의깊게 살펴본다. repeat은 반복되는 부분, doc 은 하나의 자료에서 읽어오는 방법을 의미한다. 자세한 내용은 아래 tutorial 중  xpath 관련한 내용을 참고한다. ㅏㅏㅏ

* [W3Schools Online Web Tutorials(영문)](http://www.w3schools.com/) 

특히 웹페이지에서 자료를 추출할 위치를 찾기 위해서는 chrome브라우저를 잘 활용해야 한다. 위의 영화 페이지를 열고 개발자도구(Command+Option+I)를 연다. 그 다음 Select Element(Command+Shift+C)를 해서 원하는 값을 브라우저 상에서 선택하면 개발도구에 해당하는 html 위치가 표시된다. 이때 html의 위치값을 가지고 xpath 값을 정해야 한다. 

그 다음으로 다음과 같이 테스트 한다. 

    입력> 영화 공통
    출력> 공통 XPATH
        1. 수어사이드 스쿼드 감독: 데이비드 에이어 출연진: 윌 스미스...
        2. 마이펫의 이중생활 감독: 크리스 리노드 출연진: 루이스 C.K....
        3. 인천상륙작전 감독: 이재한 출연진: 이정재,이범수,리암 니슨,진세연 
        4. 덕혜옹주 감독: 허진호 출연진: 손예진,박해일 
        5. 부산행 감독: 연상호 출연진: 공유,정유미,마동석,김수안...
        상세한 정보를 보고싶은 영화를 선택해주세요.
    입력> 1
    출력> 영화상세안내
        제목: 수어사이드 스쿼드
        감독: 데이비드 에이어
        출연진: 윌 스미스,자레드 레토,마고 로비

아래 상세조회를 위해서는 기존 영화목록에서 해당하는 링크부분을 추출하여 상세 페이지로 이동하여 세부 내용을 표시한 것이다. 

이를 위한 스크립트는 다음과 같다. 

    a: (_~num *) {\
            "module": "http",\
            "action": "xpathByIndex", \
            "index": %22 '_0 %22, \
            "content": "영화상세안내%0a제목: +title+%0a감독: +director+%0a출연진: +actors+",\
            "url": "http://movie.naver.com",\
            "xpath": {\
                "doc": {"title": "//div[@class='mv%5finfo']/h3[@class='h%5fmovie']/a[1]/text()", "director": "//dl[@class='info%5fspec']/dd[2]/p/a/text()", "actors": "//dl[@class='info%5fspec']/dd[3]/p/a/text()", "poster_img": "//div[@class='poster']//img/@src"}}}

여기서 xpathByIndex는 이전에 조회한 목록에서 번호로 선택한 후 추가적으로 xpath를 조회하는 것으로 이전 화면에서 path에 해당하는 링크의 주소를 저장해 놓으면 xpathByIndex를 하면 해당 path로 조회를 하게 된다. 

####6. 공통함수, 템플릿함수
위의 구현을 위해서 다음과 같은 모듈과 함수가 정의되어 있다.

* mongo
    * findById
    * findOne
    * find
    * findByIndex
    * selectByIndex
* http
    * json
    * xpathRepeat
    * xpath
    * xpathByIndex
    * selectByIndex
    * plaintext

위의 함수를 공통함수라고 부르며, 기본 기능으로 탑재하여 활용할 수 있도록 한다. 

위의 http.xpathRepeat를 보면 내용이 복잡한 것을 알 수 있다. 이것을 매번 다 기술하는 대신에 계속 사용하는 부분은 미리 정의해 두고 사용할 수 있다. 공통함수의 물리적위치는 /modules/bot/action/common 에 있다. 

아래 스크립트 부분을 확인해 보면 이전 보다 간소화 되어 있고, module은 template action은 movies로 되어 있다. 

    u: (<<영화 템플릿>>) { "module": "template", "action": "movies", "content": "템플릿 XPATH%0a##+index+. +title+ 감독: +director+ 출연진: +actors+ %0a#상세한 정보를 보고싶은 영화를 선택해주세요."}

이 부분은 아래 위치에 정의되어 있다. 

    bot-web/custom-modules/moneybot/template.js 

    var templates = {
      "movies": {
        "module": "http",
        "action": "xpathRepeat",
        "url": "http://movie.naver.com",
        "path": "/movie/running/current.nhn?view=list&tab=normal&order=reserve",
        "save": "true",
        "xpath": {
          "repeat": "//ul[@class='lst_detail_t1']/li",
          "limit": "5",
          "doc": {
            "title": "//dt[@class='tit']/a/text()",
            "path": "//dt[@class='tit']/a/@href",
            "director": "//dl[@class='info_txt1']/dd[2]//a/text()",
            "actors": "//dl[@class='info_txt1']/dd[3]//a/text()"
          }
        }
      },

위와 같이 정의하는 것을 템플릿 함수라고 한다. 


