---
date: 2020-08-14
linktitle: Stateful Components
title: Managing Admin Console State
weight: 10065
---

The KOTS Admin Console requires persistent storage for state.
When installing KOTS to an existing Kubernetes cluster, the installer will provision the required stateful components using the default StorageClass in the cluster.
The only requirement is that a StorageClass be present.

## Postgresql

KOTS uses a Postgresql StatefulSet to store the version history, application metadata and other small amounts of data needed to manage the application(s).
This Postgresql component is deployed with KOTS and secured with a randomly generated password, and only exposed as a ClusterIP on the overlay network.

{{< notes title="Future Plans">}}

A future version of KOTS will remove the Postgresql requirement to enable multiple replicas of every component and facilitate a true highly-available deployment.
The work is being tracked in [GitHub issue #537](https://github.com/replicatedhq/kots/pull/537).

{{< /notes >}}

## S3 Compatible Object Store

KOTS also requires an S3 compaitble object store to store application archives and support bundles.
A KOTS installation to an existing cluster will deploy Minio to satisfy this requirement, and nothing is required during installation.
When deploying, Minio is configured with a randomly generated AccessKeyID and SecretAccessKey, and only exposed as a ClusterIP on the overlay network.

{{< notes title="Future Plans">}}

A future version of KOTS will remove compatibiltiy with S3 in favor of a unified storage backend using OCI registries.
KOTS 1.19 contains the first release of this.
Before S3 is removed, full compatibility will be guaranteed and KOTS will be able to seemlessly and automatically migrate data.

{{< /notes >}}

## OCI Registry (alpha support)

Beginning with KOTS 1.19, experimental support has been added to use an OCI Registry for storage instead of the Object Store listed above.
When this feature is enabled, KOTS will deploy [docker distribution](https://github.com/docker/distribution) to satisfy the storage requirement.


When using the OCI Registry instead of the S3 compatible object store, a few benefits are realized:

**The OCI registry can be externalized**

The KOTS installation does not need to use it's own, locally configured registry for storage. 
A customer can choose to provide their own registry to use as storage.
At this time, KOTS has been tested to be compatible with the following registries:

- [Docker Hub](https://hub.docker.com)
- Locally Hosted Docker Registry [(docker distribution)](https://github.com/docker/distribution)
- [Google Artifact Registry](https://cloud.google.com/artifact-registry/docs/transition-from-gcr)
- Amazon Elastic Container Registry [(coming soon)](https://github.com/aws/containers-roadmap/issues/308)

**Improved performance**

Because application artifacts are made from layered container images and application manifests, there's similarity between versions.
Using the OCI format enables KOTS to build on the layer functionality to minimize the storage and bandwidth for each version.

#### Enabling OCI Registry Support

To install KOTS with the built-in OCI registry instead of Minio, run:

```shell
kubectl kots install --with-dockerdistribution
```

To install KOTS using your own registry, run:

```shell
kubectl kots install --storage-base-uri docker://docker.io/myorg/myapp
```

