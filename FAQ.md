# FAQ

English | [简体中文](FAQ.zh-CN.md)

[← Back to README](README.md)


## Why can't the macOS App Store build use the credential plugin in my kubeconfig?

The App Store build runs inside Apple's App Sandbox, which prevents it from launching external
binaries. kubeconfig entries that use `exec` credential plugins — such as `aws eks get-token` for
EKS or `gke-gcloud-auth-plugin` for GKE — therefore cannot run. Use the macOS build from
[GitHub Releases](https://github.com/kbterm/kubeterm/releases/latest) or Homebrew instead, which
is not sandboxed:

```
brew install --cask kubeterm
```

OIDC is an exception — see [the OIDC question below](#how-do-i-connect-a-cluster-that-uses-oidc).


## How do I connect a cluster that uses OIDC?

OIDC is built into Kubeterm, so you don't need the `kubelogin` binary installed. Selecting the
cluster opens your browser to sign in — or an in-app window, depending on your setting for
opening links in an external browser.

Kubeterm reads either kubeconfig shape. **`auth-provider`:**

```yaml
users:
  - name: my-user
    user:
      auth-provider:
        name: oidc
        config:
          idp-issuer-url: https://accounts.google.com
          client-id: <client-id>
          client-secret: <client-secret>   # optional — see PKCE note below
```

**`exec` with kubelogin** — Kubeterm recognises `kubectl oidc-login get-token` and reads the
flags directly instead of executing anything, which is why this also works in the sandboxed App
Store build:

```yaml
users:
  - name: my-user
    user:
      exec:
        apiVersion: client.authentication.k8s.io/v1beta1
        command: kubectl
        args:
          - oidc-login
          - get-token
          - --oidc-issuer-url=https://accounts.google.com
          - --oidc-client-id=<client-id>
```

At minimum you need a client ID, plus **either** an issuer URL — endpoints are then discovered
from `/.well-known/openid-configuration` — **or** an explicit authorization endpoint and token
endpoint (`idp-authorization-endpoint` / `idp-token-endpoint`, or `--oidc-authorization-endpoint`
/ `--oidc-token-endpoint`).

Also supported: `extra-scopes`, `idp-certificate-authority-data` for an IdP behind a private CA,
`use-access-token`, `insecure-skip-tls-verify`, and redirect overrides (`redirect-url`, or
`--listen-address` / `--oidc-redirect-url-hostname` in the exec form).

Notes:

- **A client secret is optional.** Kubeterm always uses PKCE, which helps with IdPs that force
  client-secret rotation.
- **The exec form must be `command: kubectl`** with `oidc-login` and `get-token` among the args —
  the shape kubelogin's own documentation generates. Other spellings aren't detected as OIDC.
- **Google:** `access_type=offline` is added automatically so a refresh token is issued.


## How do I enable Prometheus for historical metrics?

The cluster dashboard reads live values from Metrics Server. Connecting Prometheus adds
historical charts for pods and nodes. It's configured per cluster:

1. Open the cluster's details page and select **Prometheus**.
2. Press **Auto-detect from cluster**. Kubeterm looks for Prometheus Operator resources, Services
   labelled `app.kubernetes.io/name=prometheus` or `app=prometheus`, and well-known service names
   in the `monitoring`, `prometheus`, `observability`, `kube-system`, and `kube-prometheus-stack`
   namespaces, then probes each candidate.
3. Choose a verified result, or enter a URL yourself:
   - `kube-proxy://<namespace>/<service>:<port>` — routed through the Kubernetes API server, so
     no Ingress or port-forward is required.
   - `https://…` — for a Prometheus you can already reach directly.
4. Set authentication if needed (bearer token or basic auth), then **Test Connection** and save.

If something doesn't work:

- **"Permission denied — ask admin for services/proxy RBAC"** — the `kube-proxy://` form needs
  permission to proxy through the API server:

  ```yaml
  - apiGroups: [""]
    resources: ["services/proxy"]
    verbs: ["get"]
  ```

- **Node charts are empty but pod charts work** — change **Node label** from `node` to
  `instance`. Some node_exporter setups label metrics by scrape-target address rather than node
  name.
- **"Reachable but not Prometheus"** — usually something else listening on that port, or a
  strict-mTLS service mesh refusing the API server's connection.


## The cluster dashboard shows no CPU or memory metrics.

Metrics come from the Kubernetes Metrics Server, which is not installed by default on every
distribution. Install `metrics-server` in the cluster and the dashboard will populate.


## What does the AI Agent send, and where does it go?

Only the context needed for your request: the selected cluster, resource details, and any YAML or
log snippets involved. It goes directly to the AI provider you configured, using your own API key
— Kubeterm does not proxy it. Selecting Ollama keeps everything local. Any action that would
modify the cluster requires your explicit approval first.

See [Security & Privacy](README.md#security--privacy) for the full picture of what leaves your
machine.


## Can I connect to a cluster with a self-signed or private CA certificate?

Yes — Kubeterm uses the certificate authority data from your kubeconfig. If a connection still
fails with a TLS error, make sure you are on the latest version, then
[open an issue](https://github.com/kbterm/kubeterm/issues) with the error text.


---

Question not answered here? Ask on [Discord](https://discord.gg/Jv4zEEBMR2) or open an
[issue](https://github.com/kbterm/kubeterm/issues).
