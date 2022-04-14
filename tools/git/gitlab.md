## 错误

### /etc/ssl/certs/ca-certificates.crt CRLfile: none
解决方法:

```
echo -n | openssl s_client -showcerts -connect gitlab.ubt.com:443 \
  2>/dev/null  | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p'
```