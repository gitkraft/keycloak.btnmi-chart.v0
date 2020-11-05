# Keycloak

[Keycloak](https://www.keycloak.org) is a high performance Java-based identity and access management solution. It lets developers add an authentication layer to their applications with minimum effort.

## TL;DR

```console
  helm repo add bitnami https://charts.bitnami.com/bitnami
  helm install my-release bitnami/keycloak
```

## Introduction

Bitnami charts for Helm are carefully engineered, actively maintained and are the quickest and easiest way to deploy containers on a Kubernetes cluster that are ready to handle production workloads.

This chart bootstraps a [Keycloak](https://github.com/bitnami/bitnami-docker-keycloak) deployment on a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.

Bitnami charts can be used with [Kubeapps](https://kubeapps.com/) for deployment and management of Helm Charts in clusters. This Helm chart has been tested on top of [Bitnami Kubernetes Production Runtime](https://kubeprod.io/) (BKPR). Deploy BKPR to get automated TLS certificates, logging and monitoring for your applications.

## Prerequisites

- Kubernetes 1.12+
- Helm 2.12+ or Helm 3.0-beta3+

## Installing the Chart

To install the chart with the release name `my-release`:

```bash
$ helm repo add bitnami https://charts.bitnami.com/bitnami
$ helm install my-release bitnami/keycloak
```

These commands deploy a Keycloak application on the Kubernetes cluster in the default configuration.

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```bash
$ helm delete my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Parameters

The following tables lists the configurable parameters of the Keycloak chart and their default values per section/component:

### Global parameters

| Parameter                               | Description                                                | Default                                                 |
|-----------------------------------------|------------------------------------------------------------|---------------------------------------------------------|
| `global.imageRegistry`                  | Global Docker image registry                               | `nil`                                                   |
| `global.imagePullSecrets`               | Global Docker registry secret names as an array            | `[]` (does not add image pull secrets to deployed pods) |
| `global.storageClass`                   | Global storage class for dynamic provisioning              | `nil`                                                   |

### Common parameters

| Parameter                               | Description                                                | Default                                                 |
|-----------------------------------------|------------------------------------------------------------|---------------------------------------------------------|
| `nameOverride`                          | String to partially override keycloak.fullname             | `nil`                                                   |
| `fullnameOverride`                      | String to fully override keycloak.fullname                 | `nil`                                                   |
| `commonLabels`                          | Labels to add to all deployed objects                      | `{}`                                                    |
| `commonAnnotations`                     | Annotations to add to all deployed objects                 | `{}`                                                    |
| `clusterDomain`                         | Default Kubernetes cluster domain                          | `cluster.local`                                         |
| `extraDeploy`                           | Array of extra objects to deploy with the release          | `[]` (evaluated as a template)                          |

### Keycloak parameters

| Parameter                               | Description                                                                              | Default                                                 |
|-----------------------------------------|------------------------------------------------------------------------------------------|---------------------------------------------------------|
| `image.registry`                        | Keycloak image registry                                                                  | `docker.io`                                             |
| `image.repository`                      | Keycloak image name                                                                      | `bitnami/keycloak`                                      |
| `image.tag`                             | Keycloak image tag                                                                       | `{TAG_NAME}`                                            |
| `image.pullPolicy`                      | Keycloak image pull policy                                                               | `IfNotPresent`                                          |
| `image.pullSecrets`                     | Specify docker-registry secret names as an array                                         | `[]` (does not add image pull secrets to deployed pods) |
| `image.debug`                           | Specify if debug logs should be enabled                                                  | `false`                                                 |
| `auth.createAdminUser`                  | Create administrator user on boot                                                        | `true`                                                  |
| `auth.adminUser`                        | Keycloak administrator name                                                              | `user`                                                  |
| `auth.adminPassword`                    | Keycloak administrator password for the new user                                         | _random 10 character long alphanumeric string_          |
| `auth.managementUser`                   | Wildfly management user                                                                  | `manager`                                               |
| `auth.managementPassword`               | Wildfly management password                                                              | _random 10 character long alphanumeric string_          |
| `auth.tls.enabled`                      | Enable TLS encryption                                                                    | `false`                                                 |
| `auth.tls.jksSecret`                    | Existing secret containing the truststore and one keystore per Keycloak replica          | `nil`                                                   |
| `auth.tls.keystorePassword`             | Password to access the keystore when it's password-protected                             | `nil`                                                   |
| `auth.tls.truststorePassword`           | Password to access the truststore when it's password-protected                           | `nil`                                                   |
| `bindAddress`                           | Keycloak bind address                                                                    | `0.0.0.0`                                               |
| `proxyAddressForwarding`                | Enable Proxy Address Forwarding                                                          | `false`                                                 |
| `serviceDiscovery.enabled`              | Enable Service Discovery for Keycloak (required if `replicaCount` > `1`)                 | `false`                                                 |
| `serviceDiscovery.protocol`             | Sets the protocol that Keycloak nodes would use to discover new peers                    | `kubernetes.KUBE_PING`                                  |
| `serviceDiscovery.properties`           | Properties for the discovery protocol set in `serviceDiscovery.protocol` parameter       | `[]`                                                    |
| `serviceDiscovery.transportStack`       | Transport stack for the discovery protocol set in `serviceDiscovery.protocol` parameter  | `tcp`                                                   |
| `cache.ownersCount`                     | Number of nodes that will replicate cached data                                          | `1`                                                     |
| `cache.authOwnersCount`                 | Number of nodes that will replicate cached authentication data                           | `1`                                                     |
| `configuration`                         | Keycloak Configuration. Auto-generated based on other parameters when not specified      | `nil`                                                   |
| `existingConfigmap`                     | Name of existing ConfigMap with Keycloak configuration                                   | `nil`                                                   |
| `initdbScripts`                         | Dictionary of initdb scripts                                                             | `{}` (evaluated as a template)                          |
| `initdbScriptsConfigMap`                | ConfigMap with the initdb scripts (Note: Overrides `initdbScripts`)                      | `nil`                                                   |
| `command`                               | Override default container command (useful when using custom images)                     | `nil`                                                   |
| `args`                                  | Override default container args (useful when using custom images)                        | `nil`                                                   |
| `extraEnvVars`                          | Extra environment variables to be set on Keycloak container                              | `{}`                                                    |
| `extraEnvVarsCM`                        | Name of existing ConfigMap containing extra env vars                                     | `nil`                                                   |
| `extraEnvVarsSecret`                    | Name of existing Secret containing extra env vars                                        | `nil`                                                   |

### Keycloak deployment/statefulset parameters

| Parameter                             | Description                                                                                | Default                                                 |
|---------------------------------------|--------------------------------------------------------------------------------------------|---------------------------------------------------------|
| `replicaCount`                        | Number of Keycloak replicas to deploy                                                      | `1`                                                     |
| `containerPorts.http`                 | HTTP port to expose at container level                                                     | `8080`                                                  |
| `containerPorts.https`                | HTTPS port to expose at container level                                                    | `8443`                                                  |
| `podSecurityContext`                  | Keycloak pods' Security Context                                                            | Check `values.yaml` file                                |
| `containerSecurityContext`            | Keycloak containers' Security Context                                                      | Check `values.yaml` file                                |
| `resources.limits`                    | The resources limits for the Keycloak container                                            | `{}`                                                    |
| `resources.requests`                  | The requested resources for the Keycloak container                                         | `{}`                                                    |
| `lifecycleHooks`                      | LifecycleHooks to set additional configuration at startup.                                 | `{}` (evaluated as a template)                          |
| `livenessProbe`                       | Liveness probe configuration for Keycloak                                                  | Check `values.yaml` file                                |
| `readinessProbe`                      | Readiness probe configuration for Keycloak                                                 | Check `values.yaml` file                                |
| `customLivenessProbe`                 | Override default liveness probe                                                            | `nil`                                                   |
| `customReadinessProbe`                | Override default readiness probe                                                           | `nil`                                                   |
| `updateStrategy`                      | Strategy to use to update Pods                                                             | Check `values.yaml` file                                |
| `podAffinityPreset`                   | Pod affinity preset. Ignored if `affinity` is set. Allowed values: `soft` or `hard`        | `""`                                                    |
| `podAntiAffinityPreset`               | Pod anti-affinity preset. Ignored if `affinity` is set. Allowed values: `soft` or `hard`   | `soft`                                                  |
| `nodeAffinityPreset.type`             | Node affinity preset type. Ignored if `affinity` is set. Allowed values: `soft` or `hard`  | `""`                                                    |
| `nodeAffinityPreset.key`              | Node label key to match. Ignored if `affinity` is set.                                     | `""`                                                    |
| `nodeAffinityPreset.values`           | Node label values to match. Ignored if `affinity` is set.                                  | `[]`                                                    |
| `affinity`                            | Affinity for pod assignment                                                                | `{}` (evaluated as a template)                          |
| `nodeSelector`                        | Node labels for pod assignment                                                             | `{}` (evaluated as a template)                          |
| `tolerations`                         | Tolerations for pod assignment                                                             | `[]` (evaluated as a template)                          |
| `podLabels`                           | Extra labels for Keycloak pods                                                             | `{}`                                                    |
| `podAnnotations`                      | Annotations for Keycloak pods                                                              | `{}`                                                    |
| `priorityClassName`                   | Controller priorityClassName                                                               | `nil`                                                   |
| `lifecycleHooks`                      | LifecycleHooks to set additional configuration at startup.                                 | `{}` (evaluated as a template)                          |
| `extraVolumeMounts`                   | Optionally specify extra list of additional volumeMounts for Keycloak container(s)         | `[]`                                                    |
| `extraVolumes`                        | Optionally specify extra list of additional volumes for Keycloak pods                      | `[]`                                                    |
| `initContainers`                      | Add additional init containers to the Keycloak pods                                        | `{}` (evaluated as a template)                          |
| `sidecars`                            | Add additional sidecar containers to the Keycloak pods                                     | `{}` (evaluated as a template)                          |

### Exposure parameters

| Parameter                               | Description                                                                              | Default                                                 |
|-----------------------------------------|------------------------------------------------------------------------------------------|---------------------------------------------------------|
| `service.type`                          | Kubernetes service type                                                                  | `LoadBalancer`                                          |
| `service.port`                          | Service HTTP port                                                                        | `80`                                                    |
| `service.nodePort`                      | Service HTTPS port                                                                       | `443`                                                   |
| `service.nodePorts.http`                | Kubernetes HTTP node port                                                                | `""`                                                    |
| `service.nodePorts.https`               | Kubernetes HTTPS node port                                                               | `""`                                                    |
| `service.clusterIP`                     | Keycloak service clusterIP IP                                                            | `None`                                                  |
| `service.externalTrafficPolicy`         | Enable client source IP preservation                                                     | `Cluster`                                               |
| `service.loadBalancerIP`                | loadBalancerIP if service type is `LoadBalancer`                                         | `nil`                                                   |
| `service.loadBalancerSourceRanges`      | Address that are allowed when service is LoadBalancer                                    | `[]`                                                    |
| `service.annotations`                   | Annotations for Keycloak service                                                         | `{}` (evaluated as a template)                          |
| `ingress.enabled`                       | Enable ingress controller resource                                                       | `false`                                                 |
| `ingress.certManager`                   | Add annotations for cert-manager                                                         | `false`                                                 |
| `ingress.hostname`                      | Default host for the ingress resource                                                    | `keycloak.local`                                        |
| `ingress.tls`                           | Enable TLS configuration for the hostname defined at `ingress.hostname` parameter        | `false`                                                 |
| `ingress.annotations`                   | Ingress annotations                                                                      | `{}` (evaluated as a template)                          |
| `ingress.extraHosts[0].name`            | Additional hostnames to be covered                                                       | `nil`                                                   |
| `ingress.extraHosts[0].path`            | Additional hostnames to be covered                                                       | `nil`                                                   |
| `ingress.extraTls[0].hosts[0]`          | TLS configuration for additional hostnames to be covered                                 | `nil`                                                   |
| `ingress.extraTls[0].secretName`        | TLS configuration for additional hostnames to be covered                                 | `nil`                                                   |
| `ingress.secrets[0].name`               | TLS Secret Name                                                                          | `nil`                                                   |
| `ingress.secrets[0].certificate`        | TLS Secret Certificate                                                                   | `nil`                                                   |
| `ingress.secrets[0].key`                | TLS Secret Key                                                                           | `nil`                                                   |
| `networkPolicy.enabled`                 | Enable the default NetworkPolicy policy                                                  | `false`                                                 |
| `networkPolicy.allowExternal`           | Don't require client label for connections                                               | `true`                                                  |
| `networkPolicy.additionalRules`         | Additional NetworkPolicy rules                                                           | `{}` (evaluated as a template)                          |

### RBAC parameters

| Parameter                               | Description                                                         | Default                                                 |
|-----------------------------------------|---------------------------------------------------------------------|---------------------------------------------------------|
| `serviceAccount.create`                 | Enable the creation of a ServiceAccount for Keycloak pods           | `true`                                                  |
| `serviceAccount.name`                   | Name of the created ServiceAccount                                  | Generated using the `keycloak.fullname` template        |
| `rbac.create`                           | Weather to create & use RBAC resources or not                       | `false`                                                 |
| `rbac.rules`                            | Specifies whether RBAC resources should be created                  | `[]`                                                    |

### Other parameters

| Parameter                               | Description                                                         | Default                                                 |
|-----------------------------------------|---------------------------------------------------------------------|---------------------------------------------------------|
| `pdb.create`                            | Enable/disable a Pod Disruption Budget creation                     | `false`                                                 |
| `pdb.minAvailable`                      | Minimum number/percentage of pods that should remain scheduled      | `1`                                                     |
| `pdb.maxUnavailable`                    | Maximum number/percentage of pods that may be made unavailable      | `nil`                                                   |
| `autoscaling.enabled`                   | Enable autoscaling for Keycloak                                     | `false`                                                 |
| `autoscaling.minReplicas`               | Minimum number of Keycloak replicas                                 | `1`                                                     |
| `autoscaling.maxReplicas`               | Maximum number of Keycloak replicas                                 | `11`                                                    |
| `autoscaling.targetCPU`                 | Target CPU utilization percentage                                   | `nil`                                                   |
| `autoscaling.targetMemory`              | Target Memory utilization percentage                                | `nil`                                                   |

### Metrics parameters

| Parameter                                 | Description                                                                                                          | Default                                                      |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------|
| `metrics.enabled`                         | Enable exposing Keycloak statistics                                                                                  | `false`                                                      |
| `metrics.service.port`                    | Service HTTP managemenet port                                                                                        | `9990`                                                       |
| `metrics.service.annotations`             | Annotations for enabling prometheus to access the metrics endpoints                                                  | `{prometheus.io/scrape: "true", prometheus.io/port: "9990"}` |
| `metrics.serviceMonitor.enabled`          | Create ServiceMonitor Resource for scraping metrics using PrometheusOperator                                         | `false`                                                      |
| `metrics.serviceMonitor.namespace`        | Namespace which Prometheus is running in                                                                             | `nil`                                                        |
| `metrics.serviceMonitor.interval`         | Interval at which metrics should be scraped                                                                          | `30s`                                                        |
| `metrics.serviceMonitor.scrapeTimeout`    | Specify the timeout after which the scrape is ended                                                                  | `nil`                                                        |
| `metrics.serviceMonitor.relabellings`     | Specify Metric Relabellings to add to the scrape endpoint                                                            | `nil`                                                        |
| `metrics.serviceMonitor.honorLabels`      | honorLabels chooses the metric's labels on collisions with target labels.                                            | `false`                                                      |
| `metrics.serviceMonitor.additionalLabels` | Used to pass Labels that are required by the Installed Prometheus Operator                                           | `{}`                                                         |
| `metrics.serviceMonitor.release`          | Used to pass Labels release that sometimes should be custom for Prometheus Operator                                  | `nil`                                                        |

### Database parameters

| Parameter                                    | Description                                                                           | Default                                                   |
|----------------------------------------------|---------------------------------------------------------------------------------------|-----------------------------------------------------------|
| `postgresql.enabled`                         | Deploy a PostgreSQL server to satisfy the applications database requirements          | `true`                                                    |
| `postgresql.postgresqlUsername`              | PostgreSQL user to create (used by Keycloak)                                          | `bn_keycloak`                                             |
| `postgresql.postgresqlPassword`              | Password for the Dicourse user - ignored if existingSecret is provided                | `some-password`                                           |
| `postgresql.postgresqlDatabase`              | Name of the database to create                                                        | `bitnami_keycloak`                                        |
| `postgresql.persistence.enabled`             | Enable database persistence using PVC                                                 | `true`                                                    |
| `externalDatabase.host`                      | Host of the external database                                                         | `""`                                                      |
| `externalDatabase.port`                      | Database port number (when using an external db)                                      | `5432`                                                    |
| `externalDatabase.user`                      | PostgreSQL username (when using an external db)                                       | `bn_keycloak`                                             |
| `externalDatabase.password`                  | Password for the above username (when using an external db)                           | `""`                                                      |
| `externalDatabase.database`                  | Name of the existing database (when using an external db)                             | `bitnami_keycloak`                                        |

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`. For example,

```bash
helm install my-release --set auth.adminPassword=secretpassword bitnami/keycloak
```

The above command sets the Keycloak administrator password to `secretpassword`.

Alternatively, a YAML file that specifies the values for the parameters can be provided while installing the chart. For example,

```bash
$ helm install my-release -f values.yaml bitnami/keycloak
```

> **Tip**: You can use the default [values.yaml](values.yaml)

## Configuration and installation details

### [Rolling VS Immutable tags](https://docs.bitnami.com/containers/how-to/understand-rolling-tags-containers/)

It is strongly recommended to use immutable tags in a production environment. This ensures your deployment does not change automatically if the same tag is updated with a different image.

Bitnami will release a new chart updating its containers if a new version of the main container, significant changes, or critical vulnerabilities exist.

### Using an external database

Sometimes you may want to have Keycloak connect to an external database rather than installing one inside your cluster, e.g. to use a managed database service, or use run a single database server for all your applications. To do this, the chart allows you to specify credentials for an external database under the [`externalDatabase` parameter](#database-parameters). You should also disable the PostgreSQL installation with the `postgresql.enabled` option. For example with the following parameters:

```console
postgresql.enabled=false
externalDatabase.host=myexternalhost
externalDatabase.port=5432
externalDatabase.user=myuser
externalDatabase.password=mypassword
externalDatabase.database=mydatabase
```

Note also that if you disable PostgreSQL per above you MUST supply values for the `externalDatabase` connection.

### Adding extra environment variables

In case you want to add extra environment variables (useful for advanced operations like custom init scripts), you can use the `extraEnvVars` property.

```yaml
kong:
  extraEnvVars:
    - name: LOG_LEVEL
      value: error
```

Alternatively, you can use a ConfigMap or a Secret with the environment variables. To do so, use the `extraEnvVarsCM` or the `extraEnvVarsSecret` values.

### Sidecars and Init Containers

If you have a need for additional containers to run within the same pod as the Keycloak app (e.g. an additional metrics or logging exporter), you can do so via the `sidecars` config parameter. Simply define your container according to the Kubernetes container spec.

```yaml
sidecars:
  - name: your-image-name
    image: your-image
    imagePullPolicy: Always
    ports:
      - name: portname
       containerPort: 1234
```

Similarly, you can add extra init containers using the `initContainers` parameter.

```yaml
initContainers:
  - name: your-image-name
    image: your-image
    imagePullPolicy: Always
    ports:
      - name: portname
        containerPort: 1234
```

### Initialize a fresh instance

The [Bitnami Keycloak](https://github.com/bitnami/bitnami-docker-keycloak) image allows you to use your custom scripts to initialize a fresh instance. In order to execute the scripts, you can specify custom scripts using the `initdbScripts` parameter as dict.

In addition to this option, you can also set an external ConfigMap with all the initialization scripts. This is done by setting the `initdbScriptsConfigMap` parameter. Note that this will override the previous option.

The allowed extensions is `.sh`.

### Deploying extra resources

There are cases where you may want to deploy extra objects, such a ConfigMap containing your app's configuration or some extra deployment with a micro service used by your app. For covering this case, the chart allows adding the full specification of other objects using the `extraDeploy` parameter.

### Setting Pod's affinity

This chart allows you to set your custom affinity using the `affinity` paremeter. Find more infomation about Pod's affinity in the [kubernetes documentation](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#affinity-and-anti-affinity).

As an alternative, you can use of the preset configurations for pod affinity, pod anti-affinity, and node affinity available at the [bitnami/common](https://github.com/bitnami/charts/tree/master/bitnami/common#affinities) chart. To do so, set the `podAffinityPreset`, `podAntiAffinityPreset`, or `nodeAffinityPreset` parameters.

### Ingress

This chart provides support for ingress resources. If you have an ingress controller installed on your cluster, such as [nginx-ingress-controller](https://github.com/bitnami/charts/tree/master/bitnami/nginx-ingress-controller) or [contour](https://github.com/bitnami/charts/tree/master/bitnami/contour) you can utilize the ingress controller to serve your application.

To enable ingress integration, please set `ingress.enabled` to `true`.

#### Hosts

Most likely you will only want to have one hostname that maps to this Keycloak installation. If that's your case, the property `ingress.hostname` will set it. However, it is possible to have more than one host. To facilitate this, the `ingress.extraHosts` object can be specified as an array. You can also use `ingress.extraTLS` to add the TLS configuration for extra hosts.

For each host indicated at `ingress.extraHosts`, please indicate a `name`, `path`, and any `annotations` that you may want the ingress controller to know about.

For annotations, please see [this document](https://github.com/kubernetes/ingress-nginx/blob/master/docs/user-guide/nginx-configuration/annotations.md). Not all annotations are supported by all ingress controllers, but this document does a good job of indicating which annotation is supported by many popular ingress controllers.

### TLS Secrets

This chart will facilitate the creation of TLS secrets for use with the ingress controller, however, this is not required. There are three common use cases:

- Helm generates/manages certificate secrets.
- User generates/manages certificates separately.
- An additional tool (like [cert-manager](https://github.com/jetstack/cert-manager/)) manages the secrets for the application.

In the first two cases, it's needed a certificate and a key. We would expect them to look like this:

- certificate files should look like (and there can be more than one certificate if there is a certificate chain)

    ```console
    -----BEGIN CERTIFICATE-----
    MIID6TCCAtGgAwIBAgIJAIaCwivkeB5EMA0GCSqGSIb3DQEBCwUAMFYxCzAJBgNV
    ...
    jScrvkiBO65F46KioCL9h5tDvomdU1aqpI/CBzhvZn1c0ZTf87tGQR8NK7v7
    -----END CERTIFICATE-----
    ```

- keys should look like:

    ```console
    -----BEGIN RSA PRIVATE KEY-----
    MIIEogIBAAKCAQEAvLYcyu8f3skuRyUgeeNpeDvYBCDcgq+LsWap6zbX5f8oLqp4
    ...
    wrj2wDbCDCFmfqnSJ+dKI3vFLlEz44sAV8jX/kd4Y6ZTQhlLbYc=
    -----END RSA PRIVATE KEY-----
    ```

If you are going to use Helm to manage the certificates, please copy these values into the `certificate` and `key` values for a given `ingress.secrets` entry.

If you are going to manage TLS secrets outside of Helm, please know that you can create a TLS secret (named `keycloak.local-tls` for example).

## Troubleshooting

Find more information about how to deal with common errors related to Bitnami’s Helm charts in [this troubleshooting guide](https://docs.bitnami.com/general/how-to/troubleshoot-helm-chart-issues).
