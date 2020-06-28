# 7. Circuit Breaker

Add outlier detection to a load-balanced servcie so unhealthy endpoints are removed.

## 7.0 Prerequisites

Reset to default state (Istio & book-info deployed):

```
kubectl delete -f ../reset/
```
```
kubectl apply -f ../setup/
```

## 7.1 Details v2 deployment

Deploy [service update with 4 replicas](details-v2.yaml):

```
kubectl apply -f details-v2.yaml
```

Verify deployment:

```
kubectl describe service details

kubectl describe vs details

kubectl describe dr details
```

Check logs:

```
kubectl logs -l app=details,version=v2 -c details | grep "Running with status"
```

> Browse http://localhost/productpage, refresh & confirm call failures.

Check logs:

```
kubectl logs -f -l app=details,version=v2 -c details
```

## 7.2 Circuit breaker

Deploy [updated rules with outlier detection](details-circuit-breaker.yaml):

```
kubectl apply -f details-circuit-breaker.yaml
```

Veirfy deployment:

```
kubectl describe dr details
```

> Browse http://localhost/productpage & confirm that pods that return errors get excluded - after a while there are no errors, requests only go to healthy pods.
