# **Headers readable from JS in the browser**
- document.contentType
- document.cookie (as long as the cookie header doesn't have the HttpOnly attr)
- document.referrer
- document.userAgent
- Not sure if there are more

## Alternative A: Make a request from js
You or an attacker could create a request and get them. i.e.
```js
var req = new XMLHttpRequest()
req.open('HEAD', 'https://blog.uidrafter.com', false)
req.send(null)
console.log(req.getAllResponseHeaders())
```

```output
cache-control: no-cache
content-encoding: br
content-length: 11452
content-type: text/html; charset=utf-8
date: Sun, 03 Apr 2022 02:45:54 GMT
last-modified: Sat, 02 Apr 2022 15:58:23 GMT
permissions-policy: accelerometer=(), camera=(), geolocation=(), gyroscope=(), magnetometer=(), microphone=(), payment=(), usb=()
referrer-policy: strict-origin-when-cross-origin
server: nginx
strict-transport-security: max-age=31536000; includeSubDomains; preload
x-content-type-options: nosniff
x-frame-options: deny
```

another example, go to www.googlec.com and paste
```shell
var req = new XMLHttpRequest()
req.open('HEAD', 'https://www.google.com', false)
req.send(null)
console.log(req.getAllResponseHeaders())
```
```shell 
VM338:4 accept-ch: Sec-CH-UA-Platform, Sec-CH-UA-Platform-Version, Sec-CH-UA-Full-Version, Sec-CH-UA-Arch, Sec-CH-UA-Model, Sec-CH-UA-Bitness
alt-svc: h3=":443"; ma=2592000,h3-29=":443"; ma=2592000,h3-Q050=":443"; ma=2592000,h3-Q046=":443"; ma=2592000,h3-Q043=":443"; ma=2592000,quic=":443"; ma=2592000; v="46,43"
bfcache-opt-in: unload
cache-control: private
content-type: text/html; charset=UTF-8
date: Sun, 03 Apr 2022 02:49:31 GMT
expires: Sun, 03 Apr 2022 02:49:31 GMT
server: gws
strict-transport-security: max-age=31536000
x-frame-options: SAMEORIGIN
x-xss-protection: 0
```



## Alternative B: Use Service Workers
https://github.com/gmetais/sw-get-headers
is a Service Worker that reads network headers and sends them back to the page

