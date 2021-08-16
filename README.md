# eks-fluxcd-boostrap

This repository contains non-production [FluxCD](https://eksctl.io/usage/gitops-v2/) manifests used on GitOps-based boostrap of Amazon EKS sandbox clusters.

## Prerequisites

- eksctl, >= 0.58.0
- awscli, >= 2.2.25

## Setup

1. Fork this repository

2. Create an GitHub Personal Access Token, and export it to environment variables:

```bash
export GITHUB_TOKEN=<REDACTED>
export GITHUB_USER=<GH USERNAME>
```

3. Checkout the fork repository, create the `demo` branch and push to the origin:

```bash
git clone git@github.com:davivcgarcia/eks-fluxcd-boostrap.git
cd eks-fluxcd-boostrap/
git checkout -b demo
git push -u origin demo
```

4. Modify the `owner` on `eksctl-config.yaml` file to use your GitHub username:

```yaml
gitops:
  flux:
    gitProvider: github
    flags:
      owner: davivcgarcia
      repository: eks-fluxcd-boostrap
      private: "false"
      branch: demo
      path: ./clusters/sandbox/
      namespace: flux-system
```

5. Create the Amazon EKS cluster with `eksctl`:

```bash
eksctl create cluster -f helpers/eksctl-config.yaml
```

## Cleanup

1. Delete your Amazon EKS cluster:

```bash
eksctl delete cluster --name sandbox
```

2. Delete the remote and local `demo` branch of your forked repository:

```bash
git checkout main
git push --delete origin demo
git branch -D demo
```
