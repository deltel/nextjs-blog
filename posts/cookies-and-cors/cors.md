---
title: "CORS"
date: "2023-09-10"
---

So what is CORS?

**Cross-Origin Resource Sharing (CORS)** is a means by which a server indicates which origins apart from its own may request data from it.

A CORS Request will always contain the **Origin** header

A preflight request is sent first to find out if the server will permit the actual request. In this preflight, the method and headers to be used in the actual request are sent along.

![preflight request headers](/images/cookies-and-cors/preflight-request-headers.png)

The fetch api follows the **same-origin** policy meaning that any requests made to a different domain than the one the request was sent from will be blocked unless the response has the appropriate **CORS** headers.

The server can also specify whether or not credentials such as cookies or http authentication should be sent with each request.

For our case we will be using a credentialled request using cookies. We therefore have to specify the **Access-Control-Allow-Origin** instead of the "**\***" wildcard. Another reason for our requests to be preflighted is the addition of the custom headers being sent. That is the **x-csrf-token** header in our case.

Important points:

- The browser will not set the cookie specified by **Set-Cookie** Response header if the **Accesss-Control-Allow-Origin** header is the "\*" wildcard.

The important headers are:

- **Access-Control-Allow-Origin** - specifies which domains have access to load the resource
- **Access-Control-Expose-Headers** - specifies which response headers can be programmatically accessed by JavaScript
- **Access-Control-Allow-Credentials** - specifies whether or not the response is accessible when the **credentials** flag is set to true
- **Access-Control-Allow-Methods** - specifies which HTTP methods are allowed when accessing the resource.

As this is an **express** application we will be doing this using the **cors** npm package.
We can enable cors app wide or on a request basis, for simplicity we'll do app wide.
The cors middleware takes an object which can be used to specify the options available for the cors configuration.

```
cors({
  origin: "http://localhost:3000",
  exposedHeaders: ["x-csrf-token"],
  credentials: true,
  methods: ["GET", "POST"]
})
```

From the front-end we have to specify the fetch request with credentials:

```
fetch(
    "fake.fake.io",
    { credentials: "include" }
).then(/* do whatever */)
```

In the case of configuring CORS on a request basis:

```
app.get(
    "/v1/protected",
    cors({
        origin: "http://localhost:3000",
        credentials: true,
        methods: ["GET"],
    }),
    (req, res) => { /* do whatever */ }
);
```

You'll have to ensure you respond to preflight requests appropriately as is shown below. Preflight requests are sent with the options headers. So we call **app.options** and specify the url as well as the response headers for the preflight request. Again, this is done using the cors middleware.

```
app.options(
    "/v1/protected",
    cors({
        origin: "http://localhost:3000",
        credentials: true,
        methods: ["GET"],
    })
);
```

In order for cookies to function properly, the fetch request must be made with credentials. Otherwise the **Set-Cookie** header will be returned, but the cookie itself will be ignored by the browser.
