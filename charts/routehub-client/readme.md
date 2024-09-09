# RouteHub Client Helm Chart

This Helm chart deploys the RouteHub Client application on a Kubernetes cluster.

## Components

The RouteHub Client application consists of the following components:

- REST API Server
- MQTT Server
- Redis
- TimescaleDB

## Prerequisites

- Kubernetes 1.12+
- Helm 3.0+

## Installing the Chart

To install the chart with the release name `my-release`:

```bash
helm install my-release ./charts/routehub-client
```

## Configuration

The following table lists the configurable parameters of the RouteHub Client chart and their default values.

| Parameter | Description | Default |
|-----------|-------------|---------|
| `global.customRegistry` | Custom Docker registry | `""` |
| `global.imagePullSecrets` | Global Docker registry secret names as an array | `[]` |
| `global.redis.PORT` | Redis port | `"6379"` |
| `global.redis.PASSWORD` | Redis password | `""` |
| `global.redis.DB` | Redis database number | `"0"` |
| `routehubClientRest.replicaCount` | Number of REST API replicas | `1` |
| `routehubClientRest.image.repository` | REST API image repository | `"your-repo/client-rest"` |
| `routehubClientRest.image.tag` | REST API image tag | `"latest"` |
| `routehubClientRest.service.type` | REST API service type | `"ClusterIP"` |
| `routehubClientRest.service.port` | REST API service port | `80` |
| `routehubClientMqtt.replicaCount` | Number of MQTT server replicas | `1` |
| `routehubClientMqtt.image.repository` | MQTT server image repository | `"your-repo/client-mqtt"` |
| `routehubClientMqtt.image.tag` | MQTT server image tag | `"latest"` |
| `routehubClientMqtt.service.type` | MQTT server service type | `"ClusterIP"` |
| `routehubClientMqtt.service.port` | MQTT server service port | `1883` |
| `redis.replicaCount` | Number of Redis replicas | `1` |
| `redis.image.repository` | Redis image repository | `"redis"` |
| `redis.image.tag` | Redis image tag | `"6.0.9"` |
| `redis.service.type` | Redis service type | `"ClusterIP"` |
| `redis.service.port` | Redis service port | `6379` |
| `timescale.replicaCount` | Number of TimescaleDB replicas | `1` |
| `timescale.image.repository` | TimescaleDB image repository | `"timescale/timescaledb"` |
| `timescale.image.tag` | TimescaleDB image tag | `"latest-pg12"` |
| `timescale.service.type` | TimescaleDB service type | `"ClusterIP"` |
| `timescale.service.port` | TimescaleDB service port | `5432` |

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`. For example:

```bash
helm install my-release ./charts/routehub-client --set routehubClientRest.replicaCount=2
```

Alternatively, a YAML file that specifies the values for the parameters can be provided while installing the chart. For example:

```bash
helm install my-release ./charts/routehub-client -f values.yaml
```

## Updating the Chart

To update the release of the RouteHub Client chart:

```bash
helm upgrade my-release ./charts/routehub-client
```

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```bash
helm delete my-release
```

This command removes all the Kubernetes components associated with the chart and deletes the release.
