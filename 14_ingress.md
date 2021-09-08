# Ingress

Without Ingress, routing rules are connected to a Service.
If the Service does not exist anymore, the application can be updates without worrsig about external access.

Ingress configures a Layer 7 HTTP(S) load balancer consisting of:

- TLS (Transport Layer Security)
- Name-based virtual hosting 
- Fanout routing
- Loadbalancing
- Custom rules

An Ingress is defined as follows:

```yaml
apiVersion: networking.k8s.io/v1 
kind: Ingress
metadata:
  name: virtual-host-ingress
  namespace: default
spec:
  rules:
  - host: blue.example.com
    http:
      paths:
      - backend:
          service:
            name: webserver-blue-svc
            port:
              number: 80
        path: /
        pathType: ImplementationSpecific
  - host: green.example.com
    http:
      paths:
      - backend:
          service:
            name: webserver-green-svc
            port:
              number: 80
        path: /
        pathType: ImplementationSpecific
```

*Fanout* Ingress rules are used to split traffic to the same domain to different pods by path (`domain.com/green`, `domain.com/blue`).

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: fan-out-ingress
  namespace: default
spec:
  rules:
  - host: example.com
    http:
      paths:
      - path: /blue
        backend:
          service:
            name: webserver-blue-svc
            port:
              number: 80
        pathType: ImplementationSpecific
      - path: /green
        backend:
          service:
            name: webserver-green-svc
            port:
              number: 80
        pathType: ImplementationSpecific
```

## IngressController

The `IngressController` watches the Master Node API and updates the load balancer automatically.