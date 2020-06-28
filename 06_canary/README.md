# 6. Canary Deployment

## 6.0 Prerequisites

Reset to default state (Istio & book-info deployed):

```
kubectl delete -f ../reset/
```
```
kubectl apply -f ../setup/
```
```
kubectl apply -f productpage-v2.yaml
```

## 6.1 30% canary to v2

Deploy [canary rules with 70/30 split](productpage-canary-70-30.yaml)

```
kubectl apply -f productpage-canary-70-30.yaml
```

Check deployment:

```
kubectl describe vs bookinfo-test

kubectl describe vs bookinfo
```

> Browse to http://bookinfo.com/productpage & refresh, mostly v1 responses with some v2

## 6.2 Canary rollout

Shift traffic to v2:

- [55/45 split](productpage-canary-55-45.yaml)
- [25/75 split](productpage-canary-25-75.yaml)
- [0/100 split](productpage-canary-0-100.yaml)

## 6.3 Canary with sticky session

Deploy [canary split with with cookie](productpage-canary-with-cookie.yaml):

```
kubectl apply -f productpage-canary-with-cookie.yaml
```

> Browse to http://bookinfo.com/productpage & refresh - once you hit v2 that puts a cookie in your browser, and you'll always get v2

Check by disabling cookies:

_e.g. in Chrome_
- Preferences
- Search "cookies"
- Site settings
- Cookies and site data -> Block
- Add `bookinfo.com`

> Browse to http://bookinfo.com/productpage & refresh - back to 70/30 split