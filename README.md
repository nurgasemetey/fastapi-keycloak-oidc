# fastapi-keycloak-oidc

This is example of how to use Keycloak on Fastapi. 

It is useful for those who want to use JWT tokens and don't want to use Keycloak SDK in the backend. 

## Prerequisites

**Run keycloak**

`docker run -p 8080:8080 -e KEYCLOAK_USER=admin -e KEYCLOAK_PASSWORD=admin jboss/keycloak`

**Create client on Keycloak**

We are creating client for testing and getting JWT tokens.

1. Go to `http://localhost:8080/auth/`
2. Login with `admin/admin`
3. Go to `Clients` section on sidebar
4. Click `Create` button
5. Fill `Client ID` as `login-app`
6. Click `Save` button

**Install dependencies**

`pip install -r requirements.txt`


## Testing

**Run python code**

```bash
python starter.py`
```

**Run curl on secured endpoint**

```bash
curl localhost:8000/users/me/items/
```

which will gives following error

```
{"detail":"Not authenticated"}
```


**Get token from Keycloak**

```bash
curl --request POST 'http://localhost:8080/auth/realms/master/protocol/openid-connect/token' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'client_id=login-app' \
--data-urlencode 'username=admin' \
--data-urlencode 'password=admin' \
--data-urlencode 'grant_type=password'
```

which will give following

```json
{
  "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJQUEZ2V3dSQy1RTjNFaFlqWlQ4NFAzeTEwRU5fWC1CTnE2NzI3MWdsZ21RIn0.eyJqdGkiOiI2NTM1MWQ4Yy1hNzg3LTRlYmMtYmFhYi0xOTQxNmFkN2Y4YmIiLCJleHAiOjE2MTg5OTMwMTcsIm5iZiI6MCwiaWF0IjoxNjE4OTkyOTU3LCJpc3MiOiJodHRwOi8vbG9jYWxob3N0OjgwODAvYXV0aC9yZWFsbXMvbWFzdGVyIiwiYXVkIjoiYWNjb3VudCIsInN1YiI6IjljNWYxZDM1LWNhZGItNGVlNS04MmZkLWY0NGU3MThmODIxNCIsInR5cCI6IkJlYXJlciIsImF6cCI6ImxvZ2luLWFwcCIsImF1dGhfdGltZSI6MCwic2Vzc2lvbl9zdGF0ZSI6IjBiOTM2Zjc4LTkzY2QtNDAzMC05MWFkLTJhY2E5NTJhMWE3YiIsImFjciI6IjEiLCJyZWFsbV9hY2Nlc3MiOnsicm9sZXMiOlsib2ZmbGluZV9hY2Nlc3MiLCJ1bWFfYXV0aG9yaXphdGlvbiIsInN5c3RlbV91c2VyIl19LCJyZXNvdXJjZV9hY2Nlc3MiOnsiYWNjb3VudCI6eyJyb2xlcyI6WyJtYW5hZ2UtYWNjb3VudCIsIm1hbmFnZS1hY2NvdW50LWxpbmtzIiwidmlldy1wcm9maWxlIl19fSwic2NvcGUiOiJlbWFpbCBwcm9maWxlIiwiZW1haWxfdmVyaWZpZWQiOmZhbHNlLCJwcmVmZXJyZWRfdXNlcm5hbWUiOiJzaW1wbGVfdXNlciJ9.0e6DiKHrLSFcmd9HCDu7E1g8o19hR3xc6IzMViO06mp1h5C2U7HjJDAWlUN2dk7VilWolze9i_w9r8ggyZ7FvmPRZ-v8gohDMJLS2xgt0GmvKEjcH2Kh2GMtaGXrG4y4AkAYytqDCO7p04pHPZT_tBsReO3WH4EAtsAcG4NrCPvtkkPGj7SFuzQQm9boEV9dsTX4UXeebabUQGr_KAhpcRyAtqPhgKqsmmupsUeWfVdPz1MiyD0DMcLaI4BOCMTUQA_lMjZDdu0GifhAiWfU_1ewDXcMlOYpIFoB_F-6101QKbSDswzrw4cciHIQt2Le1uZhm0UE6MNU2iIOy6a5RQ",
  "expires_in": 60,
  "refresh_expires_in": 1800,
  "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICI5ZjJjZmI3Zi00MThkLTRkMTUtYWM2OC1kM2E4YmQ0NDRlZWEifQ.eyJqdGkiOiI1OTRmYTc2NC0yZjFlLTRmY2ItOTU1YS0yMjcwNWY4NDcyZjUiLCJleHAiOjE2MTg5OTQ3NTcsIm5iZiI6MCwiaWF0IjoxNjE4OTkyOTU3LCJpc3MiOiJodHRwOi8vbG9jYWxob3N0OjgwODAvYXV0aC9yZWFsbXMvbWFzdGVyIiwiYXVkIjoiaHR0cDovL2xvY2FsaG9zdDo4MDgwL2F1dGgvcmVhbG1zL21hc3RlciIsInN1YiI6IjljNWYxZDM1LWNhZGItNGVlNS04MmZkLWY0NGU3MThmODIxNCIsInR5cCI6IlJlZnJlc2giLCJhenAiOiJsb2dpbi1hcHAiLCJhdXRoX3RpbWUiOjAsInNlc3Npb25fc3RhdGUiOiIwYjkzNmY3OC05M2NkLTQwMzAtOTFhZC0yYWNhOTUyYTFhN2IiLCJyZWFsbV9hY2Nlc3MiOnsicm9sZXMiOlsib2ZmbGluZV9hY2Nlc3MiLCJ1bWFfYXV0aG9yaXphdGlvbiIsInN5c3RlbV91c2VyIl19LCJyZXNvdXJjZV9hY2Nlc3MiOnsiYWNjb3VudCI6eyJyb2xlcyI6WyJtYW5hZ2UtYWNjb3VudCIsIm1hbmFnZS1hY2NvdW50LWxpbmtzIiwidmlldy1wcm9maWxlIl19fSwic2NvcGUiOiJlbWFpbCBwcm9maWxlIn0.e8ONRUVnaBbTASqIbpr5iqaYEjFY-XTI-zI_59Gun84",
  "token_type": "bearer",
  "not-before-policy": 0,
  "session_state": "0b936f78-93cd-4030-91ad-2aca952a1a7b",
  "scope": "email profile"
}
```

**Run curl again on secured endpoint but with token now**

```bash
curl localhost:8000/users/me/items/ --header 'Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJQUEZ2V3dSQy1RTjNFaFlqWlQ4NFAzeTEwRU5fWC1CTnE2NzI3MWdsZ21RIn0.eyJqdGkiOiJmYTQ1NGY3ZC1lNWJjLTQ4ZTQtYWI3My1hOGRmNDc5ODM2NGEiLCJleHAiOjE2MTg5OTMxMDksIm5iZiI6MCwiaWF0IjoxNjE4OTkzMDQ5LCJpc3MiOiJodHRwOi8vbG9jYWxob3N0OjgwODAvYXV0aC9yZWFsbXMvbWFzdGVyIiwiYXVkIjoiYWNjb3VudCIsInN1YiI6IjljNWYxZDM1LWNhZGItNGVlNS04MmZkLWY0NGU3MThmODIxNCIsInR5cCI6IkJlYXJlciIsImF6cCI6ImxvZ2luLWFwcCIsImF1dGhfdGltZSI6MCwic2Vzc2lvbl9zdGF0ZSI6ImVjYmNiNjZlLTRlODUtNDFhOS04NTIyLWNiNWU0ZjBiZDQwZSIsImFjciI6IjEiLCJyZWFsbV9hY2Nlc3MiOnsicm9sZXMiOlsib2ZmbGluZV9hY2Nlc3MiLCJ1bWFfYXV0aG9yaXphdGlvbiIsInN5c3RlbV91c2VyIl19LCJyZXNvdXJjZV9hY2Nlc3MiOnsiYWNjb3VudCI6eyJyb2xlcyI6WyJtYW5hZ2UtYWNjb3VudCIsIm1hbmFnZS1hY2NvdW50LWxpbmtzIiwidmlldy1wcm9maWxlIl19fSwic2NvcGUiOiJlbWFpbCBwcm9maWxlIiwiZW1haWxfdmVyaWZpZWQiOmZhbHNlLCJwcmVmZXJyZWRfdXNlcm5hbWUiOiJzaW1wbGVfdXNlciJ9.XraITtlc8QwesbSi6YjqindLWY7NtPj-_VTHdhuCLAdmUIbW03G2A8C-23GMVdvVS2X7ZnZiW2I7nZ3jRx5EKt9yVxm23gKq0JSL3520y30a-5iHWXy6Ld52aym8lAwKvBP0KODf4yEF1C55iYK6fOyna5Wn3ytdHnO3HPOVzUREsnTsC4q3J2TnGDZR3DQ8IgsEOwADqzwFxmSAdUag6W20eGo_Y9ljbWVffwwJJHJvNdCM3__7QfgmmG4vv8SQypebjmX3FUqUTCu8y4Z9ED2Qox-b_CLlxhHLjQIwwWgOzfiVEKjcLJm6f2qXmwvJzrfguWV2Bxi0VF-e-RDITg'
```

which will give following

```json
[{"item_id":"Foo","owner":"admin"}]
```

## Notes

- If you look at code at `main.py`, you will see that we didn't use  Keycloak SDK
- On start it fetches public key from `http://localhost:8080/auth/realms/master` which gives us following 

```json
{
  "realm": "master",
  "public_key": "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA8PtFbgQHNhn8WrgOfVVb/a1qV87vR7R2t0jO5fAJqKq24+pVrYw7D38Qh/O2HjWPVrPRzEBiUE61poUqEJfVjclSqJoqXLhTMOm1NRbpfzf1Nid+uZfJ4B5JtA/yNLpY13s9+4F77FjCmA0fmNAnaxRJ26a7bFYacl7+rcZqPD9zl+FKfta5vKw5onwz0aTVQLgYZ4Ysmehr+f6z4cKgY2+z7IpQOELXFyWktWPiOtkL/Q6mBPr/mHqhQ7ARmlOM6DC5Babb7y4H3U4fRIO9ByPAMQMTtjvmL9NlrqSw+51s6GPGqtlJegi2jW4vIOSOOOHJH0OhTDWOyQHrp3XRIQIDAQAB",
  "token-service": "http://localhost:8080/auth/realms/master/protocol/openid-connect",
  "account-service": "http://localhost:8080/auth/realms/master/account",
  "tokens-not-before": 0
}
```
`public_key` is used for verification of JWT tokens.
