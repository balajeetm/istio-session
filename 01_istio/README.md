# 1. Installing Istio

> For details refer [Istio Quick Start Docs](https://istio.io/latest/docs/setup/getting-started/)

## 1.1 Istio Deployment

Install CRDs (custom resource definitions) ([istio-crds.yaml](./istio-crds.yaml)):

```
kubectl apply -f istio-crds.yaml
```

Install istio in demo configuration ([istio-demo.yaml](./istio-demo.yaml)):

```
kubectl apply -f istio-demo.yaml
```

## 1.2 Istio Installation Verification

Check all resources running:

```
kubectl get all -n istio-system
```

> Components run as docker conatiner & consume memory

## 1.3 Auto proxy injection configuration

Configure default namespance:

```
kubectl label namespace default istio-injection=enabled
```

Check label of the namespace:

```
kubectl describe namespace default
```

## 1.4 Check the namespace

```
kubectl get all

docker info
```
