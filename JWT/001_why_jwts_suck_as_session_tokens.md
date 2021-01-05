note

https://scotch.io/bar-talk/why-jwts-suck-as-session-tokens

Why JWTs Suck as Session Tokens - by rdegges <br>
July 11, 2017

JSON Web Tokens (JWTs) are so hot right now. They’re all the rage in web development:

- Trendy? ✓
- Secure? ✓
- Scalable? ✓
- Compact? ✓
- JSON? ✓

## How are People Using JWTs Today?

The idea is that when someone authenticates to a website/API, the server will generate a JWT that contains the user’s ID, as well as some other critical information, and then send it to the browser/API/etc. to store as a session token.

![](https://scotch-res.cloudinary.com/image/upload/dpr_1,w_800,q_auto:good,f_auto/media/36632/6Bvu9yhRQKQjxgCLj6nS_jwt-session.png)

When that user visits another page on the website, for instance, their browser will automatically send that JWT to the server, which will validate the JWT to make sure that it’s the same token it created originally, then let the user do stuff.

In theory, this sounds nice because:

- When the server receives a JWT, it can validate that it is legitimate and trust that the user is whoever the token says they are
- The server can **`validate this token locally`** without making any network requests, talking to a database, etc. This can potentially make `session management faster` because instead of needing to load the user from a database (or cache) on every request, you just need to run a small bit of local code. This is probably the single biggest reason people like using JWTs: they are **stateless**.

These two perks sound great because they will speed up webapp performance, reduce load on cache servers and database servers, and generally provide faster experiences.

As a bonus benefit, as the webapp creator you can embed other information about the user into your JWT:

User permissions
User personal information
Etc.
This means that you can reduce your database load even further by simply embedding extra user information in your tokens as well!


## Why Do JWTs Suck?

why JWTs are not good session tokens.

### Context

Before I start making web developers all over the world angry, I want to provide some context into my reasoning.

Most websites that developers build are relatively simple:

- A user registers for the website
- A user signs into the website
- A user clicks around and does stuff
- The website uses the user’s information to create, update, and delete information

99.9% of all websites match the criteria above.

For these types of websites, what’s important to know is that almost every page a user interacts with contains some sort of dynamic data. Odds are, if you’re running a website that requires a user to sign in to use it, you’re going to be doing things with that user in your database often:

- Recording the actions a user is taking
- Adding some data for the user to the database
- Checking a user’s permissions to see if they can do something
Etc.

The important thing to remember is that most sites require user information for nearly every operation.

With that out of the way, let’s get into the reasons why JWTs suck. First up? Size.

### Size (x)


`{
"iss": "https://api.mywebsite.com",
"sub": "abc123",
"nbf": 1497934977,
"exp": 1497938577,
"iat": 1497934977,
"jti": "1234567",
"typ": "authtoken"
}
`
### You’re Going to Hit the Database Anyway

![](https://scotch-res.cloudinary.com/image/upload/dpr_1,w_800,q_auto:good,f_auto/media/36632/g3uN84CSPecKxBAt2ATc_youll-hit-the-db-anyway.png)

The issue with using JWTs on these websites is that for almost every single page the user loads, the user object needs to be loaded from a cache / database because one of the following situations are occurring:

- A mission critical user check needs to run (eg: `does this user have enough money` in their account to complete the transaction?)
- A database write needs to occur to persist information (if this information is related to the user, it’s likely that the full user object must also be retrieved from the database)
- The full user object must be pulled out of the cache / database so that the website can properly generate its dynamic page content

Think about the websites you build. Do they often manipulate user data? Do they frequently use various fields on the user account to work? If so, your site falls into this category, and you’ll likely be talking to the cache server / database regardless of whether or not you’ve got a JWT.

`This means that on most websites, the stateless benefits of a JWT are not being taken advantage of.`

To compound this issue, since JWTs are larger (in bytes) and also `require CPU to compute cryptographic signatures`, they’re actually significantly slower than traditional sessions when used in this manner.

Almost every web framework loads the user on every incoming request. This includes frameworks like Django, Rails, Express.js (if you’re using an authentication library), etc. This means that even for sites that are primarily stateless, the web framework you’re using is still loading the user object regardless.

Finally: if you’re storing your user information in a modern cache like memcached/redis, it’s not uncommon `over a VPC ???` to achieve cache GET times of 5ms or below, which is extremely fast. I’ve personally used DynamoDB on Amazon in the past as a session store, and consistently achieved 1ms cache retrieval times. Because caching systems are so fast, there’s very little performance overhead when retrieving users in this manner.

### Redundant Signing

One of the main selling points of JWTs are cryptographic signatures. Because JWTs are cryptographically signed, a receiving party can verify that the JWT is valid, and trusted.

But… what would you say if I told you that in pretty much every single web framework created over the last 20 years, you could also get the benefits of cryptographic signatures when using plain old session cookies?

Well, you can =)

Most web frameworks cryptographically sign (and many encrypt!) your cookies for you automatically. This means that you get the exact same benefits as using JWT signatures without using JWTs themselves.

In fact, in most web authentication cases, the JWT data is stored in a session cookie anyways, meaning that there are now two levels of signing. One on the cookie itself, and one on the JWT.

While having two levels of signing may sound like a good idea, it is not. You get no security benefits, and you’ve now got to spend twice as long on CPU cycles to validate both signatures. Not really ideal for web environments where milliseconds are important. This is especially true in single threaded environments (cough cough nodejs) where number crunching can block your main event loop.



## What’s a Better Solution?

If JWTs suck, then what’s a better solution? Plain old sessions!

If your website is popular and has many users, cache your sessions in a backend like memcached or redis, and you can easily scale your service with very little hassle.




## How Should I Use JWTs?

It’s important to note that I don’t hate JWTs. I just think they’re useless for a majority of websites. With that said, however, there are several cases in which JWTs can be useful.

If you’re building API services that need to support `server-to-server` or `client-to-server` (like a mobile app or single page app (SPA)) communication, using JWTs as your API tokens is a very smart idea. In this scenario:

- You will have an authentication API which clients authenticate against, and get back a JWT
- Clients then use this JWT to send authenticated requests `to other API services`
- These other API services use the client’s JWT to validate the client is trusted and can perform some action without needing to perform a network validation

![](https://scotch-res.cloudinary.com/image/upload/dpr_1,w_800,q_auto:good,f_auto/media/36632/HQDEsxKPRX23pblvod3S_good-jwt-example.png)

For these types of API services, JWTs make perfect sense because clients will be making requests frequently, with limited scope, and usually authentication data can be persisted in a stateless way without too much dependence on user data.

If you’re building any type of service where you need three or more parties involved in a request, JWTs can also be useful. In this case the requesting party will have a token to prove their identity, and can forward it to the third (or 4th ... nth) service without needing to incur a real-time validation each and every time.

Finally: if you’re using user federation (things like single sign-on and OpenID Connect), JWTs become important because you need a way to validate a user’s identity via a third party. Thanks to cryptographic signing, JWTs make a valuable addition to federated user protocols.


## Wrap Up

When you start building your next website, just rely on your web framework’s default authentication libraries and tools, and stop trying to shove JWTs into the mix unnecessarily.









