# Environment

## Prerequisites

- OpenShift 4.16 environment
- OpenShift account with administrator privileges
- Nvidia L4 GPU x 1 of 1 compute node
- OpenShift AI components installed

## The component changed from pre-configured TAP environment

1. upgrade `OpenShift` AI to 2.13.1, changed `DataScienceCluster` kserve serving ingressGateway certificate type to `OpenshiftDefaultIngress` instead  of self-signed

2. removed `gitlab`, `sonarqube`, `minio`  and `GitLab Runner` component(there are specific configuration of these component in the demo, these component will be configured manually in the tutorial steps)

3. install Red Hat - Authorino operator

4. below pre-configured components in TAP environments will not be used for this demo

   - Advanced Cluster Management for Kubernetes

   - Multicluster engine for Kubernetes

   - Red Hat Streams for Apache Kafka

   - OpenShift Data Foundation

   - Red Hat OpenShift GitOps

   - Orchestrator Operator

   - Red Hat Quay

   - Red Hat Single Sign-On Operator

   - Red Hat Trusted Artifact Signer

   - Advanced Cluster Security for Kubernetes

   - OpenShift Serverless Logic Operator

