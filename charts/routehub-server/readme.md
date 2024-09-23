# RouteHub Server Helm Chart

This Helm chart deploys the RouteHub Server application on a Kubernetes cluster.

## Components

The RouteHub Server application consists of the following components:

- GraphQL API Server
- Redis
- PostgreSQL for Data & Policy storage

## Prerequisites

- Kubernetes 1.12+
- Helm 3.0+
- Hosted Zitadel Instance

## Installing the Chart

To install the chart with the release name `my-release`:

### Create a namespace

```bash
kubectl create namespace routehub-server
```

### Local chart Deployment

```bash
# Local chart
helm install my-release ./charts/routehub-server
  --namespace routehub-server \
  --set global.customRegistry=your.registry/ \
  --set global.imagePullSecrets[0].name=your-secret-name
```

### Remote chart Deployment

```bash
helm repo add routehub-helm https://routehub-link.github.io/RouteHub.HELM/

# Remote chart
helm install my-release routehub-helm/routehub-server \
  --namespace routehub-server \
  --set global.customRegistry=your.registry/ \
  --set global.imagePullSecrets[0].name=your-secret-name
```

## Configuration

The following table lists the configurable parameters of the RouteHub Server chart and their default values.
Please refer to the [values.yaml](values.yaml) file for more information.

| Section                    | Key                                    | Default Value                                          | Description                                  |
| -------------------------- | -------------------------------------- | ------------------------------------------------------ | -------------------------------------------- |
| global                     | customRegistry                         | ""                                                     | Custom registry for images                   |
| global.imagePullSecrets    | name                                   | ""                                                     | Name of the image pull secret                |
| global.redis               | HOST                                   | "{{ .Release.Name }}-redis"                            | Redis host                                   |
| global.redis               | PORT                                   | 6379                                                   | Redis port                                   |
| global.redis               | PASSWORD                               | ""                                                     | Redis password                               |
| global.redis               | DB                                     | 0                                                      | Redis database number                        |
| redis                      | image.repository                       | eqalpha/keydb                                          | Redis image repository                       |
| redis                      | image.tag                              | latest                                                 | Redis image tag                              |
| redis                      | replicaCount                           | 1                                                      | Number of Redis replicas                     |
| redis                      | service.type                           | ClusterIP                                              | Redis service type                           |
| redis                      | service.port                           | 6379                                                   | Redis service port                           |
| postgres                   | image.repository                       | postgres                                               | PostgreSQL image repository                  |
| postgres                   | image.tag                              | latest                                                 | PostgreSQL image tag                         |
| postgres                   | database                               | RouteHub                                               | PostgreSQL database name                     |
| postgres                   | username                               | postgres                                               | PostgreSQL username                          |
| postgres                   | password                               | postgres                                               | PostgreSQL password                          |
| postgres                   | replicaCount                           | 1                                                      | Number of PostgreSQL replicas                |
| postgres                   | service.type                           | ClusterIP                                              | PostgreSQL service type                      |
| postgres                   | service.port                           | 5432                                                   | PostgreSQL service port                      |
| routehubServer             | image.repository                       | routehub-server                                        | RouteHub server image repository             |
| routehubServer             | image.tag                              | latest                                                 | RouteHub server image tag                    |
| routehubServer             | replicaCount                           | 1                                                      | Number of RouteHub server replicas           |
| routehubServer             | service.type                           | LoadBalancer                                           | RouteHub server service type                 |
| routehubServer             | service.port                           | 8080                                                   | RouteHub server service port                 |
| routehubServer             | livenessProbe.path                     | /healthz                                               | Path for liveness probe                      |
| routehubServer             | livenessProbe.port                     | 8080                                                   | Port for liveness probe                      |
| routehubServer             | livenessProbe.failureThreshold         | 3                                                      | Failure threshold for liveness probe         |
| routehubServer             | livenessProbe.periodSeconds            | 120                                                    | Period seconds for liveness probe            |
| routehubServer             | livenessProbe.initialDelaySeconds      | 30                                                     | Initial delay seconds for liveness probe     |
| routehubServer.environment | HOSTING_MODE                           | REST                                                   | Hosting mode for the server                  |
| routehubServer.environment | SERVICES_DOMAIN_UTILS_HOST             | "http://localhost:9001"                                | Domain utils service host                    |
| routehubServer.environment | DATABASE_TYPE_MIGRATE                  | "true"                                                 | Enable database migration                    |
| routehubServer.environment | DATABASE_TYPE_SEED                     | "true"                                                 | Enable database seeding                      |
| routehubServer.environment | DATABASE_TYPE_PROVIDER                 | "postgres"                                             | Database provider type                       |
| routehubServer.environment | DATABASE_SEED_ADMINS_0_SUBJECT         | "278978826325721091"                                   | Subject ID for first admin seed              |
| routehubServer.environment | DATABASE_SEED_ADMINS_1_SUBJECT         | "278979034530971651"                                   | Subject ID for second admin seed             |
| routehubServer.environment | DATABASE_SEED_ORGANIZATION_NAME        | "RouteHub"                                             | Name of the seeded organization              |
| routehubServer.environment | DATABASE_SEED_ORGANIZATION_DESCRIPTION | "RouteHub is a GraphQL API for managing routes"        | Description of the seeded organization       |
| routehubServer.environment | DATABASE_SEED_DOMAIN_NAME              | "RouteHub Public Shortener"                            | Name of the seeded domain                    |
| routehubServer.environment | DATABASE_SEED_DOMAIN_URL               | "https://s.r4l.cc"                                     | URL of the seeded domain                     |
| routehubServer.environment | GRAPHQL_PLAYGROUND                     | "true"                                                 | Enable GraphQL playground                    |
| routehubServer.environment | GRAPHQL_DATALOADER_WAIT                | "1ms"                                                  | GraphQL dataloader wait time                 |
| routehubServer.environment | GRAPHQL_DATALOADER_CACHE               | "true"                                                 | Enable GraphQL dataloader cache              |
| routehubServer.environment | GRAPHQL_DATALOADER_LRUE_SIZE           | "1000"                                                 | GraphQL dataloader LRU cache size            |
| routehubServer.environment | GRAPHQL_DATALOADER_LRUE_EXPIRE         | "10m"                                                  | GraphQL dataloader LRU cache expiration time |
| routehubServer.environment | CASBIN_MODEL                           | "./config/authorization/rbac.model.conf"               | Path to Casbin RBAC model configuration      |
| routehubServer.environment | CASBIN_LOG_LEVEL                       | "DEBUG"                                                | Casbin log level                             |
| routehubServer.environment | ZITADEL_CLIENT_ID                      | "278962924343590915"                                   | Zitadel client ID                            |
| routehubServer.environment | ZITADEL_CALLBACK                       | "/oauth2/callback"                                     | Zitadel OAuth2 callback path                 |
| routehubServer.environment | ZITADEL_SCOPE                          | "openid,profile,email"                                 | Zitadel OAuth2 scopes                        |
| routehubServer.environment | ZITADEL_AUTHORIZE_URL                  | "http://server.proxy.internal:7077/oauth/v2/authorize" | Zitadel authorization URL                    |
| routehubServer.environment | ZITADEL_TOKEN_URL                      | "http://server.proxy.internal:7077/oauth/v2/token"     | Zitadel token URL                            |
| routehubServer.environment | ZITADEL_ISSUER                         | "http://server.proxy.internal:7077"                    | Zitadel issuer URL                           |
| routehubServer.environment | ZITADEL_DOMAIN                         | "server.proxy.internal"                                | Zitadel domain                               |
| routehubServer.environment | ZITADEL_PORT                           | "7077"                                                 | Zitadel port                                 |
| routehubServer.environment | ZITADEL_INSECURE                       | "true"                                                 | Allow insecure connections to Zitadel        |

## About the private registry

### Create a secret for the registry

```bash
kubectl create secret docker-registry your-secret-name \
  --namespace routehub-server \
  --docker-server=your.registry \
  --docker-username=your-username \
  --docker-password=your-password
```

### Use the secret in the Helm chart

```bash
helm install my-release routehub-helm/routehub-server \
  --namespace routehub-server \
  --create-namespace \
  --set global.customRegistry=your.registry/ \
  --set global.imagePullSecrets[0].name=your-secret-name
```

### For Debug you could add the following flags

```bash
--dry-run --debug
```

## About Zitadel

App uses Zitadel for OAUTH authentication. To use the app, you need to create a Zitadel instance and configure the app with the Zitadel client ID, callback URL, and other required information.
After that you need to create an admin user in Zitadel and assign the user to the organization.
Geather the userId and add it to the DATABASE_SEED_ADMINS_0_SUBJECT and DATABASE_SEED_ADMINS_1_SUBJECT environment variables. (These are optional, but recommended for access as an admin and first time run.)
