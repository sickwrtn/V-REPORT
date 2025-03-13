# wrtn.ai

wrtn.ai 다음과 같은 취약점이 있었으며 패치되었습니다.

## reward-api.wrtn.ai

사용된 도구 : dirsearch

문제가 되는 요청 : GET reward-api.wrtn.ai/docs, GET superchat-api.wrtn.ai/docs  

*.wrtn.ai 의 api document 가 노출되어있었습니다.

api document에는 일반적으로 기밀이여야되는 url 경로들이 노출되어있었으며 

해당 경로들의 접근은 그 어떤 인증 절차도 필요하지 않았기 때문에 해당 document에 나와있는 경로로

요청을 보내 악의적인 공격을 수행할수있었습니다.

