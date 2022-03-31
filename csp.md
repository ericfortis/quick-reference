# Content Security Policy

## Usage
Instead of a header, we use a `<meta/>` tag. See `src/Views/head.ejs`.
```xml
<meta 
  http-equiv="Content-Security-Policy" 
  content="default-src 'self'; style-src 'self' 'sha256-+9+6y2OyIeRrO4dSTJt/awKsfEc/P47u6msFCxf+gFM='" />
```

## Generating hashes for inline styles

### Option A
The Chrome Dev Tools Console shows the `sha256` hash of a violation of an inline style or script. 

### Option B
```bash
echo sha256-"$(echo -n 'html{visibility:hidden}' | openssl sha256 -binary | openssl base64)"
```


## More Info
- CSP supports sha256, sha384, and sha512 hashes.

https://www.html5rocks.com/en/tutorials/security/content-security-policy/

### Modes

#### Enforce
    Content-Security-Policy: policyA; policyB
#### Report Only
    Content-Security-Policy-Report-Only: policyA; policyB

