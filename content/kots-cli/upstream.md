---
date: 2019-12-16
linktitle: "upstream"
title: kots upstream
weight: 90180
---

The `kots upstream upgrade` fetches the latest version of the upstream application.
It is functionality equivalent to clicking the "Check For Updates" in the Admin Console.

### Usage
```bash
kubectl kots upstream upgrade [app-slug] [flags]
```
* _Replace `[app-slug]` with the app slug for your KOTS application (required)._
* _Provide `[flags]` according to the table below_

| Flag                 | Type | Description |
|:----------------------|------|-------------|
| `-h, --help` | |   help for upstream |
| `--kubeconfig`| string |    the kubeconfig to use _(default is `$KUBECONFIG`. If unset, then `$HOME/.kube/config`)_ |
| `-n, --namespace`| string |     the namespace where the admin console is running _(required)_ |
| `--deploy`| bool |     deploy the latest release if a new update is found |
| `--skip-preflights`| bool |     set to true to skip preflight checks |

### Example
```bash
kubectl kots upstream upgrade kots-sentry --namespace kots-sentry
```
