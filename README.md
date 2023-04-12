# Apache Flink on Kubernetes

This project provides a simple and reliable way to deploy an Apache Flink cluster on Kubernetes using Kubernetes-native resources. Apache Flink is a powerful open-source distributed stream and batch data processing engine, designed to deliver low-latency, high-throughput, and fault-tolerant analytics at scale. The configuration files in this repository include resources for Flink configuration and logging settings, Deployments for Flink JobManager and TaskManager components, a Service for the JobManager, and an Ingress resource for routing external traffic to the Flink WebUI.

This project is set up with GitLab CI/CD to automatically deploy the Flink cluster on Kubernetes.

## Components

This section briefly explains the components of the Flink cluster:

- **ConfigMap**: Stores the Flink and logging configurations (`flink-conf.yaml` and `log4j-console.properties`). This ConfigMap is mounted as a volume in the JobManager and TaskManager containers.

- **JobManager**: The JobManager is the central coordinator of the Flink cluster. It is responsible for managing the lifecycle of jobs, distributing tasks to TaskManagers, and coordinating checkpoints and recovery. The `jobmanager-deployment.yaml` file contains the Deployment configuration for the JobManager, which includes a single replica, liveness probe, and mounting of the Flink ConfigMap.

- **TaskManager**: TaskManagers are responsible for executing tasks and processing data streams. They communicate with the JobManager for coordination and receive tasks to execute. The `taskmanager-deployment.yaml` file contains the Deployment configuration for the TaskManager, which includes the desired number of replicas, liveness probe, and mounting of the Flink ConfigMap.

- **JobManager Service**: The JobManager Service is a Kubernetes Service resource that exposes the JobManager's RPC, Blob Server, and WebUI ports. This Service enables communication between JobManager and TaskManager components and provides access to the Flink WebUI.

- **Ingress (Optional)**: The Ingress resource routes external traffic to the Flink WebUI using a specified hostname. This configuration assumes you have an Ingress controller deployed in your Kubernetes cluster. Update the Ingress configuration (`ingress.yaml`) with the appropriate hostname and other settings as needed.

## Deployment

The GitLab CI/CD pipeline is configured to deploy the Flink cluster on Kubernetes automatically. When you push changes to the repository, GitLab CI/CD will run the pipeline to deploy or update the Flink cluster.

## Accessing the Flink WebUI

With the Ingress resource configured, you can access the Flink WebUI through the hostname specified in the `ingress.yaml` file. Ensure your DNS or `/etc/hosts` file is configured to resolve the specified hostname to the IP address of your Ingress controller.

## Configuration

You can customize the Flink and logging configurations in the `configmap.yaml` file. The JobManager and TaskManager resources can be scaled and customized using the respective Deployment configurations (`jobmanager-deployment.yaml` and `taskmanager-deployment.yaml`). If you're using an Ingress resource, you can customize the routing rules and hostname in the `ingress.yaml` file.

## Cleanup

The GitLab CI/CD pipeline will automatically clean up resources when the branch is deleted or the project is archived. Alternatively, you can manually delete the Kubernetes resources by running `kubectl delete -f <resource_file>.yaml`.

## License

This project is licensed under the [Apache License 2.0](LICENSE).

## Contributing

If you would like to contribute to this project, please follow the [contributing guidelines](CONTRIBUTING.md).
