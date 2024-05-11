# External DNS

```sh
echo -n vince | kubectl create secret \
  generic pihole --dry-run=client -n external-dns --from-file=pihole_password=/dev/stdin  -o yaml \
  | kubeseal --cert pub-cert.pem -o yaml
```
