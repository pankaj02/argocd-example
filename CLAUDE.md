# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is an Argo CD example repository containing Kubernetes Application manifests and the `product-catalog` microservice's Kubernetes resources (plain YAML and Helm chart) for deploying via Argo CD's GitOps workflow.

## Repository Structure

- `argocd-apps/` — Argo CD examples, one subfolder per pattern
  - `plain-yaml/application.yaml` — deploys `product-catalog` from plain YAML manifests
  - `helm/application.yaml` — deploys `product-catalog` from a Helm chart
  - `app-of-apps/` — App of Apps pattern example
    - `root-application.yaml` — root Application that manages child apps in `apps/`
    - `apps/` — child Application manifests managed by the root app
- `workloads/` — Kubernetes workload manifests, one subfolder per microservice
  - `product-catalog/deployments/` — plain-YAML manifests (Deployment, Service, Ingress)
  - `product-catalog/helm/product-catalog/` — Helm chart with templates, values, and helpers
- All Application manifests source from `https://github.com/pankaj02/argocd-example.git` (branch `main`)


## Key Conventions

- All Application resources live in the `argocd` namespace (metadata.namespace).
- Each app creates its own target namespace via `syncOptions: [CreateNamespace=true]`.
- Destination cluster is always the local cluster (`https://kubernetes.default.svc`).
