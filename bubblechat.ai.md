# bubbletap.com
bubbletap.com에 다음과 같은 취약점이 있었으며 패치되었습니다.

## SQL Injection

사용된 도구 : burp suite professional 2025.1.4v

문제가 되는 요청 : GET api.bubblechat.ai/users/{userId}/identity-verifications?select=*

select 파라미터에 sqli 구문이 들어갈시

```json
{
  "statusCode":500,
  "code":"TYPEORM_QUERY_ERROR",
  "message":["You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near '' FROM     `user_identity_verifications` `user_identity_verifications` LEFT JOIN `us' at line 1"],
  "data":{}
}
```

위와 같이 SQL Query Error 구문이 그대로 노출되며 Mysql DBMS를 사용한다는 정보를 확인가능합니다.

그렇기 때문에 [sqlmap](https://github.com/sqlmapproject/sqlmap/tree/master) 을 사용하여 해당 파라미터를 검사하였고

**Boolean-based Blind SQL injection** 취약점과 **Time-based Blind SQL injection** 이 발견되었습니다.

해당 문제는 bubblechat.ai 사측에 전달되었으며 패치되었습니다.

## Bypassed Insecure Direct Object Reference (BIDOR)

사용된 도구 : burp suite professional 2025.1.4v

문제가 되는 요청 : POST api.bubblechat.ai/persona-chats, GET api.bubblechat.ai/chats/{chatId}

해당 취약점은 userPersona의 정보를 우회적인 수평적권한 상승을 사용해 userPersonaId로 조회하는 취약점입니다.

POST api.bubblechat.ai/persona-chats 은 새로운 채팅방을 생성하는 요청입니다.

body 값
```json
{
  "personaId":"423",
  "callName":"asf",
  "universeId":"218",
  "sceneId":"252",
  "userPersonaId":"18443",
  "refModelCode":"DIAMOND",
  "firstMessage":"\"정식으로 인사하마. 나는 제국의 제 1황녀 아리아 그웬돌린. 앞으로 그대가 내게 있어 얼마나 쓸모 있는 존재일지, 그 가치를 천천히 가늠해 보겠다.\" *아리아는 팔짱을 끼며 오만한 태도로 덧붙여 말한다.* \"바라건대 나를 실망시키지 말아다오.\"",
  "isAdult":true,
  "isNovel":false
}
```

이곳에서 가장 중요한점은 userPersonaId 입니다.

userPersona란 유저의 개인정보와 비슷하다 생각하시면 됩니다.

이 정보는 기본적으로 userPersonaId 만으론 조회할수없으며 접근할수없습니다.

하지만 POST api.bubblechat.ai/persona-chats 의 body값중 "userPersonaId" 항목을 사용하면

채팅방 생성시 참조되는 userPersona를 userPersonaId로 임의로 바꿀수있었습니다.

이것만으론 문제가 되지 않을수있지만

GET api.bubblechat.ai/chats/620649

요청을 통해 채팅방의 정보를 불러올경우

응답
```js
{
  ...
  "userPersona" : {/*userPersona정보*/}
  ...
}
```

채팅방 정보에 userPersona 정보가 그대로 참조되는 문제가 발견되었습니다.

해당 문제는 bubblechat.ai 사측에 전달되었으며 패치되었습니다.


