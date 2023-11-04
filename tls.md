# TLS

## Show Certificate
https://stackoverflow.com/a/59412853/529725
```sh
show_cert () {
  local domain=$1
  openssl s_client -showcerts -servername $domain -connect $domain:443 <<< "Q" | openssl x509 -text | grep -iA2 "Validity"
}
```