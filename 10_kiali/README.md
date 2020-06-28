# 10 - Visualizing the Service Mesh

Using [Kiali](https://kiali.io) to graph active services and see the traffic flow between them.

## 10.0 Prerequisites

Reset to default state (Istio & book-info deployed):

```
kubectl delete -f ../reset/
```
```
kubectl apply -f ../vis-setup/
```

> There are some new ports in the [Istio ingress gateway](../vis-setup/02_istio-demo.yaml).

## 10.1 Open up the Kiali UI

Deploy a [Gateway and VirtualService](kiali.yaml) for Kiali:

```
kubectl apply -f kiali.yaml
```

Hit product page a few times
> http://localhost/productpage

> Browse to http://localhost:7189, sign in with `admin`/`admin`

- App graph in _Graph_
- Check `productpage` in Kiali _Workloads_

## 10.2 Canary deployment for v2

Deploy [v2 product page](./productpage-v2-canary.yaml) and [v2 reviews API](./reviews-v2-canary.yaml) updates:

```
kubectl apply -f ./productpage-v2-canary.yaml
kubectl apply -f ./reviews-v2-canary.yaml
```

> Browse to http://bookinfo.com/productpage and refresh 

- Back to Kiali
- Switch versioned app graph
- Add _Requests percentage_ label
- Check bookinfo virtual service in _Istio Config_ (editable!)

## 10.3 Generate some load

Add the ip address for bookinfo.com in `/etc/hosts` file
```
ifconfig | grep inet
```
In `/etc/hosts` 
```
192.xxx.xxx.xxx bookinfo.com
```

Use [Fortio](https://fortio.org) to send a few hundred requests to the app:

```
docker container run \
  fortio/fortio \
  load -c 32 -qps 25 -t 30s http://bookinfo.com/productpage
```

> Back to Kiali _Graph_ and view the canary