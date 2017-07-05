## 신한카드봇 메시지 및 버튼 수정 방법
### 1. 관리자 페이지 접속

챗봇을 수정하기 위해서는 관리자 페이지에 접속해야 합니다.

관리자페이지: [https://shinhancard.moneybrain.ai](https://shinhancard.moneybrain.ai)

ID: shinhancard@moneybrain.ai  
PS: shinhan12

### 2. 메시지 및 버튼 수정하기

왼쪽 상단에 챗봇 개발 메뉴를 클릭합니다.

마인드맵처럼 보이는 것이 신한카드봇의 대화 그래프입니다. 매뉴얼에 따라 이곳에서 대화를 수정 및 편집할 수 있습니다.  

수정하고싶은 대화를 찾는 방법은 검색 기능을 이용하는 것입니다.

왼쪽 상단에 검색창이 있습니다. name으로 설정된 것을 output으로 바꾸고, 원하는 Dialog의 output으로 검색해보세요.

### 3. Task로 처리된 메시지 수정하기

몇가지 Dialog는 그래프 외에 Task를 통해 처리되었습니다.

그러한 Dialog를 수정하기 위해서는 default.js 파일에서 직접 수정해야합니다.

아래는 Task를 쓴 Dialog들입니다.

#### FAN 가입 : task1

FAN 가입 url은 채널에 따라 달라지기 때문에 task로 처리되었습니다.

FAN 가입의 url를 변경하기 위해서는 상단 탭에서 default.js를 선택한 후,

```javascript

var task1 = {
    name: 'task1',
    action: function (task, context, callback) {
        if (context.user.channel == 'navertalk' || context.user.channel == 'socket') {
            task.buttons = [
                {
                    text: "바로가입하기",
                    url: "https://newm.shinhancard.com/event/2015/pt06.jsp?prm=naver"
                }
            ]
        } else if (context.user.channel == 'facebook') {
            task.buttons = [
                {
                    text: "바로가입하기",
                    url: "https://newm.shinhancard.com/event/2015/pt06.jsp?prm=facebook"
                }
            ]

        } else if (context.user.channel == 'kakao') {
            task.buttons = [
                {
                    text: "바로가입하기",
                    url: "https://newm.shinhancard.com/event/2015/pt06.jsp?prm=kakao"
                }
            ]

        }
        callback(task, context);
    }
}

bot.setTask("task1", task1);
```
를 찾아 변경해주시면 됩니다.

if (context.user.channel == 'navertalk') {...} 안에 들어간 url은 네이버 톡톡 url입니다.  
if (context.user.channel == 'facebook') {...} 안에 들어간 url은 페이스북 url입니다.  
if (context.user.channel == 'kakao') {...} 안에 들어간 url은 카카오톡 url입니다.

#### 카드 추천 랜딩 페이지

각 카드의 랜딩 url 또한 채널에 따라 달라지기 때문에 task로 처리되었습니다.

랜딩 url를 변경하기 위해서는 상단 탭에서 default.js를 선택한 후, 카드 이름의 변수를 찾습니다.

예시) YOLO카드의 랜딩페이지를 변경하고 싶을때는,

```javascript
var YOLO = {
    name: 'YOLO',
    action: function(task, context, callback) {
        if (context.user.channel == 'navertalk' || context.user.channel == 'socket') {
            task.buttons = [
                {
                    text: "바로가기",
                    url: "https://m.shinhancard.com/mob/MOBFM038N/MOBFM038C03.shc?EntryLoc=2804&tmEntryLoc=TM2534&empSeq=563&datakey&agcCd="
                }
            ]

        } else if (context.user.channel == 'facebook') {
            task.buttons = [
                {
                    text: "바로가기",
                    url: "https://m.shinhancard.com/mob/MOBFM038N/MOBFM038C03.shc?EntryLoc=2804&tmEntryLoc=TM2534&empSeq=562&datakey&agcCd="
                }
            ]

        } else if (context.user.channel == 'kakao') {
            task.buttons = [
                {
                    text: "바로가기",
                    url: "https://m.shinhancard.com/mob/MOBFM038N/MOBFM038C03.shc?EntryLoc=2804&tmEntryLoc=TM2534&empSeq=561&datakey&agcCd="
                }
            ]

        }
        callback(task, context);
    }
};
bot.setTask("YOLO", YOLO);
```

여기서 각 채널별로 url을 수정해주면 됩니다.

#### 카드 추천 리스트

카드를 여러개 추천해주고 선택해야하는 경우에 페이스북, 네이버톡톡과 카카오톡의 UI가 달라집니다.

그에따라 Dialog에서 분기가 되어있고, 페이스북과 네이버톡톡은 카드형태로 보여줘야하기 때문에 task로 처리가 되었습니다.

여기에 따른 task는 cardlist1~6로 6개가 있고, 여기서 채널별, 카드별로 url을 수정할 수 있습니다.



