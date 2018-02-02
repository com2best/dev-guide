Table Of Contents

[TOC]

## 0. 사전자료
[chatIF.md](https://github.com/com2best/dev-guide/blob/master/ChatIF.md)
[dialog.md](https://github.com/com2best/dev-guide/blob/master/dialog.md)
[READMEv1.md](https://github.com/com2best/dev-guide/blob/master/READMEv1.md)

참조 코드의 위치는 bot-web 0.9 기준

## 1. Dialog

### Dialog 형식

```
dialog = {
    originalInput: [],
    originalOutput:
	input: {
	    text:
	    nlpText: 
	    nlp: []
	    intents: []
	    entities: []
	    types: {}
	    sentence:
	    emotion
	},
    task: 
    data:
	option:
    output:   
}
```

context.botUser 하위 정보 현재 있는 값들을 어디로 옮길지 고민

* inNLP: (dialog.input.nlp 로 이동)
* inRaw: (dialog.input.nlp 로 이동)
* intent: (dialog.input.intent로 이동)
* entities: (dialog.input.intent로 이동) 
* nlp: (dialog.input.nlp 로 이동)
* nlu
* topic: 현재 선택된 context (context.session으로 이동)
* sentenceInfo: aspectType, questionWord, sentenceType, tenseType, toneType 
* currentDialog
* _currentDialog
* lastDialog
* orgBot

context.dialog 하위 정보 어디로 옯길지 고민

* inNLP: (dialog.input.nlp 로 이동)
* inRaw: (dialog.input.nlp 로 이동)
* isFail:
* numOfPage:
* page
* typeDoc
* typeInits
* typeMatches
 
###  Input

Input은 아래와 같은 종류가 있다. 

* text: 키워드 매칭
* if: "변수명 === true" 같은 조건문과 true/false를 return하는 function도 될 수 있다. 
* intent: 인텐트
* entity: 엔터티
* types: 여러게의 type이 들어갈수 있다. 
  type에 매치가 되면 task 변수에 저장되어 action에서 사용하거나 출력의 치환에 사용할 수 있다. 
* function: true, false를 return 하는 함수가 올 수 있다. 
    
###  Output
output format으로 값을 변환 한다. 

* 반복 <repeat doc> </repeat>
* 치환 {변수}
<repeat doc>{index}. {name}\n</repeat>
 
index는 1번부터 시작되는 번호로 자동으로 만든다. 
dialog.product.name 과 같이 하위 json 검색 가능
(검토) 치환할 변수는 task, context.dialog, context.user 순으로 scope 별로 순차적으로 찾는다. 

현재는 processOuput function에 구현되어 있다. 

아래와 같은 형식을 표현해야 한다. 

* image
* buttons
* list

### Actions

Action의 종류

* call
* up: 강제로 상위 Dialog
* back: 기본적으로는 상위 Dialog인데, 현재는 이전 Dialog와 혼합해서 사용됨
  call로 이동한 경우는 이전 dialog인데, call이 없었으면 상위로
* repeat
* returnCall: 다른 Dialog로 이동한후 다시 원래 Dialog로 복귀할 때 
* return: returnDialog로 호출되었을 때 호출한 Dialog로 복귀
* callChild: 하위에 있는 Dialog를 바로 검색
* returnCallChild: 하위 Dialog를 검색하여 returnCall
  {returnCallChild: '메뉴입력', options: {commonCallChild: 1, returnDialog: '주문주소확인'}}  }
    
Action에서는 다음과 같은 option을 사용할 수 있다. 

* output: 이동하면서 표시될 내용 전체를 바꾸는 경우. 형식 String
* prefix: 이동하면서 본래 출력될 내용의 위에 내용을 추가하는 경우. 형식 String
* postfix: 이동하면서 본래 출력될 내용의 아래에 내용을 추가하는 경우. 형식 String
* returnDialog: returnCall과 사용. return시 이동할 dialog를 변경할 경우. 형식 Dialog이름
* page: 이전 다음 페이지로 이동. 
  {repeat: 1, options: {page: "next"}}
  {repeat: 1, options: {page: "pre"}}
* commonCallChild: common Dialog에서 callChild할때 인것 같음
* top: 확인 중
  {call: '배달주문', options: {top: '배달주문'}}
 
### Types
#### Type 정의
아래와 같이 Type의 정의는 JSON으로 하고, typeCheck 함수를 통해 match 여부를 결정한다. 

```
var sampleType = {
	name: 'sampleType',
	typeCheck: function (text, type, dialog, context, callback) {
	  var matched = true; 매치여부
	  callback(text, dialog, matched);
	}
}
```

typeCheck 함수의 파라미터는 다음과 같다.

* text: 사용자에서 입력받은 내용이 넘어온다. NLP 처리가 된 문장이 넘어온다.
* type: type 개체 자체
* dialog: dialog개체로 match된 값은 task.param 저장하여 task 처리에 사용한다.
* context: context 개체
* callback: match처리 후 비동기 방식으로 callback을 호출한다. 


callback 함수 호출 시 파라미터는 다음과 같다.

* text: 입력 paramter를 다시 넘겨 준다. 
* dialog: 처리할 dialog
* matched: 매치 여부

type의 처리는 [dialog.js::executeType](https://github.com/com2best/bot-web/blob/0.9/modules/bot/action/common/dialog.js) function에 구현되어 있다. 

#### type option

* (삭제)raw: 일반적으로 type 처리하기전에 자연어 처리 후 typeCheck함수를 실행하는데, 원문으로 typeCheck에 넘겨줌. 주소를 원문이 필요한 경우
* context: type이 매칭된 경우 user에 저장. 주소, 휴대폰번호 등. true, false 모두 가능
  {types: [{type : type.mobileType, context: false}]}
* init: 여러단계에 걸처서 Dialog호출이 이루어진경우 처음 진입시의 input을 사용함.
  name: '음식점메뉴단일',
  input: {types: [{type: orderTask.restaurantType, init: true}, orderTasks.menuType],
* save: 매치된 type을 task에만 저장하는 것이  아니라, context.dialog에도 자동 저장하여 사용한다. 
  여러 dialog에서 쓰려고 저장해 놓고 싶은데, 함수로 구현하지 않고 간단히 옵션으로 처리하기 위함
* failSave: save로 저장된 context.dialog의 값을 삭제하고 싶을 때 
  예시: { input: {types: [{type: orderTask.restaurantType, failSave: false}]}, output: {callChild: '음식점입력'}}
  context.dialog[type.name] = null;  //구현코드
  
#### common Type

* listType: 리스트 선택, 번호, 텍스트의 일부로도 선택가능 해야함.
  {types: [{name: 'orderRestaurant', listName: 'restaurant', typeCheck: 'listTypeCheck'}]}
  listName: array를 저장하고 있는 변수. 기본은 task.doc
  name: 선택된 값을 저장할 변수. 기본은 type name
* amount: 금액, 십백천만억원 으로 끝나는 말
* mobile: 휴대폰 번호
* phone: 전화번호
* date: 날짜
* time: 시간
* account: 계좌번호, 숫자와 -로 이루어진 문자열

현재는 type.js에 있음 
 
### DM 고려사항

* 이전으로 갔을 때: 처음 진입시 dialog에 저장한 데이터 그대로 있어야하고, 이전으로 갈때도 task를 실행해야함.
* concept: 동의어 사전 구현 필요.
* type matching시에 한번 matching된것은 재실행 하지 않는다.
* Quibble 구현: 명사, 동사, 문장 등으로 구분. 봇별/글로벌 모두 구현 [global-quibbles.js](https://github.com/com2best/bot-web/blob/0.9/custom_modules/global/global-quibbles.js)
* Global 기능: 공통대화, 퀴블, 동의어 등을 적용 가능하도록 custom_moduels/globals
 
### DM 기능 구현 소스

배달봇
[order/order.dialog.js](https://github.com/com2best/bot-web/blob/0.9/custom_modules/order/order.dialog.js)
[Concept](https://github.com/com2best/bot-web/blob/0.9/custom_modules/order/concepts.js): 동의어

CSDemo
[csdemo/csdemo.graph.js]() github에 없으나 로컬 소스에 있는 지 확인

B2B Demo
[Dialog Graph Demo](https://test.moneybrain.ai/developer/bots/graph-dialog/58bd136d0e8e202503d50587/58d1fb01bc12bd4d4e5360cb)

## 2. Question - Answer (QA)

### QA Options

```	
      limit: 8,
      matchRate: 0.34,
      matchCount: 3,
      exclude: ['하다', '이다'],
      mongo: {
        model: 'dialogsetdialogs',
        queryStatic: {$or: queryList},
        queryFields: ['input'],
        fields: 'dialogset input inputRaw output context id' ,
        taskFields: ['input', 'inputRaw', 'output', 'matchCount', 'matchRate', 'dialogset', 'context'],
        minMatch: 1,
      }
```

### QA 고려사항

* QA 검색을 type으로 하여 전체에 사용하고, 메뉴 안에서도 사용 가능하도록
* DM-DL Hybrid: Intent 분석과 QA를 같은 구조로하고, 더 확률이 높은 것을 선택, 하위 구조에서는 우선권
* context 기능: Context 인식, Context 스위칭, 계층적 Context

### QA 구현 참고
B2B Demo
[실시간DL](https://test.moneybrain.ai/developer/user-bots/context-analytics): QA분석, Context분석


## 3. Task
--

ATHENA의 봇은 사용자의 질문에 대한 정해진 답변만을 출력하는 것에 머물지 않습니다. 사용자의 질문, 요청 등 입력에 대해 태스크(업무)를 수행하고, 이를 답변으로 출력하는 것을 목표로 합니다.  
이때 사용자의 요청에 대한 수행할 업무를 정의한 것을 태스크 라고 합니다. 태스크의 구현은 Javascript 언어(Node.js기반)로 구현하며, Task를 비롯한 자료들은 JSON 형태로 정의합니다.  
Task는 *.js파일에서 정의되며, 대화 그래프 편집창 왼쪽 파일트리 혹은 위쪽 탭에서 들어갈 수 있습니다.  

### 태스크 형식
Task의 기본적인 포맷은 다음과 같이 정의됩니다.  

```javascript
var bot = require(path.resolve('config/lib/bot')).getBot('defaultbot');

var defaultTask = {
    name: 'defaultTask',
    action: function(task, context, callback) {
        callback(task, context);
    }
};
bot.setTask("defaultTask", defaultTask);
```
### Action
Action 함수는 Task 내에서 실제 구현이 들어가 있는 함수로 다음과 같이 정의한다.  

```javascript
function actionName(task, context, callback) {	
	// 여기에 Task에 처리할 내용을 구현한다. 
	callback(task, context);			
}
```

Action 함수는 세가지 파라메터 변수를 받으며, 그 정의는 다음과 같습니다.  
첫번째, 파라미터 변수인 task는 action 함수에서 업무 처리에 필요한 정보를 JSON 형태로 받고, 처리한 결과를 담아서 return 하는 데 사용됩니다. 내부적으로는 action이 정의된 task와 같은 JSON 객체이다.  
두번째, 파라미터 변수인 context는 문맥, 상황을 위한 변수입니다. 스마트한 봇을 구현하기 위해 대화의 문맥, 사용자의 정보 들을 context에 담아서 JSON 형태로 전달합니다.  
세번째, 파라미터 변수인 callback은 결과를 return 하는 역할을 합니다. Action 함수는 비동기(Async) 방식으로 구현합니다. 그러므로 업무 처리가 끝나면 반드시 callback을 호출하여야 합니다.  
Action 함수를 세가지 파라미터 변수로 표준화 함으로써 어떠한 업무를 처리하는 Action 함수도 동일한 구조로 구현할 수 있게 합니다. 다양한 업무처리에 필요한 변수가 달라질 수도 있는 점은 task와 context 변수를 JSON 으로 정의해서 필요한 정보를 JSON에 담아서 보내는 방식으로 처리합니다.

#### Task Parameter
자세한 설명 전에 간단한 Action 함수를 예제로 정의해 봅니다. 아래 함수는 http 요청을 해서 웹에서 데이터를 가지고 옵니다. task는 node 기반 javascript로 작성할 수 있어 node library를 사용할 수 있다. 에러 예외 처리 등은 되어 있지 않는 예시용 함수입니다. 

```javascript
function httpAction(task, context, callback) {
	var request = require('request');
	request({url: task.url}, function (error, response, body) {
	  if (!error && response.statusCode == 200) {
		task.content = body;
	    callback(task, context);
	  }
	})
}
```
위에서 request 모듈은 node에서 많이 쓰이는 http request 라이브러리로 자세한 내용은 https://github.com/request/request 를 참고합니다. 

다음으로 Task 파라미터 변수를 설정하는 방법을 알아봅니다.  
Task를 정의할 때 추가하는 방법입니다. 미리 정의된 변수라고 생각할 수 있습니다.

```javascript
var googleTask = {
	url: 'https://www.google.com',
	action: httpAction,
}
```

위와 같이 정의한 googleTask를 실행하면, task.uri 에 있는 google 주소에 접속에 페이지의 HTML을 읽어서 task.content에 저장합니다.

```javascript
var naverTask = {
	url: 'http://www.naver.com',
	action: httpAction,
}
```
위와 같이 정의한 naverTask를 실행하면, task.uri 에 있는 naver 주소에 접속에 페이지의 HTML을 읽어서 task.content에 저장합니다.

#### Context Parameter

context는 해당 task에 제한되지 않은 여러정보들을 담고 있습니다. 이를 통해 현재 사용자의 입력에 해당되는 정보뿐만 아니라, 사용자 정보 등 다양한 상황에 따른 처리를 할 수 있습니다.  
현재 정의된 상황별 context는 다음과 같습니다.

* context.user: 사용자의 정보를 담고 있다. 
* context.bot: 현재 봇의 정보를 담고 있다. 
* context.botUser: 현재 봇을 사용하는 사용자의 정보를 담고 있다. 
* context.dialog: Dialog가 여러단계로 이루어지는 경우 대화의 문맥을 파악하기 위해 사용 한다. 
	
머니브레인 봇의 모든 데이터는 JSON을 기반으로 하므로, bot 개발시 필요한 경우 각 context 수준에 맞게 추가적인 key를 정의하여 사용할 수 있습니다. 

### Task 결과 후처리
Action 함수에서 처리한 결과를 xpaht 와 regexp로 간편하게 추출하여 사용할 수 있습니다. 또는, 결과값을 http 등으로 받는 html 데이터를 xpath 를 사용하여 저장할 수 있습니다.  
아래의 예시와 같이 하면 html 값에서 <title></title> 사이의 값을 xapth로 읽어서 task.title 에 저장합니다. _text: 'body' 로설정한 것은 task.body 에 있는 값을 사용해서 xpath 검색을 한다는 의미로 http 요청으로 받은 데이터가 task.body 에 저장되어 있기 때문입니다.  

```javascript
var xpathTask = {
	url: 'https://www.google.co.kr/search?q=moneybrain.ai',
	action: httpAction,
	xpath: {
	    _text: 'body',
		title: '//title/text()'
	}
}
```
아래와 같이 하면 html 값에서 검색리스트를 task.doc 에 array 형태로 저장합니다. doc 하부에 _repeat에 있는 xpath로 목록을 가져와서 list와 link 값으로 array를 구성합니다. doc 과 다른 이름으로 추가 xpath를 지정하여 여러게의 list 을 담을 수 있습니다.  

```javascript
var xpathListTask = {
	url: 'https://www.google.co.kr/search?q=moneybrain.ai',
	action: httpAction,
	xpath: {
	    _text: 'body'
		title: '//title/text()',
        doc: {
	      _repeat: '//div[@class="srg"]/div[@class="g"]',
          list: '//a/text()',
          link: '//a/@href',
        }			
	}
}
```
정규식(Regular Expression)을 이용하여 결과값을 task 에 저장할 수 있습니다. 아래 예시에서는 정규식을 사용하여 html에서 title 값을 task.title에 저장합니다.  

```javascript
var xpathTask = {
	url: 'https://www.google.com',
	action: httpAction,
	regexp: {
	    _text: 'body',
		title: /<title>(.*)<\/title>/
	}
}
```
아래와 같이 하면 검색 리스트 텍스트에서 regexp로 match하여 task.doc을 array 형태로 저장합니다. _repeat에 있는 정규식 g flag 로 검색하여 여러개의 매치를 찾고, ( ) 를 사용하여 변수에 저장합니다. link와 list의 숫자는 매치 저장에서 저장되는 순서이며 1번부터 시작합니다.  

```javascript
var xpathTask = {
	url: 'https://www.google.com',
	action: httpAction,
	regexp: {
	    _text: 'body',
		title: /<title>(.*)<\/title>/
		doc: {
			_repeat: /<h class="r"><a.*href="(.*)".*>(.*)<\/a></h>/g
			link: 1,
			list: 2
		}
	}
}
```
### PreCallback,  PostCallback

아래와 같이 task에 preCallback, postCallback 함수를 구현하여 action 함수를 호출하기 전, 후에 처리할 작업을 추가할 수 있습니다.  

```javascript
var sampleTask =
{
  name: 'sample',
  preCallback: function(task, context, callback) {
      // 여기서 Action 함수를 호출하기 전에 수행할 일을 구현
	  callback(task, context);
  },
  action: sampleAction,
  postCallback: function(task, context, callback) {
      // 여기서 Action 함수를 호출한 후에 수행할 일을 구현
	  callback(task, context);
  }
};
```

### Action Parameter Definition

Action 함수에서 사용할 Entity 정보를 paramDefs로 정의할 수 있습니다. Action 함수의 Entity 정보를 정의하는 이유는 Dialog를 통해서 Task 를 사용할 때 어떠한 정보를 넘겨주어야 하는지 파악할 수 있기 때문입니다. 필수적인 정보가 입력된 내용에 없을 경우는 봇이 자동으로 사용자에게 질문하여 정보를 얻을 수 있습니다.  

```javascript
var sampleTask =
{
  name: 'sample',
  paramDefs: [
    {name: '휴대폰', type: mobileType, require: false, dialog: '휴대폰Dialog'},
  ],
  action: sampleAction,
};
```

paramDef 정의에는 다음의 속성들이 사용가능합니다.  

* name: 정의할 엔터티 이름으로 task JSON 개체에 같은 이름으로 저장된다. 
* require: boolean 형으로 필수 Entity 여부. 필수 Entity가 task에 없는 경우 봇이 사용자에게 요청하는 질문을 하여 입력을 받는다. 
* question: String 형으로 봇이 사용자에게 Entity를 물어볼 경우 질문 내용
* dialog: 봇이 사용자에게 Entity를 물어볼 경우 단순한 질문이 아니라, 몇단계의 dialog 가 있을 수 있는데 이를 정의한다. String 값이면 같은 이름의 dialog를 찾고, dialog 개체를 바로 참조할 수도 있다. 

### Task 구조화

Task를 한가지 이상 조합하여 상위 Task를 만들 수 있습니다.  

여러개의 Task를 연결하여 실행할 수 있습니다. 아래의 예시에서는 sequenceTask는 sampleTask1을 실행하고, sampleTask2를 실행합니다. 

```javascript
var sampleTask1 =
{
  name: 'sample1',
  action: function (task, context, callback) {
    task.result = 'sample1';
    callback(task, context);
  }
};

var sampleTask2 =
{
  name: 'sample2',
  action: function (task, context, callback) {
    task.result = 'sample2';
    callback(task, context);
  }
};

var sequenceTask = {
	action: 'sequence'
	tasks: [
		sampleTask1,
		sampleTask2,
	]
}
```

while이나 for 문처럼 특정조건이 true일때 까지 반복해서 Task을 수행하게 할 수 있습니다. 조건이 true인지를 체크하는 방법은 수식이나 boolean을 return 하는 함수로 할 수 있습니다. 아래 예에서는 10번까지 Task를 반복 수행합니다.  

```
var sequenceTask = {
	action: 'while'
	whileIf: function(task, context) {
		if(!task.count) task.count = 0;
		else task.count++;
		
	    return task.count <= 10;
    }, 
	actions: [
		sampleTask1,
		sampleTask2
	]
}
```

if 문처럼 특정 조건일때만 Task를 실행하게 할 수 있습니다. 조건이 true인지를 판단하는 것은 조건식이나 function 으로 정의가 가능합니다. 아래 예시에서는 sequence로 여러 task를 실행할 때 각 task if 조건이 맞는 경우에만 실행됩니다.  

```
var sequenceTask = {
	action: 'sequence'
	tasks: [
		{
		  name: 'sample1',
		  if: function(task, context) {
		    return task.mobile != undefined;
		  },
		  action: function (task, context, callback) {
		    task.result = 'sample1';
		    callback(task, context);
		  }
		},
		{
		  name: 'sample2',
		  if: 'context.user.mobile == undefined',
		  action: function (task, context, callback) {
		    task.result = 'sample2';
		    callback(task, context);
		  }
		}
	]
}
```

이러한 task 구조 상에서 다른 상하 task를 참조할 수 있습니다.  

* task.topTask:  최상위 Task
* task.parentTask: 상위 Task
* task.preTask: 이전 Task

### Task 간 데이터 전달

Task의 연결관계, 상하구조에서 데이터를 전달하기 위해서 기본적으로 preCallback, postCallback을 사용할 수 있습니다.  

아래 예시에서는 preCallback을 통해 sequence 등의 이전 task에서 query 값을 가져와서 task.param으로 사용합니다. postCallback을 통해서는 task에서 처리한 result.query 값을 topTask에 저장해 놓습니다. 

```javascript
var sampleTask =
{
  name: 'sample',
  preCallback: function(task, context, callback) {
	  task.param.query = task.preTask.result.query;
	  callback(task, context);
  },
  action: sampleAction,
  postCallback: function(task, context, callback) {
	  task.topTask.param.query = task.result.query;
	  callback(task, context);
  }
};
```
아래와 같이 task 속성을 통해 데이터를 전달할 수도 있습니다.  

```javscript
var sampleTask = {
  name: 'sample',
  data: {
	set: 'replace',
	context: {
	  user: ['result.doc']
	}, 
	task: {
	  topTask: ['result.title'], 
	  parent: ['param.title']
	}
  },
  action: sampleAction
};

var sampleTask = {
  name: 'sample',
  data: {
	get: 'merge',		
    context: ['bot.botName', 'user.mobile'], 
    task: ['topTask.name', parent.result.doc']
  },
  action: sampleAction
};
```

data 속성 안내는 두가지 key 값으로 구분을 합니다.   

* get: context 나 다른 task에서 값을 가져와 현재 task에 저장한다. 
* set: 현재 task의 결과를 context 나 다른 task에 저장한다. 

데이터를 전달하는데 다음의 옵션들이 있습니다.  

* replace: 기본 옵션으로 같은 key의 데이터가 있으면 대체한다.
* merge: 같은 key의 기존 데이터가 있으면 다른 부분만 새로 겹쳐서 합친다.
* concat: 데이터가 array인 경우 두 array를 합쳐서 저장한다. array concat 함수와 동일

### Template Task 

Template task를 사용해서 기존에 있던 task를 재사용하여 확장할 수 있습니다. task의 template 항목에 상속받을 template task 참조를 넣습니다. 참조의 방법은 일반 task참조와 동일합니다.

아래의 예시에서는 googleTask의 url은 그대로 사용하고, moneybrainTask에서 task.param.q 만 변경하여 요청합니다.

```javascript
var moneybrainTask = {
  name: 'moneybrainTask',
  template: googleTask,
  param: {
	  q: 'moneybrain.ai'
  }
};

var googleTask = {
	url: 'https://www.google.com',
	param: {
		q: ''
	},
	action: httpAction,
}
```
### common Task
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


### Task 고려사항

* Task UI: 위 간단한 task의 입력항목을 ui 로 입력하여 코딩 없이 간단한 업무 개발 가능
* Task Server: DM과 독립적으로 실행하여 Local에서 개발 디버깅 가능하게
* Task Module: 제공되는 module에는 여러 테스크가 있으므로 js파일을 모듈로하고 여러 task를 담을 수 있게
* Task 공유: npm 처럼 개발자가 task, action을 등록하고 다른 사람이 사용 가능하도록 (TOBE)
* Task Graph: sequence, if, while등의 조건을 task graph에서 편집 가능하게, task ui 와 결합하면 코딩없이 task를 개발 (TOBE)

### Task 기능 구현 소스
금융봇
[moneybot/faq.js](https://github.com/com2best/bot-web/blob/0.9/custom_modules/moneybot/faq.js) 게시판 크롤링 테스크
[moneybot/fss-crawling.js](https://github.com/com2best/bot-web/blob/0.9/custom_modules/moneybot/fss-crawling.js) 금융정보 크롤링 테스크
배달봇
[order/franchise/lotteria.js](https://github.com/com2best/bot-web/blob/0.9/custom_modules/order/franchise/lotteria.js)   http 롯데리아 자동주문 테스크 
[order/menuData.js](https://github.com/com2best/bot-web/blob/0.9/custom_modules/order/menuData.js) webdriver 배달의민족, 요기요 데이터 수집 테스크

## 4. Context

context는 해당 dialog에 제한되지 않은 여러정보들을 담고 있습니다. 이를 통해 현재 사용자의 입력에 해당되는 정보뿐만 아니라, 사용자 정보 등 다양한 상황에 따른 처리를 할 수 있습니다. 

현재 정의된 상황별 context는 다음과 같습니다.

* context.global: 모든 봇들이 공통으로 사용하는 정보를 담고 있다.
* context.bot: 현재 봇의 정보를 담고 있다.
* context.channel: 현재 채널의 정보를 담고 있다.
* context.user: 사용자의 정보를 담고 있다.
* context.session: 사용자의 한번의 상담 기간 동안의 정보. 채팅의 경우 채팅방 입장 기간 동안을 의미. 10분 동안 대화가 없으면 새로운 세션 생성
* context.topic: 대화 중 하나의 주제에 대한 대화 정보를 저장한다. 
* context.dialog: Dialog가 여러단계로 이루어지는 경우 대화의 문맥을 파악하기 위해 사용 한다.

머니브레인 봇의 모든 데이터는 JSON을 기반으로 하므로, bot 개발시 필요한 경우 각 context 수준에 맞게 추가적인 key를 정의하여 사용할 수 있습니다.

### Global context

* dialogs: 
* commonDialogs:
* tasks:
* actions:
* types:
* typeChecks
* concepts:
* quibbles

### Bot context

context.bot에는 로딩한 bot 객체를 넣는다. 

### Channel context
```
channel: {
  "name": "socket"
}
```
### User context

context.user 에는 db에서 로딩한 user정보와 해당봇의 사용자에 대한 동적정보를 모두 담는다. 

* _id: mongo db id
* name: 
* mobile:
* address: 
 
### Session context
currentDialog
lastDialog
history (dialog)

### Topic context

### Dialog context

```
    dialog : {
      "inRaw": "안녕",
      "inNLP": "안녕",
      "typeMatches": {},
      "typeInits": {},
      "isFail": false,
      "typeDoc": null,
      "page": null,
      "numOfPage": null
    }
```


## 5. Bot

### Bot 설정 정보
bot 설정 정보는 OOO.bot.js 에 저장된다. 

* version: bot의 버젼 (TOBE)
* engineVersion: 엔진의 버젼(TOBE)
* id: 내부 사용하는 id
* name: 사용자에 표시되는 이름
* description: 봇 설명
* use: 해당 봇 사용 여부 
* dev: 개발모드 인지 여부. 때로 실제 거래등을 하지 않고 테스트 하기 위해 개발모드 구분
* public: 해당봇 공개 여부. 혼자만 사용하는 봇도 가능
* language: 봇의 사용 언어
* dialogFiles: Dialog Graph에서 OOO.graph.js 파일의 로딩 순서 설정. 기본은 알파벳 순으로 로딩되는데, graph.js에 있는 dialogs의 순서가 검색 우선순위가 되므로 로딩 순서를 바꿀 수 있게 한다. 
   
* topicKeywords: 형식 String Array. Question Answer에서 주요한 키워드에 대해서 가중치를 두어서 우선 검색되도록 해야 사용자의 의도를 파악한 대화를 잘 답변함. 

  park/park.bot.js
  topicKeywords: ['탄핵', '최순실', '세월호', '구속', '감옥', '검찰', '특검', '청와대', '박근령', '파면', '삼성동', '사저','뇌물',
    '드라마', '길라임', '보톡스', '성형', '시술', '머리', '삼성', '문재인', '안철수', '안희정', '이재명', '이재용', '재벌', '우병우', '김기춘', '장시호', '정유'],

* useAutoCorrection: 오타수정 사용여부

* useLearning(현재는 learning): 지식 학습 사용여부
* quibble: 학습된 답변이 없을 경우. 둘러대기 답변 설정

```
  park/park.bot.js
  useQuibble: true,
  slangQuibbles: require('./quibbles').slangQuibbles,
  nounQuibbles: require('./quibbles').nounQuibbles,
  verbQuibbles: require('./quibbles').verbQuibbles,
  sentenceQuibbles: require('./quibbles').sentenceQuibbles
```
   slangQuibbles: 욕설 퀴블
   nounQuibble: 명사 퀴블. 특정 명사가 나오는 경우에 답변
   verbQuibble: 동사 퀴블. 특정 동사가 나오는 경우에 답변
   sentenceQuibble: 문장 형식에 따른 퀴블. 질문에 대한 둘러대기 등
   
* channels: 메신져 등 채널 정보 
```
channel : {
   kakao: {
     use: true,
     keyboard: { type :"buttons", buttons:["배달주문시작", "배달내역보기", "도움말"]}
   },
   facebook: {
     use: true,
     id: '1166917363364364',
     APP_SECRET :  "eb2974959255583150013648e7ac5da4",
     PAGE_ACCESS_TOKEN :  "EAAJGZBCFjFukBA...,
     VALIDATION_TOKEN : "moneybrain_token"
   },
   line: {
      use: true,
     CHANNEL_ID: '1487430426',
     CHANNEL_SECRET: 'bb522efdeb637a4583807377a93ab527',
     CHANNEL_ACCESS_TOKEN: 'qEZFTLvs+VZc26...'
   },
   naver: {
     use: false,
     clientId: 'c7VNVyIG3s95N4q2LWZQ',
     clientSecret: 'HXWvXdrKi7'
   },
}
```

봇 로딩 시 생성되는 정보

* startDialog: 시작 Dialog 객체
* noDialog: 답변없음 Dialog 객체
* dialogs: Dialog Graph에서 만든 전체 dialogs. children이 포함되어 있어서 하위 트리 검색 가능
* commonDialogs: 공통 Dialogs
* dialogsets: 사용하도록 설정된 대화학습에 있는 dialogset 참조들
* dialogsetContexts: dialogste에서 사용하는 계층화된 context topics 정보
* contexts: 계층화된 context topics 정보
* types: Bot.setType 으로 설정된 모든 bot의 type들
* typeChecks: Bot.setTypeCheck 로 설정된 모든 bot typeCheck
* tasks: Bot.setTask로 설정된 모든 bot task
* actions: Bot.setAction으로 설정된 모든 bot action
* intents: bot에 만들어진 모든 intent 정보
* entities: bot에 만들어진 모든 entity 정보
* concepts: bot의 concepts (동의어) 정보

botUser: {
{
  "orgBot": {}
  
}

### Bot 고려사항

* Bot봇 호출: 어떤 봇에서 "ㅇㅇㅇ봇 불러줘" 하면 다른 봇으로 변경됨

### Bot 구현 소스

[Playchat bot](https://github.com/com2best/bot-web/tree/0.9/custom_modules/playchat): 어떤 봇 불러줘, 인기봇 등 찾아서 바로 사용
[Global Dialog](https://github.com/com2best/bot-web/blob/0.9/custom_modules/global/global-dialogs.js): 봇전환 등 기능 있음

## 6. Knowledge

### Knowledge 구현 소스
[지식그래프](https://test.moneybrain.ai/developer/bots/graph-knowledge/58d2734ae10743e31278f86b)








