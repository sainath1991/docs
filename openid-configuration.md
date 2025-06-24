- download and install any of the drupal module
```
https://www.drupal.org/project/openid_connect

https://www.drupal.org/project/oidc
```
- openid uses oauth2 to get the user information.
Note: below clien id and screts are tests data. will not work actually.
```
https://authorization-server.com/authorize?
  response_type=code
  &client_id=EYhQ-ekIxEVBZckPQHgeiFEl
  &redirect_uri=https://www.oauth.com/playground/oidc.html
  &scope=openid+profile+email+photos
  &state=oocwjihSw1rv9UDe
  &nonce=uzINmyEU1KWJj9Q6
```
- redirect_uri will get below query parameters
```
?state=oocwjihSw1rv9UDe&code=XwaiyebtehayY3fDnV0K0a2gMYp-eIuwQVnLtv0BKrkT3awk
```

- finally post call to get the token
```
POST https://authorization-server.com/token

grant_type=authorization_code
&client_id=EYhQ-ekIxEVBZckPQHgeiFEl
&client_secret=xCAouQ9PuzngrYhjb-3iow9rlGyMV264f2YAvDvwpxI0I7YJ
&redirect_uri=https://www.oauth.com/playground/oidc.html
&code=XwaiyebtehayY3fDnV0K0a2gMYp-eIuwQVnLtv0BKrkT3awk
```

Normally user info endpoint will be
```
GET https://authorization-server.com/userinfo
```
