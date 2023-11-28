# uptime-kuma

A Helm Chart to deploy [uptime-kuma](https://github.com/louislam/uptime-kuma/tree/master) on Kubernetes.

The uptime-kuma [repository](https://github.com/louislam/uptime-kuma/tree/master) doesn't provide its own chart yet.


## Install 

```
cd uptime-kuma

# render the template
helm template kuma ./

# deploy
helm install kuma ./ --create-namespace -n kuma --atomic -f <custom-values-file>
```

Example of `<custom-values-file>`:

```yaml
ingress:
  enabled: true
  className: "nginx"
  annotations:
    cert-manager.io/cluster-issuer: "letsencrypt-prod"
  hosts:
    - host: kuma.example.com
      paths:
        - path: /
          pathType: ImplementationSpecific
  tls:
    - hosts:
        - kuma.example.com
      secretName: kuma-tls


serviceMonitor:
  enabled: true
```