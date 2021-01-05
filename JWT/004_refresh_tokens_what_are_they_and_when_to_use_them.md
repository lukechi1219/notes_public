# note

https://auth0.com/blog/refresh-tokens-what-are-they-and-when-to-use-them/

https://auth0.com/learn/refresh-tokens/

https://auth0.com/docs/tokens/refresh-tokens


Refresh Tokens: 

When to Use Them and How They Interact with JWTs
Learn about refresh tokens and how they fit in the modern web. Get a working sample of how to implement it with NodeJS

zSebastian Peyrott <br>
Software Developer <br>
Last Updated On: February 08, 2020 <br>

In this post we will explore the concept of refresh tokens as defined by OAuth2. We will learn why they came to be and how they compare to other types of tokens. We will also learn how to use them with a simple example. Read on!

Update: at the moment this article was written Auth0 had not gone through OpenID Connect certification. Some of the terms used in this article such as access token do not conform to this spec but do conform to the OAuth2 specification. OpenID Connect establishes a clear distinction between access tokens (used by resource servers to authorize or deny requests) and the id token (used by client applications to identify users).


Introduction

Modern authentication and/or authorization solutions have introduced the concept of tokens into their protocols. Tokens are specially crafted pieces of data that carry just enough information to either authorize the user to perform an action, or allow a client to get additional information about the authorization process (to then complete it). In other words, tokens are pieces of information that allow the authorization process to be performed. Whether this information is readable or parsable by the client (or any party other than the authorization server) is defined by the implementation. The important thing is: the client gets this information, and then uses it to get access to a resource. The JSON Web Token (JWT) spec defines a way in which common token information may be represented by an implementation.

A short JWT recap
JWT defines a way in which certain common information pertaining to the process of authentication/authorization may be represented. As the name implies, the data format is JSON. JWTs carry certain common fields such as subject, issuer, expiration time, etc. JWTs become really useful when combined with other specs such as JSON Web Signature (JWS) and JSON Web Encryption (JWE). Together these specs provide not only all the information usually needed for an authorization token, but also a means to validate the content of the token so that it cannot be tampered with (JWS) and a way to encrypt information so that it remains opaque to the client (JWE). The simplicity of the data format (and its other virtues) have helped JWTs become one of the most common types of tokens.

If you're interested in learning more about how to implement JWTs, click the link below and we'll email you our in-depth JWT Handbook for free!


Token types
For the purposes of this post, we will focus on the two most common types of tokens: access tokens and refresh tokens.

Access tokens carry the necessary information to access a resource directly. In other words, when a client passes an access token to a server managing a resource, that server can use the information contained in the token to decide whether the client is authorized or not. Access tokens usually have an expiration date and are short-lived.

![](https://cdn.auth0.com/blog/refresh-token/diag1.png)

Refresh tokens carry the information necessary to get a new access token. In other words, whenever an access token is required to access a specific resource, a client may use a refresh token to get a new access token issued by the authentication server. Common use cases include getting new access tokens after old ones have expired, or getting access to a new resource for the first time. Refresh tokens can also expire but are rather long-lived. Refresh tokens are usually subject to strict storage requirements to ensure they are not leaked. They can also be blacklisted by the authorization server.

![](https://cdn.auth0.com/blog/refresh-token/diag2.png)

Whether tokens are opaque or not is usually defined by the implementation. Common implementations allow for direct authorization checks against an access token. That is, when an access token is passed to a server managing a resource, the server can read the information contained in the token and decide itself whether the user is authorized or not (no checks against an authorization server are needed). This is one of the reasons tokens must be signed (using JWS, for instance). On the other hand, refresh tokens usually require a check against the authorization server. This split way of handling authorization checks allows for three things:

Improved access patterns against the authorization server (lower load, faster checks)
Shorter windows of access for leaked access tokens (these expire quickly, reducing the chance of a leaked token allowing access to a protected resource)
Sliding-sessions (see below)


Sliding-sessions
Sliding-sessions are sessions that expire after a period of inactivity. As you can imagine, this is easily implemented using access tokens and refresh tokens. When a user performs an action, a new access token is issued. If the user uses an expired access token, the session is considered inactive and a new access token is required. Whether this token can be obtained with a refresh token or a new authentication round is required is defined by the requirements of the development team.

Security considerations
Refresh tokens are long-lived. This means when a client gets a refresh token from a server, this token must be stored securely to keep it from being used by potential attackers. If a refresh token is leaked, it may be used to obtain new access tokens (and access protected resources) until it is either blacklisted or it expires (which may take a long time). Refresh tokens must be issued to a single authenticated client to prevent use of leaked tokens by other parties. Access tokens must be kept secret, but as you may imagine, security considerations are less strict due to their shorter life.

"Access tokens must be kept secret, security considerations are less strict due to their shorter life."



Use Refresh Tokens in Your Auth0 Apps
At Auth0 we do the hard part of authentication for you. Refresh tokens are not an exception. Once you have setup your app with us, follow the docs here to learn how to get a refresh token.


Conclusion

Refresh tokens improve security and allow for reduced latency and better access patterns to authorization servers. Implementations can be simple using tools such as JWT + JWS. If you are interested in learning more about tokens (and cookies), check our article here.


You can also check the Refresh Tokens landing page for more information.

