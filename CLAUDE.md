# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is an Argo CD example repository containing Kubernetes Application manifests and the `product-catalog` microservice's Kubernetes resources (plain YAML and Helm chart) for deploying via Argo CD's GitOps workflow.

## Repository Structure

- `argocd-apps/` — Argo CD `Application` custom resources that tell Argo CD what to deploy and where
  - `application.yaml` — deploys `product-catalog` (plain YAML) to namespace `product-catalog`
  - `application-helm.yaml` — deploys `product-catalog` (Helm chart) to namespace `product-catalog-helm`
- `product-catalog/` — Kubernetes resources for the product-catalog microservice
  - `deployments/` — plain-YAML manifests (Deployment, Service, Ingress)
  - `helm/product-catalog/` — Helm chart with templates, values, and helpers
- Both Application manifests source from `https://github.com/pankaj02/argocd-example.git` (branch `main`)


## Key Conventions

- All Application resources live in the `argocd` namespace (metadata.namespace).
- Each app creates its own target namespace via `syncOptions: [CreateNamespace=true]`.
- Destination cluster is always the local cluster (`https://kubernetes.default.svc`).
