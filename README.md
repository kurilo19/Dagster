# Dagster Helm

[![Artifact HUB](https://img.shields.io/endpoint?url=https://artifacthub.io/badge/repository/dagster)](https://artifacthub.io/packages/search?repo=dagster)

#


## Introduction

[Dagster](https://github.com/dagster-io/dagster) is a Python library for building data applications. This chart will bootstrap a Dagit web server deployment on a Kubernetes cluster using the Helm package manager.

In addition, our helm chart allows for Dagster configuration such as:

- Deploying user code containers separately from Dagster system components
- Specifying the Dagster run launcher
- Specifying the Dagster scheduler to handle recurring pipeline runs

# Deploying Dagster on Helm

## Commands

### Install kubernetes to a specific version

```shell
$ minikube start --kubernetes-version=v1.19.15
```

### Install helm to a specific version

[Helm 3.4.2](https://github.com/helm/helm/releases?q=helm+3.4.2&expanded=true)

[Installing Helm - From the Binary Releases](https://helm.sh/docs/intro/install/)

1. Download your desired version
2. Unpack it (tar -zxvf helm-v3.0.0-linux-amd64.tar.gz)
3. Find the helm binary in the unpacked directory, and move it to its desired destination

```shell
$ mv linux-amd64/helm /usr/local/bin/helm
```

### Add the Dagster Helm chart repository

```shell
$ helm repo add dagster https://dagster-io.github.io/helm
$ helm install kurylo dagster/dagster \
    --namespace dagster \
    --create-namespace
$ helm show values dagster/dagster > values.yaml
$ helm upgrade --install dagster dagster/dagster -f values.yaml
```

## Tutorial

For a introductory guide on using Dagster on Helm, [check out our documentation](https://docs.dagster.io/deployment/guides/kubernetes/deploying-with-helm).

## Contributing

We cover instructions to get started with developing on our chart.

### JSON Schema

[JSON Schema](https://helm.sh/docs/topics/charts/#schema-files) can impose a structure on our Dagster chart's values to ensure requirement
checks, type validation, range validation, and constraint validation. The Dagster chart's JSON Schema is generated through a Pydantic
model of our values.

```bash
# Install the cli to generate the JSON Schema
pip install -e ./schema

# Display the resulting schema from the Dagster chart values Pydantic model
dagster-helm schema show

# Update the existing schema
dagster-helm schema apply
```

### Template Testing

We use pytest to assert behaviors about our Helm chart.

Helm values are modelled using Pydantic, and then piped through to `helm template`
to generate the chart's Kubernetes manifests. Kubernetes manifests are then transformed into their pythonic object representations,
and assertions are made about these objects to ensure that our templating logic is correct.
