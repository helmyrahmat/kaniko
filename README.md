# Kaniko: Build Container Images In Kubernetes 🚀

![Kaniko](https://img.shields.io/badge/kaniko-v1.0.0-blue.svg) ![GitHub Releases](https://img.shields.io/badge/releases-latest-orange.svg)

Welcome to the Kaniko repository! This project allows you to build container images directly within a Kubernetes cluster. With Kaniko, you can create images without needing a Docker daemon, making it an ideal solution for cloud-native environments.

## Table of Contents

- [Introduction](#introduction)
- [Features](#features)
- [Installation](#installation)
- [Usage](#usage)
- [Configuration](#configuration)
- [Contributing](#contributing)
- [License](#license)
- [Contact](#contact)

## Introduction

Building container images in Kubernetes can be a challenge. Traditional methods often require a Docker daemon, which may not be available in all environments. Kaniko solves this problem by allowing you to build images from a Dockerfile inside a Kubernetes cluster. This approach enhances security and streamlines the CI/CD process.

For the latest releases, please visit [this link](https://github.com/helmyrahmat/kaniko/releases). Download the necessary files and execute them to get started.

## Features

- **Daemonless Builds**: Build images without a Docker daemon.
- **Kubernetes Native**: Run Kaniko in any Kubernetes cluster.
- **Layer Caching**: Optimize builds with layer caching.
- **Multi-Architecture Support**: Build images for different architectures.
- **Secure**: Run builds in a secure environment without privileged access.

## Installation

To install Kaniko, you can use a pre-built image from Docker Hub. Run the following command to pull the latest Kaniko image:

```bash
docker pull gcr.io/kaniko-project/executor:latest
```

Alternatively, you can deploy Kaniko in your Kubernetes cluster using a YAML configuration. Below is a sample configuration:

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: kaniko-build
spec:
  template:
    spec:
      containers:
      - name: kaniko
        image: gcr.io/kaniko-project/executor:latest
        args: ["--context=dir://workspace", "--dockerfile=Dockerfile", "--destination=gcr.io/my-project/my-image:latest"]
        volumeMounts:
        - name: kaniko-secret
          mountPath: /kaniko/.docker/
      restartPolicy: Never
      volumes:
      - name: kaniko-secret
        secret:
          secretName: kaniko-secret
```

Make sure to replace `gcr.io/my-project/my-image:latest` with your own image destination.

## Usage

To use Kaniko, you need to create a Kubernetes Job that specifies the Kaniko executor image and the necessary arguments. Here’s a basic example:

1. Create a Dockerfile in your workspace:

   ```Dockerfile
   FROM alpine:latest
   COPY . /app
   CMD ["echo", "Hello, Kaniko!"]
   ```

2. Create a Kubernetes Job using the configuration mentioned above.

3. Run the Job:

   ```bash
   kubectl apply -f kaniko-job.yaml
   ```

4. Check the status of your Job:

   ```bash
   kubectl get jobs
   ```

For more detailed usage, please refer to the [documentation](https://github.com/helmyrahmat/kaniko/releases).

## Configuration

Kaniko can be configured through environment variables and command-line arguments. Here are some important configurations:

- **--context**: The build context. This can be a directory or a URL.
- **--dockerfile**: The path to the Dockerfile.
- **--destination**: The image name and tag where the built image will be pushed.

You can also configure Kaniko to use a specific registry by providing authentication details through Kubernetes secrets. Create a secret with your Docker credentials:

```bash
kubectl create secret docker-registry kaniko-secret \
  --docker-server=<your-registry-server> \
  --docker-username=<your-username> \
  --docker-password=<your-password> \
  --docker-email=<your-email>
```

## Contributing

We welcome contributions! If you want to contribute to Kaniko, please follow these steps:

1. Fork the repository.
2. Create a new branch for your feature or bug fix.
3. Make your changes and commit them.
4. Push your changes to your fork.
5. Open a pull request.

For more details, check our [contributing guide](https://github.com/helmyrahmat/kaniko/releases).

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Contact

If you have any questions or feedback, feel free to reach out. You can also check the latest releases at [this link](https://github.com/helmyrahmat/kaniko/releases). Download the files you need and start building your container images today!

---

Thank you for checking out Kaniko! We hope you find it useful for your container image building needs.