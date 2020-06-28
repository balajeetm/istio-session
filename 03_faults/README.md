# 3. Fault-tolerance with Istio

> Simple fault tolerance with Istio to show control over communication.

## 3.1 Change SVC routing to Virtual Service routing

Deploy [details-virtualservice.yaml](./details-virtualservice.yaml)

```
kubectl apply -f details-virtualservice.yaml
```

> Browse to http://localhost/productpage (same functionality)

## 3.2 Deploy a faulty release

A Faulty release - [details-faulty-release.yaml](./details-faulty-release.yaml)

```
kubectl apply -f details-faulty-release.yaml
```

Observing the logs:

```
kubectl logs -f -l app=details -c details
```

> Browse to http://localhost/productpage - details call times out after 15 seconds

## 3.3 Update timeout on Virtual Service

- [details-virtualservice-timeout.yaml](./details-virtualservice-timeout.yaml)

```
kubectl apply -f details-virtualservice-timeout.yaml
```

View Virtual Service Details
```
kubectl describe virtualservice details
```

> Browse to http://localhost/productpage - details call times out after 3 seconds

## 3.4 Update retry on Virtual Service

- [details-virtualservice-retry.yaml](./details-virtualservice-retry.yaml)

```
kubectl apply -f details-virtualservice-retry.yaml
```

> Browse to http://localhost/productpage - details call times out and then automatically retries