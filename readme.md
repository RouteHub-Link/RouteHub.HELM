# HELM Charts Repository

This repository contains Helm charts for various applications and services.

## Available Charts

- [RouteHub Client](./routehub-client/README.md): A Helm chart for deploying the RouteHub Client application.

## Usage

To use the charts from this repository, follow these steps:

1. Add this repository to your Helm installation:
```
helm repo add routehub-helm https://routehub-link.github.io/RouteHub.HELM/
```

2. Update your local chart information:
```
helm repo update
```

3. Install a chart:
```bash
helm install my-hub routehub-helm/routehub-client-hub \
   --namespace routehub-clients \
   --create-namespace \
   --set global.customRegistry=your.registry/ \
   --set global.imagePullSecrets[0].name=your-secret-name \
   --set routehubClientRest.environment.Name="Test Hub" \
   --set routehubClientRest.environment.ORGANIZATION_ID=test-org \
   --set routehubClientRest.environment.OWNER_ID=test-owner \
   --set routehubClientRest.environment.PLATFORM_ID=12058bdf-8940-43b3-bd90-13487e4c8fc4 \
   --set routehubClientRest.environment.SEED=TRUE 
```

## Contributing

If you'd like to contribute to this repository, please follow these steps:

1. Fork the repository
2. Create a new branch for your changes
3. Make your changes and commit them
4. Push your changes to your fork
5. Create a pull request
