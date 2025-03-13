# elyn.ai
elyn.ai 다음과 같은 취약점이 있었으며 패치되었습니다.

# JWT Signature Bypass
KST 2025-03-13 AM 4:53 (UTC+9:00) 시각에 해당 보안 취약점을 발견했으며 보고했습니다.

사용된 도구 : burp suite professional 2025.1.4v extensions by jwt web token

해당 취약점은 JWT Signature 를 제대로 검증하지 않아 JWT를 임임로 변경하여 회원 권한 탈취가 가능했습니다.

해당 사이트의 JWT token
```
eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCIsImtpZCI6IklOdUJSeHFtdkxKR0JoZjUtOG85bCJ9.eyJodHRwczovL2xpdmUuZWx5bi5haS91c2VyX21ldGFkYXRhIjp7ImVtYWlsIjoidGFlaG8wNzA2MDdAZ21haWwuY29tIiwidXNlcl9pZCI6ImJjYzA0ZWMxLTkwZTMtNGFlYi05ZmEzLTU5MTg4NzFlM2MwNCIsInVzZXJuYW1lIjoic2lsbG8ifSwiaXNzIjoiaHR0cHM6Ly9lbHluLmpwLmF1dGgwLmNvbS8iLCJzdWIiOiJhdXRoMHw2N2QxY2E0OTJmZTMxZmMzNmE3ZjcxOTAiLCJhdWQiOlsiaHR0cHM6Ly9lbHluLmpwLmF1dGgwLmNvbS9hcGkvdjIvIiwiaHR0cHM6Ly9lbHluLmpwLmF1dGgwLmNvbS91c2VyaW5mbyJdLCJpYXQiOjE3NDE4NDEzMzMsImV4cCI6MTc0MTkyNzczMywic2NvcGUiOiJvcGVuaWQgcHJvZmlsZSBlbWFpbCByZWFkOmN1cnJlbnRfdXNlciB1cGRhdGU6Y3VycmVudF91c2VyX21ldGFkYXRhIGRlbGV0ZTpjdXJyZW50X3VzZXJfbWV0YWRhdGEgY3JlYXRlOmN1cnJlbnRfdXNlcl9tZXRhZGF0YSBjcmVhdGU6Y3VycmVudF91c2VyX2RldmljZV9jcmVkZW50aWFscyBkZWxldGU6Y3VycmVudF91c2VyX2RldmljZV9jcmVkZW50aWFscyB1cGRhdGU6Y3VycmVudF91c2VyX2lkZW50aXRpZXMgb2ZmbGluZV9hY2Nlc3MiLCJndHkiOlsicmVmcmVzaF90b2tlbiIsInBhc3N3b3JkIl0sImF6cCI6ImtObndYWDNGcWl0c0JCUTI3QmVOc1VVSHBUS1lLdEJGIn0.{Signature}
```

base64 decode

HEADER
```json
{
  "alg": "RS256",
  "typ": "JWT",
  "kid": "INuBRxqmvLJGBhf5-8o9l"
}
```

PAYLOAD
```json
{
  "https://live.elyn.ai/user_metadata": {
    "email": "taeho070607@gmail.com",
    "user_id": "bcc04ec1-90e3-4aeb-9fa3-5918871e3c04",
    "username": "sillo"
  },
  "iss": "https://elyn.jp.auth0.com/",
  "sub": "auth0|67d1ca492fe31fc36a7f7190",
  "aud": [
    "https://elyn.jp.auth0.com/api/v2/",
    "https://elyn.jp.auth0.com/userinfo"
  ],
  "iat": 1741841333,
  "exp": 1741927733,
  "scope": "openid profile email read:current_user update:current_user_metadata delete:current_user_metadata create:current_user_metadata create:current_user_device_credentials delete:current_user_device_credentials update:current_user_identities offline_access",
  "gty": [
    "refresh_token",
    "password"
  ],
  "azp": "kNnwXX3FqitsBBQ27BeNsUUHpTKYKtBF"
}
```

해당 jwt token 에서 user_id가 계정정보를 인식하는 핵심 key 였으며 해당 값이 타인의 userId로 변경될시 해당 피해자의 계정이 탈취되는것을 확인했습니다.

scope는 해당 token의 권한을 의미하는듯합니다.

해당 취약점은 elyn.ai 사측에 전달되었으며 패치되었습니다.

