---
title: "HTTP Cookies"
date: "2023-09-10"
---

A cookie is a small piece of data that a server sends to a user's browser - the client. The browser then sends this data on each subsequent request to the server.

A server sets cookies using the **Set-Cookie** HTTP response header. This can be a single header or multiple headers. Once a server sends a cookie using the **Set-Cookie** header, the browser will automatically send the cookie back via the **Cookie** HTTP request header.

To set a response header using express we use the setHeader method on the response object. It takes 2 arguments, the first being the header name and the second being the value.

To set a cookie with the value jwt it would be

```
res.setHeader(
  "Set-Cookie",
  "jwt-value"
);
```

![Set-Cookie response header](/images/cookies-and-cors/set-cookie-header.png)

The corresponding request headers that will always be sent are:
![Set-Cookie response header](/images/cookies-and-cors/cookie-header.png)

A cookie can be categorized in 2 ways as it relates to its lifetime. It can either be a session cookie or a permanent cookie. A session cookie is deleted when the current session ends. This is usually determined by the browser. A permanent cookie is one that has a set expiry date.

The focus of this article will be on permanent cookies. One can set the expiry date on these permanent cookies using 2 attributes:

- **Expires**
- **Max-Age**

If both are specified the **Max-Age** attribute takes precedence. The **Expires** attribute sets the date and time at which the cookie will expire. This is relative to the client not the server. The **Max-Age** attribute on the otherhand sets how long the cookie will last in seconds.

```
res.setHeader(
  "Set-Cookie",
  "jwt-value; Expires=Thu, 31 Oct 2025 07:28:00 GMT; Max-Age=216000"
)
```

The above sets a cookie with a Max-Age of 1 hour and as both Expires and Max-Age is set, the Max-Age takes precedence.

In addition, in order to restrict access to the cookie, the **Secure** and **HttpOnly** flags must be set. Cookies with the Secure attribute are only sent over the HTTPS protocol, with the exception being localhost. This helps to mitigate man-in-the-middle attacks. However they can still be accessed using JavaScript unless the **HttpOnly** flag is set. This flag prevents the cookie from being able to be accessed through the _Document.cookie_ API. It is only sent to the server. This helps to prevent **Cross-Site Scripting (XSS)** attacks. It is however worth noting that cookies are vulnerable to **Cross-Site Request Forgery (CSRF)** attacks.

```
res.setHeader(
  "Set-Cookie",
  "jwt-value; Max-Age=216000; Secure; HttpOnly"
)
```

There are additional properties worth mentioning. They include:

- **Domain** - this specifies which domains can access a cookie. If this is not set then only requests originating from the same domain as the server will receive the cookie. If specified, it will also include sub-domains in the list of allowed domains.
- **SameSite** - this specifies the context in which cookies are set. There are 3 options:
  - Strict - cookies are only sent on requests originating from the same domain
  - Lax (default) - allows cookies to be also sent when navigating to the issuer site from a third-party site
  - None - cookies are sent with all requests whether or not they are from the issuer's domain
- **Path** - this attribute specifies a url path that must exist in the request url in order for the cookie to be sent.

Now all this works; when deployed the APIs can be accessed through postman, but should you try to spin up a UI running on a different port we suddenly have to deal with CORS. These CORS issues are for both the local instance of the server and the live instance of the server. This simulates what would happen if a third-party site attempts to make requests to the endpoints defined on the server.
