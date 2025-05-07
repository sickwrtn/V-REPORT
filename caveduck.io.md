# caveduck.io
caveduck.io 다음과 같은 취약점이 있었으며 패치되었습니다.

# HMCA Jwt Verify Weak Signature Secret Key
KST 2025-05-07 AM 11:53 (UTC+9:00) 시각에 해당 보안 취약점을 발견했으며 보고했습니다.

사용된 도구 : burp suite professional 2025.3.2v extensions by jwt web token

해당 취약점은 JWT Signature의 Secret Key 가 유추가능한 짧은 키로 설정되었었습니다.

Secret Key : helloworld (Brute Force 된 Jwt Secret Key)

해당 Jwt 토큰은 쿠키의 auth_token 으로써 세션토큰의 역할을 수행했었습니다.

## HEADER

```
{
  "alg": "HS256",
  "typ": "JWT"
}
```

## PAYLOAD

```
{
  "id": 3276221,
  "email": "sillo070607@gmail.com",
  "name": "뭐지",
  "emailVerified": 1,
  "permissions": [],
  "iat": 1746610634,
  "exp": 1778168234
}
```

해당 취약점은 caveduck.io 사측에 전달되었으며 패치되었습니다.
