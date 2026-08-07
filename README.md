# Kubeterm

English | [简体中文](README.zh-CN.md)

[![Release](https://img.shields.io/github/v/release/kbterm/kubeterm?label=release)](https://github.com/kbterm/kubeterm/releases/latest)
[![Platforms](https://img.shields.io/badge/platforms-macOS%20%7C%20Windows%20%7C%20Linux%20%7C%20iOS%20%7C%20Android-blue)](#get-started)
[![Discord](https://img.shields.io/badge/Discord-join-5865F2?logo=discord&logoColor=white)](https://discord.gg/Jv4zEEBMR2)

Kubeterm is a graphical management tool for Kubernetes clusters.
It provides clear visibility into your clusters, enables users to manage cluster resources and applications easily, as well as perform troubleshooting actions.

As a locally running application, Kubeterm doesn't require anything installed into the cluster.
It supports multiple platforms, including desktop and mobile devices.

![screenshot](images/screenshot.png)

## Features

- **Cluster Access & Authentication:**
  - Automatically loads the default kubeconfig with zero configuration.
  - Integrates with cloud provider accounts (GCP and Azure) to import clusters by simply logging in.
  - Supports cloud provider credentials, including AWS secret access keys, GCP service accounts, and Azure service principals.
  - Provides built-in OIDC authentication by reading the `auth-provider` field from the `user` section of the kubeconfig. [Example](https://github.com/kbterm/kubeterm/issues/9#issuecomment-2480673477)
- **AI Agent:**
  - Built-in AI assistant for Kubernetes troubleshooting and operations.
  - Works with cluster-aware context, including the selected cluster, resource details, YAML and log snippets.
  - Requires explicit approval before mutating cluster actions are executed.
- **Cluster dashboard:** View cluster status and live resource metrics (requires Kubernetes Metrics Server).
- **Prometheus integration:** Connect Prometheus for historical pod and node metrics charts. Auto-detects in-cluster instances and can reach them through the API server proxy, with no Ingress or port-forward required. [Setup](FAQ.md#how-do-i-enable-prometheus-for-historical-metrics)
- **Resource Viewer:** List and describe details of Kubernetes resources, including custom resources.
- **Resource operations:** Create, edit and delete resources, as well as advanced operations such as scaling, restart, node cordon/uncordon/drain.
- **Debugging:**
  - Debug Node or Pod by running an ephemeral container.
  - Run commands directly inside containers.
  - Inspect container's logs with searching, highlighting, tailing or downloading to local.
- **File transfer:** Copy files from/to Pod.
- **Port forwarding:** Forward local requests to Pod/Service.
- **Pod file browser:** Open file browser on pod directly in the app.
- **Helm management:** Install, uninstall, upgrade and rollback Helm charts/releases.
- **Cross-platform support:** Available on both mobile and desktop.
- **Data sync:** Sync kubeconfig across multiple clients via iCloud (iOS/macOS).


## Get started

### Mobile version

- **Download on your mobile devices:**

  <a href="https://apps.apple.com/us/app/kubeterm-kubernetes-client/id6450548861"><img src="https://developer.apple.com/news/images/download-on-the-app-store-badge.png" alt="Get it on AppStore" width='120px'/></a>
  <a href='https://play.google.com/store/apps/details?id=com.kubeterm'><img alt='Get it on Google Play' src='https://upload.wikimedia.org/wikipedia/commons/7/78/Google_Play_Store_badge_EN.svg' width='135px' /></a>


### Desktop version (macOS, Windows and Linux)

- **Download from GitHub Releases:**

  [Latest releases](https://github.com/kbterm/kubeterm/releases/latest)


- **Install macOS version by Homebrew:**

  ```
  brew install --cask kubeterm
  ```

- **Download macOS version from App Store:**

  <a href="https://apps.apple.com/us/app/kubeterm-kubernetes-client/id6450548861"><img src="https://developer.apple.com/news/images/download-on-the-app-store-badge.png" alt="Get it on AppStore" width='120px'/></a>

  > ℹ️ Note: This build runs inside Apple's App Sandbox and therefore cannot execute credential plugins (`exec` blocks) referenced in your kubeconfig.
  > If you rely on these, use one of the two options above instead — the build from GitHub Releases or Homebrew is not sandboxed.


## AI Agent

Kubeterm includes a built-in AI Agent for Kubernetes operations and troubleshooting.
The agent runs inside Kubeterm, can work with the currently selected cluster or resource context, and helps investigate issues directly from the UI.

![Kubeterm AI Agent Demo](https://raw.githubusercontent.com/kbterm/kubeterm/main/images/kubeterm-ai.gif)


## Security & Privacy

Kubeterm handles cluster credentials, so it's worth being explicit about where your data goes.

- **Your cluster data stays on your machine.** Kubeconfigs and cloud provider credentials are read and stored locally, and Kubeterm talks directly to your clusters' API servers — and, if you configure it, to your own Prometheus. There is no Kubeterm server in the path.
- **No telemetry.** Kubeterm collects no usage data, analytics, or crash reports.
- **The AI Agent is the only outbound exception.** When you use it, the context for your request goes directly from your machine to the AI provider you configured — Anthropic, OpenAI, Gemini, DeepSeek, or Ollama — authenticated with your own API key.
- **Mutating actions require approval.** The AI Agent must ask before it runs anything that changes cluster state.
- **Optional iCloud sync.** If you enable kubeconfig sync on iOS/macOS, it goes through your own iCloud account. It is off unless you turn it on.
- **macOS builds are signed and notarized by Apple.**


## FAQ

Common questions —  [FAQ](FAQ.md).


## Contact us

- Create and track [issues](https://github.com/kbterm/kubeterm/issues).
- Join us on [Discord](https://discord.gg/Jv4zEEBMR2).
- Follow us on [X](https://x.com/kubeterm).


## About this repository

This repository hosts Kubeterm's issue tracker, release downloads, and documentation.
The Kubeterm application is not open source.


## Copyright & License

Copyright (c) 2025 kubeterm — the content of this repository is released under the MIT license.
