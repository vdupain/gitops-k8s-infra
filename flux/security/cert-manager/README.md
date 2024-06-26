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
echo -n ${GANDI_LIVEDNS_KEY} | kubectl create secret --namespace cert-manager \
  generic gandi-credentials --dry-run=client --from-file=api-token=/dev/stdin -o yaml \
  | kubeseal --cert pub-cert.pem -o yaml > gandi-credentials.sealedsecret.yaml
```
