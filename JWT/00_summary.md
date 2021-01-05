# Summary

## JWT
- API 認證
- server-to-server 認證
- JWT is amazing: now using it also for reset password tokens

The JWT needs to be stored inside an httpOnly cookie, a special kind of cookie that’s only sent in HTTP requests to the server, and it’s never accessible (both for reading or writing) from JavaScript running in the browser.

You don’t need to coordinate sessions in a centralized database when you get to the eventual problem of "horizontal scaling"

- 主要是不用綁死在 web01 or web02
- 也不需要 ignite
- 但是需要 Redis for black list of expires, log-out token


## Redisson

https://dzone.com/articles/redis-based-tomcat-session-management

Redis-Based Tomcat Session Management



