# eksctl-config

This folder contains a sample `eksctl` cluster configuration you can use as a starting point to create a cluster with a managed nodegroup and commonly required addons.

Files
- `eksctl-cluster.yaml` — sample cluster config that creates a managed nodegroup (c5.large) with min=1, max=3 and installs managed addons: coredns, kube-proxy, vpc-cni, aws-ebs-csi-driver.

Quick start

1. Update `eksctl-cluster.yaml` metadata (cluster name, region, Kubernetes version) as needed.
2. Create the cluster with:

```bash
eksctl create cluster -f eksctl-cluster.yaml
```

Notes
- The config attaches several AWS-managed policies to the node group's instance role so addons (like EBS CSI and the VPC CNI) can function. Review and scope down the permissions for production use.
- `metrics-server` is not provided as an EKS managed addon at the time of writing — install it via Helm or a manifest after cluster creation, for example:

```bash
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

- If you prefer using IAM OIDC and IAM Roles for Service Accounts (IRSA) for controller addons, ensure an OIDC provider is associated with the cluster; `eksctl` can create it automatically when you create the cluster.

Customizing
- To attach more granular permissions for the node role or to create IAM Roles for Service Accounts (IRSA) for individual addons, consider using `eksctl create iamserviceaccount` or adding `iam.serviceRoleARN` and `iam.instanceProfileARN` entries and pre-creating roles.

Support
If you want, I can:
- Add a metric-server Helm deployment to this repo.
- Replace the broad managed policies with a minimal custom policy for EBS/VPC-CNI.
- Add an example of creating the OIDC provider and IRSA-enabled service accounts for the ebs-csi driver.
