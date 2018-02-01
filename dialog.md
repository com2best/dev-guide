## 0. 자료보기 순서
chatIF

## 1. Dialog
dialog = {
    orgInput: []
    orgOutput: [] 
    input: {
        text: String
        nlp: []
        intents: []
        entities: []
        types: {}
        sentence
        emotion
    }
    task: 
    output: {
        
    }       
}
###  orgInput
    text
    if
    intent
    entity
    types
    function
    
###  Output
output format
    반복 ##
    치환 ++
image
buttons
list

### Actions
    call
    up
    back ?
    repeat
    returnCall 
    return 
    callChild
    
    options: {output, prefix, postfix, returnDialog}
    
## 2. Context
context = {
    bot: {}
    channel: {}
    user: {}
    session: {}
    topic: {}
}


AS-IS
context =
{
    bot: {
    
      "_id": "59700f7f3080aa1802291ce1",
      "user": "578e5cdc06c7dc1d3e86649a",
      "id": "samplegraph",
      "name": "샘플 그래프",
      "description": "샘플",
      "imageFile": "/files/default.png",
      "created": "2017-07-20T02:03:43.438Z",
      "learning": false,
      "followed": 0,
      "dialogsets": [
        "5a1d306e0e6062d7da0a57ea"
      ],
      "learn": false,
      "public": false,
      "using": false,
      "facebook": false,
      "line": false,
      "kakao": false,
      "language": "ko",
      "dialogs": []
    }  
    channel: {
      "name": "socket"
    }`
    global: {
      "dialogs": {},
      "commonDialogs": {},
      "tasks": {},
      "actions": {},
      "types": {},
      "typeChecks": {},
      "concepts": {},
      "messages": {}
    }
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
}



bot : {
    id
    name
    description
    use
    public
    user
    language
    
    kakao: true
    facebook: true
    line: true
    topicKeywors: []
    
    startDialog
    noDialog
    dialogs
    commonDialogs
    dialogsets
    dialogsetContexts
    contexts
    types
    typeChecks
    tasks
    actions
    intents
    entities
    concepts
    messages
}

botUser: {
{
  "orgBot": {}
  
}
## Task

### basic
task = {
    name:
    preCallback: function(dialog, context, callback)
    action: function(dialog, context, callback) 
    postCallback: function(dialog, context, callback)       
}

action function(dialog, context, callback) {
}

### Task Template(상속)

### Task 조합
    Sequence
    while
    if
    data merge option

## Common Modules
##common Type

* listType
* +amount+: 금액, 십백천만억원 으로 끝나는 말
* +mobile+: 휴대폰 번호
* +phone+: 전화번호
* +date+: 날짜
* +time+: 시간
* +account+: 계좌번호, 숫자와 -로 이루어진 문자열

##common Task
    mongoDB
    http
    xpath
    regexp




<!--stackedit_data:
eyJoaXN0b3J5IjpbLTQ2MTMwMzg3MV19
-->