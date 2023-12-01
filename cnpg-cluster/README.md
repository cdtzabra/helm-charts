# cloudnative-pg instance

The purpose of this chart is to spawn a cnpg cluster instances...

It offers options for creating the different types of cloudnative-pg clusters: see [cnpg-cluster](https://cloudnative-pg.io/documentation/current/samples/) examples.



## Install

**Cloudnative-pg operator (cnpg-operator) must be installed first on your cluster** before installing this chart.


Example of `custom-values.yaml`

```
---
# create pg cluster for keycloak
fullnameOverride: keycloak-cnpg-cluster

bootstrap:
  initdb:
    database: bitnami_keycloak  # inital database to create
    owner: bn_keycloak          # inital database user

storage:
  resizeInUseVolumes: true
  size: 10Gi           # postgres pvc size


# backup to s3
backup_enabled: true
backup:
  s3_endpointURL: "https://s3.example.com"  
  s3_destinationPath: "s3://cnpg-cluster/db-keycloak"   # bucket/folder-for-keycloak
  s3_credential:
    secretName: "cnpg-cluster-s3-backup"                        # You must create this secret first  with the keys below
    secret_accesKey: "ACCESS_KEY_ID"
    secret_secretKey: "ACCESS_SECRET_KEY"
    # kubectl create secret generic cnpg-cluster-s3-backup cnpg-cluster-s3-backup --from-literal ACCESS_KEY_ID="xx" --from-literal ACCESS_SECRET_KEY="yy"


# Scheduled Backup
# See the chart values.yaml file for more details about this section
scheduledbackup:
  enabled: true
  # perform an immediate backup as soon as the resource is created
  immediate: false
  schedule: "0 0 */8 * * *"
  # Beware that this format accepts also the seconds field, and it is different from the crontab format in Unix/Linux systems.
  # it has a total of 6 fiels while kubernetes/linux cronjob has 5

```

Deploy the chart: `helm install keycloak-cnpg-cluster ./ -n keycloak --create-namespace -f custom-values.yaml --atomic`