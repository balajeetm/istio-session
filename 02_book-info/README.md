# 2. Deploying microservices managed by Istio

> Using the standard [Istio BookInfo Sample App](https://github.com/istio/istio/tree/master/samples/bookinfo) with some customizations

## 2.1 BookInfo Deployment

Deploy the microservice suite ([bookinfo-v1.yaml](./bookinfo-v1.yaml)):

```
kubectl apply -f bookinfo-v1.yaml
```

And the Istio gateway ([bookinfo-gateway.yaml](./bookinfo-gateway.yaml)):

```
kubectl apply -f bookinfo-gateway.yaml
```

## 2.2 Installation Verification

Check pods:

```
kubectl get pods
```

Check gateway:

```
kubectl get svc istio-ingressgateway -n istio-system
```

Browse the app:
> Go to http://localhost/productpage

## 2.3 Confirm pods' proxy is auto-injected

View the productpage proxy setup:

```
kubectl describe pods -l app=productpage
```

Find all proxy containers:

```
docker container ls --filter name=istio-proxy_*
```

View the proxy processes within product page:

```
docker container ls --filter name=istio-proxy_productpage* -q

docker container top $(docker container ls --filter name=istio-proxy_productpage* -q)
```
