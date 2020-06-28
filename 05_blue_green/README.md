# 5. Blue/Green Deployment

Deploy a new version of the app & switch between live & test site

## 5.0 Prerequisites

Reset to default state (Istio & book-info deployed):

```
kubectl delete -f ../reset/
```
```
kubectl apply -f ../setup/
```

## 5.1 v2 Deployment

Using existing gateway:

```
kubectl describe gateway bookinfo-gateway
```

Deploy [v2 product page with test domain](productpage-v2.yaml):

```
kubectl apply -f productpage-v2.yaml
```

Check deployment:

```

kubectl get pods -l app=productpage

kubectl describe vs bookinfo

kubectl describe vs bookinfo-test
```

Add `*.bookinfo.com` domains to hosts file:

Update /etc/hosts file

```
127.0.0.1       localhost bookinfo.com test.bookinfo.com
```

> Browse live (v1 version) - http://bookinfo.com/productpage

> Browse stealth (v2 version) - http://test.bookinfo.com/productpage

## 5.2 Toggle Blue/Green

Deploy [toggle test to live](./productpage-test-to-live.yaml)

```
kubectl apply -f productpage-test-to-live.yaml
```

Check live deployment:

```
kubectl describe vs bookinfo
```

> Live is now v2 at http://bookinfo.com/productpage

Check test deployment:

```
kubectl describe vs bookinfo-test
```

> Test is now v1 at http://test.bookinfo.com/productpage

## 5.3 Blue/green Toggle Back

Deploy [live to test switch](./productpage-live-to-test.yaml)

```
kubectl apply -f productpage-live-to-test.yaml
```

> Live is back to v1 http://bookinfo.com/productpage

> Test is back to v2 http://test.bookinfo.com/productpage
