
# ATHENA Developing Guide
---

## 1. 개요(Overview)

### 1.1. Introduction
[ATHENA](https://www.playchat.ai/developer/)는 머니브레인에서 개발한 챗봇 플랫폼으로서, Dialog Graph를 중심으로 챗봇을 직접 개발하고, 배포할 수 있는 툴입니다.


### 1.2. Tutorial

## 2. 시작해보기(Getting Started)

### 2.1. Make ChatBot
챗봇을 개발하기 위해서는, 일단 챗봇을 신규등록해야합니다.  
왼쪽 사이드바의 "챗봇관리 > 챗봇 리스트" 메뉴에서 신규등록 버튼을 클릭하여 챗봇을 등록합니다.  

![image](https://github.com/VictorJeon/dev-guide/blob/master/images/botlist.png?raw=true)

봇 생성 페이지에서 봇 아이디, 봇 이름, 봇 설명, 대화파일, 템플릿을 입력하면 새로운 봇이 생성됩니다.  

- 봇아이디 : 영문자로 입력. unique한 값이며, 봇의 directory name.
- 봇이름 : 자유롭게 입력. 중복입력 가능. 실제로 다른 사람들에게 보이는 값.
- 설명 : 봇에 대한 간략한 설명.
- 대화파일 : Dialog Set 파일 등록. [4. Dialogset](#dialogset) 참조.

![image](https://github.com/VictorJeon/dev-guide/blob/master/images/botcreate.png?raw=true)

봇 생성을 한 후에는, 오른쪽 위 선택창에서 봇을 선택해주세요. (봇이 보이지 않을 시, 새로 고침 후에 다시 시도해주세요.)  

![image](https://github.com/VictorJeon/dev-guide/blob/master/images/botselect.png?raw=true)

## 3. 자연어 이해(NLU)
ATHENA에는 머니브레인의 자체 NLU모듈이 탑재되어있습니다. 사용자가 입력한 문장은 NLU모듈을 통해 분석되고, 결과값으로 Entity와 Intent를 받아옵니다. 개발자는 Entity와 Intent를 Dialog의 Input으로 설정하여 대화를 발생시킬수 있습니다. 또한 Entity를 Parameter로 사용하여, Task를 개발할 수 있습니다.

### 3.1. <a name="entity"></a>개체명(Entity)
Entity는 자연어 입력에서 필요한 parameter를 추출하는 강력한 도구입니다. 특정 봇에서 정의된 Entity는 봇의 기능에 따라 달라집니다. 즉, 모든 개념에 대하여 Entity를 설정할 필요는 없습니다.  
Entity를 새로 만들기 위해서는 왼쪽 사이드바의 "자연어 이해(NLU) > 개체명 분석(Entity)" 메뉴에서 신규등록 버튼을 클릭합니다.  

![image](https://github.com/VictorJeon/dev-guide/blob/master/images/entitylist.png?raw=true)  

Entity 생성 페이지에서 엔터티명, 내용을 입력하면 새로운 Entity가 생성됩니다.  

- 엔터티 : 엔터티명. 이후 이 값을 대표명으로 사용하게 됩니다.
- 내용 추가 : Entity에 속하는 내용을 입력하고 추가버튼 혹은 enter를 누르면, 내용이 목록에 추가됩니다.
- 목록 : 각 내용에 동의어를 추가할 수 있습니다.

![image](https://github.com/VictorJeon/dev-guide/blob/master/images/entitycreate2.png?raw=true)


### 3.2. <a name="intent"></a>의도(Intent)
Intent는 입력한 문장들을 토대로 의도를 분석하는 도구입니다. Input으로 사용하기 위해 쓰이며, 봇의 Dialog 구조에 따라 필요한 Intent를 생성하여 사용합니다.  
Intent를 새로 만들기 위해서는 왼쪽 사이드바의 "자연어 이해(NLU) > 의도 분석(Intent)" 메뉴에서 신규등록 버튼을 클릭합니다.  

![image](https://github.com/VictorJeon/dev-guide/blob/master/images/intentlist.png?raw=true)  

Intent 생성 페이지에서 인텐트명, 내용을 입력하면 새로운 Intent가 생성됩니다.  

- 인텐트 : 인텐트명. 이후 이 값을 대표명으로 사용하게 됩니다.
- 내용 추가 : Intent의 의미를 가지는 문장을 입력하고 추가버튼 혹은 enter를 누르면, 문장이 목록에 추가됩니다.

![image](https://github.com/VictorJeon/dev-guide/blob/master/images/intentcreate.png?raw=true)
## <a name="dialogset"></a>4. DialogSet
대화셋(DialogSet)은 딥러닝을 통한 챗봇 개발을 할 수 있게 하는 도구입니다. 봇은 그래프와 대화셋 두가지 방법으로 구성될 수 있고, 함께 사용하는 것도 가능합니다. 대화셋은 '질문:답변 = 1:1' 방식으로 작성되어야 하며, 사용자의 입력 문장과 가장 유사도가 높은 질문을 찾아 답변을 출력하는 Retreival방식의 모델을 사용합니다. 그래프와 대화셋을 같이 사용하였을 경우에는, 그래프가 답변에서 우선순위를 가집니다.  
DialogSet을 새로 등록하기 위해서는 왼쪽 사이드바의 "대화 관리 > 대화셋 관리" 메뉴에서 신규등록 버튼을 클릭합니다.  

![image](https://github.com/VictorJeon/dev-guide/blob/master/images/dialoglist.png?raw=true)

대화셋 등록 페이지에서 Title, Type, 공개여부, 파일을 입력하면 새로운 대화셋이 등록됩니다.

- Title : 대화셋명. 이후 이 값을 대표명으로 사용하게 됩니다.
- Type : 대화셋 파일의 유형. 현재 ATHENA에서는 csv, 카카오톡 백업파일, smi 세가지 유형의 파일을 사용할 수 있습니다.  
\* 참고 : csv 파일의 형식은 '행 수는 제한 없음, 열 수는 2열, 1열은 질문, 2열은 답변'입니다.
- File : 파일을 등록합니다. Type에서 선택한 형식의 파일 등록.
- Content : 대화셋에 대한 설명을 입력.

![image](https://github.com/VictorJeon/dev-guide/blob/master/images/dialogcreate.png?raw=true)

대화셋을 등록한 이후에는, 관리자페이지에서 내용을 수정할 수 있습니다. 대화셋 리스트에서 수정하고자 하는 대화셋의 '대화 관리'버튼을 클릭하여 내용을 수정할 수 있습니다.

![image](https://github.com/VictorJeon/dev-guide/blob/master/images/dialogedit.png?raw=true)

## 5. 챗봇 개발(Developing)
ATHENA는 대화를 그래프 형식의 UI로 만들어, 사용자가 쉽게 편집하고, 개발할 수 있는 툴을 제공합니다. 그래프는 \*.graph.js 형태의 파일로 저장되고, Task는 \*.js의 파일 형태로 저장됩니다.

### 5.1. Graph
그래프를 사용하기 위해서는 오른쪽 위 선택창에서 봇을 선택한 후, 왼쪽 사이드바 중 "챗봇 개발"을 클릭합니다. 네모난 박스가 input-task-output 구조를 가진 하나의 Dialog입니다. 상단 메뉴바의 "detailed"를 클릭하면, 확장된 형태의 Dialog box를 볼 수 있습니다. Dialog간의 관계는 선으로 표현되며, Children 관계는 회색선, Call 관계는 주황색 선으로 표현됩니다.

![image](https://github.com/VictorJeon/dev-guide/blob/master/images/graph1.png?raw=true)

완성 그래프 예시

![image](https://github.com/VictorJeon/dev-guide/blob/master/images/graph2.png?raw=true)

#### 5.1.1. Add & Delete
그래프에서 Child Dialog를 추가하기 위해서는 Parent Dialog의 +버튼을 클릭합니다. (단축키 : Insert)

![image](https://github.com/VictorJeon/dev-guide/blob/master/images/graph3.png?raw=true)

Dialog를 삭제하기 위해서는 삭제하고자 하는 Dialog를 선택하고 x버튼을 클릭합니다. (단축키 : Del)

![image](https://github.com/VictorJeon/dev-guide/blob/master/images/graph4.png?raw=true)

#### 5.1.2. Edit
Dialog를 수정하기 위해서는, 수정하고자 하는 Dialog를 선택하고, 수정버튼 (+의 왼쪽 버튼)을 클릭합니다. (단축키 : Enter)  

![image](https://github.com/VictorJeon/dev-guide/blob/master/images/graph5.png?raw=true)

Dialog는 Input-Task-Output구조로 이루어져 있습니다.  

- Name : Dialog 이름. 이 후 이 값을 대표명으로 사용하게 됩니다.
- Input : [5.1.2.1. Input](#input) 참조.
- Task : [5.1.2.2. Task](#task) 참조.
- Output : [5.1.2.3. Output](#output) 참조.

![image](https://github.com/VictorJeon/dev-guide/blob/master/images/graph6.png?raw=true)

##### <a name="input"></a>5.1.2.1. Input
Input은 자연어 처리된 사용자 입력에서 Dialog를 발동시키기 위한 것을 선택하는 도구입니다. Input에는 Keyword, Regexp, Type, If, Intent, Entity 6가지를 입력할 수 있습니다.  

- Keyword : 단순 keyword를 입력할 수 있습니다. 버튼 아래에 자연어 처리된 preview를 볼 수 있으며, 사용자가 keyword가 포함된 문장을 입력할 시, 발동됩니다. (keyword 사이에 띄어쓰기를 할 시, AND 조건으로 처리됩니다.)
- Regexp : Regular Expression을 입력할 수 있습니다. 사용자가 regexp에 match되는 문장을 입력할 시, 발동됩니다.
- Type : 주소, 핸드폰 번호, 자동차 번호, 날짜, 시간 등을 인식하는 Type을 선택할 수 있습니다.
- If : (Task를 사용할 때 사용됩니다.) Javascript의 if 조건문과 동일하게 사용됩니다. 결과값이 True일 시, 발동됩니다. (if를  사용할 때는 if를 사용한 input을 두 개 이상 OR조건으로 사용하여야 합니다.)
- Entity : 등록한 Entity를 선택할 수 있습니다. [3.1 Entity](#entity) 참조
- Intent : 등록한 Intent를 선택할 수 있습니다. [3.2 Intent](#intent) 참조

Input의 AND조건은 다음과 같이 입력합니다.

![image](https://github.com/VictorJeon/dev-guide/blob/master/images/graph7.png?raw=true)

Input의 OR조건은 다음과 같이 입력합니다.

![image](https://github.com/VictorJeon/dev-guide/blob/master/images/graph8.png?raw=true)

##### <a name="task"></a>5.1.2.1. Task
Task는 Dialog에서 function을 수행하기 위한 도구입니다. Dialog에서 발동할 Task를 선택할 수 있습니다. 자세한 사항은 [5.2 Task](#task)를 참조하시기 바랍니다.

##### <a name="output"></a>5.1.2.1. Output
Output은 사용자에게 출력한 답변을 선택하기 위한 도구입니다. Output에는 Text, Image, Button, If, Call, ReturnCall, CallChild, Repeat, Up 9가지를 입력할 수 있습니다.  

- Text : 답변을 텍스트 형태로 작성할 수 있습니다.
- If : (Task를 사용할 때 사용됩니다.) Javascript의 if 조건문과 동일하게 사용됩니다. 결과값이 True일 시, 발동됩니다. (if를  사용할 때는 if를 사용한 Output을 두 개 이상 OR조건으로 사용하여야 합니다.)
- Image : Image파일을 업로드 할 수 있습니다. 답변으로 Image를 내보냅니다.
- Button : Button을 입력할 수 있습니다. 사용자는 타이핑 대신 버튼을 클릭해서 입력할 수 있습니다.
- Call : 다른 Dialog를 호출하여 재사용할 수 있습니다. 호출 할때는 Dialog 이름으로 호출합니다.
- ReturnCall : 
- CallChild : 
- Repeat : repeat를 사용하면 다시 이전 질문(Parent Dialog)을 반복할 수 있습니다. 의도한 답변이 없을 때 사용할 수 있습니다.
- Up : 이전 단계(Parent의 Parent Dialog)로 이동할 때는 up을 이용합니다.

Output의 AND조건은 다음과 같이 입력합니다.

![image](https://github.com/VictorJeon/dev-guide/blob/master/images/graph9.png?raw=true)

Output의 OR조건은 다음과 같이 입력합니다. Output을 OR조건으로 입력할 시, OR 조건의 답변들 중  랜덤선택됩니다. (if 조건문을 사용하지 않은 경우)

![image](https://github.com/VictorJeon/dev-guide/blob/master/images/graph8.png?raw=true)

#### 5.1.3. Shortcut
##### Dialog Graph
- ← ↓ ↑ → : Navigate Dialog  
- Ctrl + ↑ → : Move Dialog Up/Down  
- Enter : Open Edit Dialog  
- Esc : Cancel Edit  
- Insert : Add child dialog  
- Del : Delete Dialog  
- Space : Expand/Collapse Child Dialog  

##### Dialog Editor
- Tab : Next (Input, Task, Output 전환, Input 등 여러 개 인경우 한줄씩 이동)  
- Ctrl+Enter : Save Dialog Modal  

##### Code Editor
- Alt+← : Back (Dialog Graph에서 Code Editor로 온 후에 복귀)  

#####Project Files  
- Ctrl+P : Open/Close Project Files  
- ← ↓ ↑ → : Navigate Tree  
- Enter : Open File  

##### Common
- / : Search Box  
- Ctrl+S : Save File  
- Ctrl+Z : Undo  
- ? : keyboard shortcut help  

### <a name="task"></a>5.2. Task
ATHENA의 봇은 사용자의 질문에 대한 정해진 답변만을 출력하는 것에 머물지 않습니다. 사용자의 질문, 요청 등 입력에 대해 Task(업무)를 수행하고, 이를 답변으로 출력하는 것을 목표로 합니다.  
이때 사용자의 요청에 대한 수행할 업무를 정의한 것을 Task 라고 합니다. Task의 구현은 Javascript 언어로 구현하며, Task를 비롯한 자료들은 JSON 형태로 정의합니다.  
Task는 *.js파일에서 정의되며, 편집창 왼쪽 파일트리 혹은 위쪽 탭에서 들어갈 수 있습니다.  

### 5.2.1. Task Form
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
### 5.2.2. Action
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

#### 5.2.2.1. Task Parameter
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

#### 5.2.2.2. Context Parameter

context는 해당 task에 제한되지 않은 여러정보들을 담고 있습니다. 이를 통해 현재 사용자의 입력에 해당되는 정보뿐만 아니라, 사용자 정보 등 다양한 상황에 따른 처리를 할 수 있습니다.  
현재 정의된 상황별 context는 다음과 같습니다.

* context.user: 사용자의 정보를 담고 있다. 
* context.bot: 현재 봇의 정보를 담고 있다. 
* context.botUser: 현재 봇을 사용하는 사용자의 정보를 담고 있다. 
* context.dialog: Dialog가 여러단계로 이루어지는 경우 대화의 문맥을 파악하기 위해 사용 한다. 
	
머니브레인 봇의 모든 데이터는 JSON을 기반으로 하므로, bot 개발시 필요한 경우 각 context 수준에 맞게 추가적인 key를 정의하여 사용할 수 있습니다. 

### 5.2.3. Task 결과 후처리
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
### 5.2.4. PreCallback,  PostCallback

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

### 5.2.5. Action Parameter Definition

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

### 5.2.6. Task 구조화

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

### 5.2.7. Task 간 데이터 전달

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

### 5.2.8. Template Task 

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

## 6. 운영 관리(Management)
ATHENA는 실패한 대화를 체크하고, 다시 대화에 반영하여 봇이 말을 잘 할 수 있게 하는 운영 관리 기능을 제공합니다.

### 6.1. Dialog 대화학습
Dialog 대화학습에서는, 대화가 실패했을 때의 상황을 모아 보여줍니다. (대화 실패 : 기본 Dialog 발생)  
![image](https://github.com/VictorJeon/dev-guide/blob/master/images/manage1.png?raw=true)
표의 첫번째 열인 대화에서는 대화가 실패했을 때 사용자가 입력한 input을 보여줍니다. 각 대화 오른쪽의 대화수정 버튼을 클릭하면, 실패한 대화를 의도에 맞게 재배치하여 업데이트 할 수 있습니다.

### 6.1. 의도 분석 대화학습
의도분석 대화학습에서는, 대화가 실패했을 때, 인텐트를 모아 보여줍니다. (대화 실패 : 기본 Dialog 발생)  
![image](https://github.com/VictorJeon/dev-guide/blob/master/images/manage3.png?raw=true)
표의 실패 인텐트는 "추천 인텐트"를 뜻합니다. 오른쪽의 대화 수정 버튼을 누르면, 실패한 인텐트 들이 나옵니다. 실패한 인텐트들 중 추천 인텐트와 같은 의미를 가진다고 판단되는 것들을 체크하여 추가하면, 인텐트가 업데이트 됩니다.
![image](https://github.com/VictorJeon/dev-guide/blob/master/images/manage4.png?raw=true)

## 7. 분석(Analytics)
ATHENA에서는 봇에 대한 기본적인 Analytics를 제공하고 있으며, 웹 상으로 쉽게 확인할 수 있습니다.
### 7.1. Dashboard
봇에 대한 전반적인 Analytics를 보여주는 대쉬보드입니다.  

![image](https://github.com/VictorJeon/dev-guide/blob/master/images/anal1.png?raw=true)

### 7.2. 사용자수 분석
일별 사용자수를 보여주는 분석 창입니다.  

![image](https://github.com/VictorJeon/dev-guide/blob/master/images/anal2.png?raw=true)

### 7.3. 대화별 사용량
대화별 사용량 (사용자 input)을 보여주는 분석 창입니다.  

![image](https://github.com/VictorJeon/dev-guide/blob/master/images/anal3.png?raw=true)

### 7.4. 문장 성공률
월별 문장 성공률을 보여주는 분석 창입니다.  

![image](https://github.com/VictorJeon/dev-guide/blob/master/images/anal4.png?raw=true)

### 7.5. Context 분석
DialogSet을 사용할 시에, 딥러닝 유사도를 체크해볼 수 있는 Context 분석 창입니다.  

![image](https://github.com/VictorJeon/dev-guide/blob/master/images/anal5.png?raw=true)
