##Dilaog 개발 가이드


###1. Quick Start

###2. Dialog

####2.1. Dialog 기본

기본 질문 답변 구조이다. "<" 뒤에 사용자가 입력할 문장을 적고, ">"뒤에 봇이 답변할 문장을 적는다. 입력에는 키워드만 넣고, 동사는 기본형으로 적어야 한다. 

	< 찾다
	> 텍스트 내용을 출력합니다.
 
아래와 같이 한줄로 정의할 수도 있다. 마찬가지고 "<" 뒤에서 ">"까지는 입력이고, ">" 이후는 답변이다. 
 
	<찾다> 텍스트내용을 출력합니다. 
	
Concept을 정의하여, 여러가지 단어를 동의어로 인식하여 매칭할 수 있다. 아래 예시에서는  "응" "좋아" 등 여러가지 입력에 대해 동일하게 인식한다.

	concept: 네 [응 좋아 yes ok] 	
	<~네> 동의하고 진행하겠습니다. 

####2.2. Dialog 입력 인식

Dialog에 정의된 입력을 사용자의 질문에 매치하는 것은 기본적으로 자연어 처리에 기반하여 이루어 진다. 아래에서 매치되는 예시를 보여준다. 

	< 계좌 이체 하다
	
* (매칭) 계좌 이체 해줘.  	      // 사용자가 입력한 "하다" 활용형은 기본형 동사로 매치됨
* (매칭) 계좌 이체좀 해줘.  	  // "이체좀"에서 주요단어는 분리되어 "이체"로 매치됨
* (매칭) 계좌 이체를 해주세요 // "이체를"에서 조사는 분리되어 매치됨 
* (매칭) 해줄래~ 계좌 이체     // 현재는 순서와 상관 없이 인식한다.   	

Regular Expression을 입력의 매치에 사용할 수 있다. 일반적인 정규식을 쓰는 방식과 동일하게 사용이 가능하다.

	</정규식/g> RegExp 매치 되면 내용 출력합니다.

if 조건문을 사용하여 특정조건에서 매치되도록 할 수 있다. 

	< {if: "context.bot.botName == 'sample'"}
	> 조건이 true이면 내용을 출력합니다

조건은 함수로 정의하여 true, false 반환하여 매치할 수 있다. 아래 예시 함수처럼 비동기 (async) 방식이며 callback에 true 또는 false로 호출하는 것에 따라 매치 결과가 나뉜다.

	< {if: function(inRaw, inNLP, dialog, context, callback) { if(inRaw == '함수')callback(true);else callback(false); }}
	> 조건이 true이면 내용을 출력합니다


미리정의된 type을 인식할 수 있다. 아래 내용은 휴대폰번호가 입력 되었을 경우에 반응한다. 아래와 같이 mobile Type을 인식하면 인식된 mobile 값은 mobile 변수에 저장된다. 출력시에 +변수명+ 을 사용하면 저장된 변수를 사용하여 출력할 수 있다. 

	< {types: [type.mobileType]}
	> 휴대폰 +mobile+ 를 출력합니다.
			
이러한 조건은 여러가지를 조합하여 모두 매칭되어야 출력되도록 할 수 있다. 아래 예시에서는 사용자가 "정규식 010-1111-2222"와 같이 regexp와 mobile이 모두 매치되어야 출력된다.

	< {regexp: /정규식/g, type: [type.mobileType], if: function(inRaw, inNLP, dialog, context, callback) {callback(true);}}
	> 모두 매치 되면 내용을 출력합니다.

####2.3. 다중 Dialog
입력 부분을 여러게 정의하면,  여러가지 입력 형태 중에서 하나가 매치되면 출력된다. 이러한 다중 입력을 활용하여 여러가지 형태의 사용자 입력을 반영할 수 있다. 

	< 여러
	< /여러/g
	> 여러개 중에 하나 일치 되면 표시합니다.

출력 부분을 여러게 정의하면, 그 중에 하나가 랜덤하게 출력된다. 이 구현은 사용자가 물어볼 때 마다 다르게 답변할 때 사용한다. 

	< 랜덤
	> 랜덤 첫번재 경우입니다.
	> 랜덤 두번째 경우입니다.

####2.4. 하위 Dialog

아래와 Dialog의 단계를 두어, 상위 단계의 입력을 받은 후에 해당 상황에서만 매치하도록 정의할 수 있다. 이를 통해 단계별 시나리오를 구성하거나, 메뉴 방식의 시나리오를 정의할 수 있다. 

	<하위 질문> 1.메뉴1\n2.메뉴2\n3.메뉴3
	    <1> 메뉴1 내용입니다.
	    <2> 메뉴2 내용입니다.
	    <3> 메뉴3 내용입니다.

여러 단계를 가지는 대화도 구성가능하다.

	<상위 질문> 1.메뉴1\n2.메뉴2,
	    <1> 메뉴1 내용입니다.\n9.이전,
	        <9> {up: 1 }
	    <2> 메뉴2 내용입니다.\n9.이전,
	        <9> {up: 1}

####2.2. Dialog 호출

다른 Dialog를 호출하여 재사용할 수 있다. 호출 할때는 Dialog 이름으로 호출하므로 미리 Dialog 이름을 정의해야 하며, 정의는 Dialog명 뒤에 ":"를 붙여서 정의한다. 

	호출1:
	< 호출대기
	> 첫번째 다이얼로그가 호출 되었을 때 출력됩니다.

아래 처럼 "call"을 이용하여 "호출1" Dialog를 호출하면, 위의 "호출1"에 정의된 출력이 표시된다. 
	
	< 호출 하다 1
	> {call: '호출1'}

아래와 같이 입력부를 false로 하면, 사용자 입력에는 매치되지 않는다. Dialog 이름으로 다른 Dialog이 호출에만 사용된다.

	호출2:
	< false
	> 두번째 다이얼로그가 호출되었을 때 출력됩니다.      
	
	< 호출 하다 2
	> {call: '호출2'}

 위의 다른 함수의 호출에는 option을 사용할 수 있다. 이러한 옵션은 아래 나열되는 up, repeat 등 다른 dialog의 호출에 모두 사용된다 .

	<옵션 1> {call: '호출1', options: {output: '표시되는 내용 교체'}}
	<옵션 2> {call: '호출1', options: {prefix: '앞에\n'}}
	<옵션 3> {call: '호출1', options: {postfix: '\n뒤에'}}

* ouput option: 호출은 하되 출력은 정의한 내용으로 바꾸어 출력한다. 
* prefix option: 호출하면서 표시되는 출력 앞에 붙여서 출력한다. 
* postfix option: 호출하면서 표시되는 출력 뒤에 붙여서 출력한다. 

이전 단계로 이동할 때는 "up"을 이용한다. 아래 예시에서 9를 입력하면 상위로 이동한다. 

	<상위 질문> 1.메뉴1\n2.메뉴2,
	    <1> 메뉴1 내용입니다.\n9.이전,
	        <9> {up: 1 }

 repeat를 사용하면 다시 이전 질문을 반복할 수 있다. 의도한 답변이 없을 때 사용할 수 있다. 

	<하위 질문> 1.메뉴1\n2.메뉴2
	    <1> 메뉴1 안내입니다. 
	    <2> 메뉴2 안내입니다. 
	    <8> {repeat: 1, output: '다시입력해주세요'}

####2.4. 공통 Dialog

사용자가 처음 봇에 접속 했을 때 표시할 내용은 "시작" 이라는 Dialog로 정의한다. 아래와 같이 하면 봇에 처음 접속시에 표시되고, 사용자가 "시작"이라고 입력하여도 매치된다.

	시작:
	<시작> 초기화 메세지 입니다

사용자입력 중에 매치되는 것이 없을 때 표시되는 것으로 아래와 같이 dialog 문서의 마지막에 정의한다. "<>"으로 입력부가 되어 있는 것은, 모든 입력에 매치된다는 것과 같으므로, 위에서 매치되는 문장이 없으면 마지막에 항상 매치되어 표시가 된다. 

	<> 매치되는 문장이 없습니다.                                     

하위 Dialog의 어느 단계에서나 매치될 수 있는 공통 Dialog 는 아래와 같이 입력부 앞에 c를 붙여서 정의한다. 대표적으로 이전으로 가기나, 시작으로 가기는 모든 단계의 dialog에서 계속 활용 되므로 공통 Dialog로 정의해 두고, 하위 Dialog 단계별로 반복적으로 정의하지 않아도 된다. 
    
	c<9> {up: 1}
	c<0> {call: 시작}


Dialog 의 기본 구조는 다음과 같다.  

	{
		input: '주문해줘',
		task : {
			name: 'order',
			action: OrderTask,
		}
		output: '주문이 완료 되었습니다.' 
	}
	
###3. Task

####3.1. Task의 정의

머니브레인의 봇은 사용자의 질문에 대한 정해진 답변만을 출력하는 것에 머물지 않는다. 사용자의 질문, 요청 등 입력에 대해 Task(업무)를 수행하고, 이를 답변으로 출력하는 것을 중심으로 구현하고자 한다. 

이때 사용자의 요청에 대한 수행할 업무의 구현을 Task라고 정의 한다. 

      입력
       |
    Task처리
       |
      출력  

####3.1. Task가 있는 Dialog

sample.dlg에서 아래와 같이 정의 한다.  sampleTasks는 모듈명. helloworldTask는 처리할 Task를 의미한다. 

	< Task 실행
	sampleTasks.helloworldTask
	> +result+ task에서 처리한 내용 표시

위에서 참조한 task는 sample.task.js 에 아래와 같이 정의한다. 
```
var helloworldTask =
{
  name: 'helloworld',
  action: function (task, context, callback) {
    task.result = 'hello world';
    callback(task, context);
  }
};
```

확장자가 dlg인 정의된 Dialog 파일은 javascript 와 형식이 다르므로, dlg에서 사용한 모듈의 정의는 별도로 추가해야 한다.
  위의 sample.dlg에서 sampleTasks 모듈을 사용하기 때문에 아래와 같이 require 부분이 필요하다. sample.include.js는 sample.dlg 파일을 위한 javascript 추가파일이다. 앞부분의 이름을 맞추어 정의한다. 

	var path = require('path');
	var sampleTasks = require(path.resolve('custom_modules/sample/sample.task'));



####3.2. Task Action 함수 만들기
기본적인 Task Action 함수를 작성하는 방법이다. 

```javascript	
function actionName(task, context, callback) {	
	// 여기에 Task에 처리할 내용을 구현한다. 
	callback(task, context);			
}
```

task 파라미터는 JSON 객체로 action에서 처리할 parameter 정보를 받고, 처리한 결과를 보내는 데 사용된다. 

```
var task1 = 
{
	param1: '111',
	param2: '222',
	action: testAction,
}
```

위와 같이 task를 정의하고, 아래와 같이 testAction을 정의한다. 

```
function testAction(task, context, callback) {
	task.result1 = task.param1 + '이 결과 입니다';
	task.result2 = [task.param1, task.param2];
	
	callback(task, context);
}
```

위와 같이 task에 result 값을 설정하여 결과 값으로 보낼 수 있다. task는 기본적으로 JSON 구조이므로 action 개발자가 어떤 형태로 데이터를 내보낼 지는 자유롭게 정할 수 있다. 

위와 같이 하면 

//actionName은 함수 이름으로Task에 맞게 적절히 바꾼다.
// Task 처리후 callback을 호출한다. 
exports.actionName = actionName;		// 다른 모듈에서 사용할 수 있게 exports 해주는 것이 좋다.

####3.2. Context 의 활용

context	는 해당 task에 제한되지 않은 여러정보들을 담고 있다. 이를 통해 현재 사용자의 입력에 해당 되는 정보 뿐만아니라, 사용자 정보 등 다양한 상황에 따른 처리를 할 수 있다. 

현재 정의된 상황별 context는 다음과 같다.

* context.global: 
* context.user: 사용자의 정보를 담고 있다. 
* context.bot: 현재 봇의 정보를 담고 있다. 
* context.botUser: 현재 봇을 사용하는 사용자의 정보를 담고 있다. 
* context.dialog: Dialog가 여러단계로 이루어지는 경우 대화의 문맥을 파악하기 위해 사용 한다. 
	
머니브레인 봇의 모든 데이터는 JSON을 기반으로 하므로, bot 개발시 필요한 경우 각 context 수준에 맞게 추가적인 key를 정의하여 사용할 수 있다. 

####3.3. Common Task 사용하기

####3.4. Task 구조화

###4. Type

####4.1. 사용자 입력에서 Type 넣기

####4.2. Type 만들기

####4.3. Common Type 사용하기



