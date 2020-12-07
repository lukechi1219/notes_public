
# notes_public

# 1

## 2

### 3

---

# Currently Reading

https://curity.io/resources/architect/api-security/split-token-pattern/

The Split Token Approach

https://curity.io/resources/architect/api-security/jwt-best-practices/#1-jwts-used-as-access-tokens

https://curity.io/resources/architect/api-security/jwt-best-practices/#11-do-not-use-jwts-for-sessions

---

https://tools.ietf.org/html/rfc8725

https://tools.ietf.org/html/rfc7518

---

https://www.c-sharpcorner.com/article/understanding-workflow-of-oauth2-0-authorization-grant-types/

https://www.c-sharpcorner.com/article/oauth2-0-and-openid-connect-oidc-core-concepts-what-why-how/

https://www.c-sharpcorner.com/article/accesstoken-vs-id-token-vs-refresh-token-what-whywhen/


https://developer.okta.com/docs/reference/api/oidc/

https://connect2id.com/learn/openid-connect


https://auth0.com/blog/refresh-tokens-what-are-they-and-when-to-use-them/

https://auth0.com/learn/refresh-tokens/

https://curity.io/resources/tutorials/howtos/flows/refresh-tokens/



https://developer.okta.com/blog/2017/07/25/oidc-primer-part-1


---

http://cryto.net/~joepie91/blog/2016/06/13/stop-using-jwt-for-sessions/

http://cryto.net/~joepie91/blog/2016/06/19/stop-using-jwt-for-sessions-part-2-why-your-solution-doesnt-work/



---

https://blog.logrocket.com/jwt-authentication-best-practices/

JWT authentication: When and how to use it<br>
Learn when JWT is best used, when it’s best to use something else, and <br>
how to prevent the most basic security issues.<br>
October 11, 2018  3 min read

Some people say you should never use it, while others say it’s amazing.
http://cryto.net/~joepie91/blog/2016/06/13/stop-using-jwt-for-sessions/ <br>
https://scotch.io/bar-talk/why-jwts-suck-as-session-tokens <br>
https://twitter.com/Ocramius/status/719949307898699776 <br>
What’s the truth? Should you use it or not? That’s why we’re here.

what's JWT: 
encoded string, which is URL safe, that can contain an unlimited amount of data (unlike a cookie), and it’s cryptographically signed

https://dev.to/siwalik/what-the-heck-is-jwt-anyway--47hg

1. JWT is a great technology for API authentication and server-to-server authorization.

1. It’s not a good choice for sessions.
1. scenario
   1. Register for API use -> admin
   1. Get secrete token from API dashboard <br>
   .
   ![](https://i0.wp.com/d2mxuefqeaa7sj.cloudfront.net/s_C4E6C0C638168A5D86467312C8359FBEED3BB8EFB36D66765F38981C5E3214B0_1537767307885_Screen+Shot+2018-09-21+at+10.44.57.png?ssl=1)
   
1. How to expire a single token
   1. The Oldest Valid Token Creat Time:
      One way to do it is to add a property to your user object in the server database, to reference the datetime the token was created at. <br>
      Compare with token.iat
   1. Another way to achieve this is by having a blacklist in your database cached in memory
   1. (or, even better, a whitelist).

1. Store JWTs securely
   1. A JWT needs to be stored in a safe place inside the user’s browser. <br>
      ?????? 
   1. Browser Local Storage X -> XSS attack
   1. Browser Session Storage X -> XSS attack
   1. The JWT needs to be stored inside an **_httpOnly_** cookie, a special kind of cookie that’s only sent in HTTP requests to the server, and it’s never accessible (both for reading or writing) from JavaScript running in the browser.

1. Using JWT to authorize operations across servers

   1. Say you have one server where you are logged in, SERVER1 (BEA), which redirects you to another server SERVER2 (PHP) to perform some kind of operation.

   1. SERVER1 can issue you a JWT that authorizes you to SERVER2. 
   1. Those two servers **_don’t need to share a session or anything_** to authenticate you. The token is perfect for this use case.

1. You don’t need to coordinate sessions in a centralized database when you get to the eventual problem of horizontal scaling
   1. 主要是不用綁死在 web01 or web02
   1. 也不需要 ignite
   1. 但是需要 Redis for black list of expires, log-out token

I recommend you read these two articles on the subject if you want to get into more details about JWTs and sessions:

“Why JWTs Suck as Session Tokens“ <br>
https://developer.okta.com/blog/2017/08/17/why-jwts-suck-as-session-tokens

“Stop using JWT for sessions“ <br>
http://cryto.net/~joepie91/blog/2016/06/13/stop-using-jwt-for-sessions/

“Stop using JWT for sessions, part 2: Why your solution doesn’t work“ <br>
http://cryto.net/~joepie91/blog/2016/06/19/stop-using-jwt-for-sessions-part-2-why-your-solution-doesnt-work/

1. Conclusion

   1. JWT is a very popular standard you can use **_to trust requests_** by using signatures, and **_exchange information_** between parties. 
   1. Make sure you know when it’s best used, when it’s best to use something else, and how to **_prevent the most basic security issues_**.


---

# Auth0

## docs

### Authentication Flows

https://auth0.com/docs/flows

#### Regular Web

https://auth0.com/docs/flows/add-login-auth-code-flow

#### Single-Page App

https://auth0.com/docs/flows/add-login-using-the-implicit-flow-with-form-post

#### Native App

https://auth0.com/docs/flows/add-login-using-the-authorization-code-flow-with-pkce

---

### Auth0 Best Practices

https://auth0.com/docs/best-practices

#### Token Best Practices

https://auth0.com/docs/best-practices/token-best-practices
































