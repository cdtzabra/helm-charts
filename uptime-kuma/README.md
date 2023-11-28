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


# enable metrics scraping
serviceMonitor:
  enabled: false
  interval: 60s
  scheme: http
  port: 80
  # authentication is required to scrap the endpoint /metrics
  ########### see https://github.com/louislam/uptime-kuma/wiki/API-Keys
  # so you might not enable servicemonitor during the first deployment
  # then as soon as your kuma is UP, generate an apikey
  # come back to enable serviceMonitor
  # To provide the apikey, you have two options:
  ## secret.useExistingSecret: to provide the existing secret name. It must contain the keys: username & apikey
  ## secret.createSecret.apikey to provide apikey value in order to create a secret
  secret:
    useExistingSecret: ""
    createSecret:
      apikey: ""
      user: "randomuser"

```

