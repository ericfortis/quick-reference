# **Wireshark**

Wireshark view filters are not the same as capture filters. Capture filters are tcpdump like.

## Example
Filter the GET requests containing 'foo' or 'bar' in the URI
```text
http.request.method==GET && 
  http.referer=="app:/desktop.swf" && 
  (http.request.uri contains "foo" || http.request.uri contains "bar")
```

