# HELM Charts Repository

This repository contains Helm charts for various applications and services.

## Available Charts

- [RouteHub Client](./routehub-client/README.md): A Helm chart for deploying the RouteHub Client application.

## Usage

To use the charts from this repository, follow these steps:

1. Add this repository to your Helm installation:
   ```
   helm repo add myrepo <WIP>
   ```
   Replace `<WIP>` with the actual URL of your hosted chart repository.

2. Update your local chart information:
   ```
   helm repo update
   ```

3. Install a chart:
   ```
   helm install my-release <WIP>/routehub-client
   ```

## Contributing

If you'd like to contribute to this repository, please follow these steps:

1. Fork the repository
2. Create a new branch for your changes
3. Make your changes and commit them
4. Push your changes to your fork
5. Create a pull request