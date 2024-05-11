# Sealed Secrets

## Retreive the public key that the controller has generated

```sh
kubeseal --fetch-cert \
--controller-name=sealed-secrets \
--controller-namespace=sealed-secrets \
> pub-cert.pem
```

## Generate and seal a kubernetes secret locally

```sh
  echo -n my-secret | kubectl create secret \
  generic mysecret --dry-run=client --from-file=foo=/dev/stdin -o json \
  | kubeseal --cert pub-cert.pem
```