# 9.0 - Access Control & Authorization

Enforce authorisation based on the caller's mTLS identity.

## 9.0 Prerequisites

Reset to default state (Istio & book-info deployed):

```
kubectl delete -f ../reset/
```
```
kubectl apply -f ../setup/
```

## 9.1 Restrict access to reviews

Apply a [deny-all authorization policy](reviews-deny-all.yaml) for the service:

```
kubectl apply -f reviews-deny-all.yaml
```

> Check http://localhost/productpage

## 9.2 Allow access from product page

Apply the [updated authorization policy](reviews-allow-productpage.yaml) to allow service access:

```
kubectl delete -f reviews-deny-all.yaml
```
> Check http://localhost/productpage

```
kubectl apply -f reviews-allow-productpage.yaml
```

> And check http://localhost/productpage

## 9.3 Verify unauthorized access

Exec into details API container:

```
docker container ls --filter name=k8s_details

docker container exec -it $(docker container ls --filter name=k8s_details --format '{{ .ID}}') sh
```

Install Curl:

```
apt-get update

apt install curl
```

Try accessing the reviews & ratings APIs:

```
curl http://reviews:9080/1

curl http://ratings:9080/ratings/1
```

## 9.4 View the TLS client certs

Run a shell in the product page container:

```
docker container exec -it $(docker container ls --filter name=istio-proxy_productpage --format '{{ .ID}}') sh
```

Check the identity in the cert:

```
cat /etc/certs/cert-chain.pem | openssl x509 -text -noout  | grep 'Subject Alternative Name' -A 1
```

> URI is the principal in AuthorizationPolicy (using [SPIFFE](https://spiffe.io))

> SPIFFE - Secure Production Identity Framework for Everyone

And in the details API container:

```
docker container exec -it $(docker container ls --filter name=istio-proxy_details --format '{{ .ID}}') sh

cat /etc/certs/cert-chain.pem | openssl x509 -text -noout  | grep 'Subject Alternative Name' -A 1
```

> Principal not listed in AuthorizationPolicy

