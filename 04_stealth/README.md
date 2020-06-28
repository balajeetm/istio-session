# 4. Stealth Launch

Deploy new version of a microservice (reviews) exposed only to test users.

## 4.0 Prerequisites

Reset to default state (Istio & book-info deployed):

```
kubectl delete -f ../reset/
```
```
kubectl apply -f ../setup/
```

## 4.1 v2 Deployment

Deploy [v2 reviews service](./reviews-v2.yaml):

```
kubectl apply -f reviews-v2.yaml
```

Check deployment:

```
kubectl get pods -l app=reviews

kubectl describe svc reviews
```

> Browse to http://localhost/productpage and refresh, requests load-balanced between v1 and v2

## 4.2 Make a stealth launch

Deploy [test user routing rules](./reviews-v2-tester.yaml):

```
kubectl apply -f reviews-v2-tester.yaml

kubectl describe vs reviews
```

> Browse to http://localhost/productpage - all users see v1 except `testuser` who sees v2


## 4.3 Introduce network delays

Deploy [delay test rules](./reviews-v2-tester-delay.yaml):

```
kubectl apply -f reviews-v2-tester-delay.yaml
```

> Browse to http://localhost/productpage - `testuser` gets delayed response, all others OK

## 4.4 Introduce service faults

Deploy [service unavailable error rules](./reviews-v2-tester-503.yaml)

```
kubectl apply -f reviews-v2-tester-503.yaml
```

> Browse to http://localhost/productpage -  `testuser` gets 50% failures, all others OK