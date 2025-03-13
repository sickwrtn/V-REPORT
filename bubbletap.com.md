# bubbletap.com
bubbletap.com에 다음과 같은 취약점이 있었으며 패치되었습니다.

## Boolean-based Blind SQL Injection

GET api.bubblechat.ai/users/{userId}/identity-verifications?select=* 중

select 파라미터에 ' 키워드가 들어갈시

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

Boolean-based Blind SQL injection 취약점과

Time-based Blind SQL injection이 발견되었습니다.

해당 문제는 bubblechat 사측에 전달되었으며 패치되었습니다.


해다
