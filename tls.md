# TLS

## Show Certificate
https://stackoverflow.com/a/59412853/529725
```sh
DN=example.com
openssl s_client -showcerts -servername $DN -connect $DN:443 <<< "Q" | openssl x509 -text | grep -iA2 "Validity"
```