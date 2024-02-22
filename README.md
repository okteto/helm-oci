# helm-oci

The following demonstrates how to push a Helm Chart to an OCI registry which is configured as a [Private Registry](https://www.okteto.com/docs/self-hosted/manage/custom-resource-definitions/#private-registries) in Okteto Self-Hosted.

## Bootstrap

This repository contains a Helm Chart which was created using the following commands:

```bash
mkdir chart
helm create chart/example
```

The Helm Chart is a simple web application that uses Nginx.

## Requirements

Ensure that the following requirements are met before deploying the example application:
- Helm v3 version 3.8 or higher. Installation instructions for `helm` are available [here](https://helm.sh/docs/intro/install/).
- Setup the following [Okteto Variables](https://www.okteto.com/docs/manifest/okteto-variables/#manage-okteto-variables-from-the-okteto-dashboard):
  - `REGISTRY_HOST`: The hostname of the OCI registry.
  - `REGISTRY_REPO`: The repository name in the OCI registry.


### Create repository in AWS ECR

Requirements:
- `aws` CLI v2 version 2.15 or higher. Follow the installation instructions for `aws` CLI v2 [here](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html#getting-started-install-instructions).
- Setup the following environment variables:
  - `AWS_REGION`: The AWS region where the ECR repository will be created. Use the snippet below to use your current AWS Region or choose your own value.
    ```bash
    export AWS_REGION="$(aws configure get region)"
    ```
  - `AWS_PAGER`: (Optional) Disable AWS CLI v2 Pagination.
  - `REPOSITORY_NAME`: The name of the ECR repository. It must be the same value you configured in the `REGISTRY_REPO` Okteto Variable.

Create a repository in AWS ECR using the following command:

```bash
aws ecr create-repository --repository-name ${REPOSITORY_NAME}
```

## Deploy the example application

To [deploy](https://www.okteto.com/docs/reference/okteto-cli/#deploy) the example application, run the following command:

```bash
okteto deploy
```
