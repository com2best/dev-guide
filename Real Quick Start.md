#테스트
##테스트2
###테스트3


# Real Quick Start (10분만에 챗봇 만들기)
---
Real Quick Start에서는 ATHENA 플랫폼을 사용하여 봇을 만드는 방법에 대한 Step by Step 가이드를 제공합니다.  
가이드를 그대로 따라가세요. 10분만에 직접 챗봇을 만들어 볼 수 있습니다!

## Step 1 : 봇 생성하기
처음 ATHENA에 접속하셨다면, 회원가입을 진행해야 합니다. Real Quick Start에서는 페이스북에 봇을 연결해볼 것이기 때문에, 가능하면 페이스북 로그인을 해주세요.  

<img src="https://github.com/VictorJeon/dev-guide/blob/master/images/signup.png?raw=true" style="width:50%">

로그인을 하고 처음 나오는 화면이 여러분이 가지고 있는 챗봇 리스트입니다. 아직은 아무 봇도 만들어져 있지 않을 것입니다. 새로운 봇을 만들기 위해서 오른쪽 상단에 "신규등록"버튼을 클릭합니다.  

<img src="https://github.com/VictorJeon/dev-guide/blob/master/images/botlist1.png?raw=true">

신규 봇 생성 페이지에서 봇 아이디, 봇 이름, 봇 설명을 입력하면 새로운 봇이 생성됩니다. 이번 가이드에서는 배달음식봇을 만들어 보도록 하겠습니다. 아래 사진과 같이 입력해주세요.

<img src="https://github.com/VictorJeon/dev-guide/blob/master/images/botcreate1.png?raw=true">

## Step 2 : 대화 구성하기
이제 Step 1에서 만든 봇의 대화를 구성하기 위해서 왼쪽 사이드 바의 "챗봇 개발"메뉴를 클릭합니다.  

<img src="https://github.com/VictorJeon/dev-guide/blob/master/images/menu1.png?raw=true" style="width:20%">

챗봇 개발 화면을 보면 두개의 네모 박스가 있을 것 입니다. 이 네모 박스 하나가 한 Dialog입니다. Dialog는 input, task, output으로 이루어져 있으며, Dialog들이 모이고 조합되어 하나의 봇을 구성합니다.  
 
이제 Dialog를 수정하고 추가하여 배달음식봇을 만들어 보도록 하겠습니다. '시작' Dialog의 +버튼을 클릭하여 새로운 Dialog를 만드세요.  

<img src="https://github.com/VictorJeon/dev-guide/blob/master/images/graph10.png?raw=true">
  
Dialog 수정 창에는 Name, Input, Task, Output 4가지 속성이 존재합니다.  

- Name은 챗봇의 이름입니다. **Name에는 `음식 카테고리`라고 입력하세요.**
- Input은 챗봇 사용자의 입력값입니다. **Input에는 `배고파`를 입력하세요.** ATHENA는 한국어 자연어 처리 기능이 탑재되어 있어 자동으로 기본형인 **`배고프다`**로 저장될 것입니다. 입력 후 저장을 눌러주세요.  
- Output은 출력값으로, 사용자에게 보여지는 값입니다. **다음과 같이 입력해주세요. `배가 고프신가요? 제가 주문을 해드리겠습니다. 다음 중 어떤 것을 먹고 싶으신가요? 1.치킨 2.피자`** 입력 후 저장을 눌러주세요.

<img src="https://github.com/VictorJeon/dev-guide/blob/master/images/dialogedit1.png?raw=true" style="width:60%">

이제 Dialog를 테스트 해보겠습니다. 오른쪽 상단에 저장 버튼을 누르고, 대화창에 "나 배고파"를 입력합니다.  

잘 작동된다면 이제 Child Dialog(하위 Dialog)를 추가해보겠습니다. '음식 카테고리' Dialog의 +버튼을 클릭해 Child Dialog를 생성합니다. '음식 카테고리' Dialog에서 `1.치킨 2.피자`라고 했기 때문에, 거기에 대한 Child를 만들어 줘야 합니다. 아래와 같이 작성합니다.  

<img src="https://github.com/VictorJeon/dev-guide/blob/master/images/dialogedit2.png?raw=true" style="width:60%">

**output에 텍스트와 함께 이미지를 넣을 수 있습니다. 치킨 이미지파일을 올려보세요.*

`2.피자`를 선택하는 Dialog도 마찬가지로 작성합니다.  

완료 후에는 저장하고 테스트를 해봅니다. 잘 작동한다면 대화 학습파트를 진행합니다.

<img src="https://github.com/VictorJeon/dev-guide/blob/master/images/save.png?raw=true">  

<img src="https://github.com/VictorJeon/dev-guide/blob/master/images/chattest.png?raw=true" style="width:30%">

## Step 3 : 대화 학습하기
ATHENA에는 보조적으로 대화학습기능을 제공합니다. 여러분의 봇을 '질문:답변 = 1:1' 방식으로 구성된 데이터를 통해 딥러닝 학습시킬 수 있습니다. 대화학습에 필요한 데이터는 1:1 표를 통해 직접 작성할 수도 있고 csv, 카카오톡 백업파일, smi 파일을 import하여 등록할 수도 있습니다.  ##

대화학습 데이터를 새로 등록하기 위해서는 왼쪽 사이드바의 "개별 대화 학습" 메뉴에서 신규등록 버튼을 클릭합니다.

<img src="https://github.com/VictorJeon/dev-guide/blob/master/images/menu2.png?raw=true" style="width:20%">

아래 예시와 같은 대화학습 데이터를 작성해보세요.

<img src="https://github.com/VictorJeon/dev-guide/blob/master/images/deeplearning.png?raw=true">

완료 후에는 테스트를 해봅니다. 잘 작동한다면 봇을 자신의 페이스북 페이지에 연결해봅시다.

<img src="https://github.com/VictorJeon/dev-guide/blob/master/images/chattest2.png?raw=true" style="width:30%">

## Step 4 : 봇 연결하기
ATHENA에서는 챗봇을 카카오톡, 라인, 페이스북 메신저 3가지 채널에 연결할 수 있습니다. 봇을 자신의 계정에 연결하여 사용자들에게 배포해보세요!  

봇을 연결하기 위해서는 왼쪽 사이드바의 "채널 관리" 메뉴에서 "챗봇 직접 서비스하기 > 페이스북 메신저 > 메신져봇 생성" 버튼을 클릭합니다.  

봇을 연결하고 싶은 페이지에 연결하기 버튼을 클릭합니다. 그리고 대화하기 버튼을 클릭하여 페이스북 메신저에서 봇과 대화를 진행할 수 있습니다.

<img src="https://github.com/VictorJeon/dev-guide/blob/master/images/integrate.png?raw=true" style="width:30%">

**기존의 페이지가 없는 경우에는 페이스북 페이지를 만들어야합니다. 테스트용으로 만들기 쉬운 페이지는 비영리/자선단체 페이지입니다.*


