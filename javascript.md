# **Headers readable from JS in the browser**
- document.contentType
- document.cookie (as long as the cookie header doesn't have the HttpOnly attr)
- document.location
- document.referrer
- document.userAgent
- Not sure if there are more

## Alternative A: Make a request from js
You or an attacker could create a request and get them. i.e.
```js
var req = new XMLHttpRequest()
req.open('HEAD', 'http://site.com', false)
req.send(null)
console.log(req.getAllResponseHeaders())
```

## Alternative B: Use Service Workers
https://github.com/gmetais/sw-get-headers
is a Service Worker that reads network headers and sends them back to the page

