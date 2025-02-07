# Kubeterm

Kubeterm is a graphical (GUI) management tool for kubernetes clusters. It provides good visibility into kubernetes cluster to enable user manage cluster resource and applications easily, and do troubleshooting actions.

As a application runnning locally, it doesn't require anything installed in the cluster. Multiple platforms, including desktop and mobile device, are supported.

![screenshot](images/screenshot.png)

## Features

- Cluster management:
  - Defaut kubeconfig is loaded automatically without additinal configuration.
  - Integrated with cloud provider account( GCP and Azure ). You can import the cluster by just login with your cloud account. 
  - Support cluster access credentials from cloud provider, including AWS secret access key, GCP service account, and Azure service principal.
  - Built-in OIDC authentication supporting. It reads fileld auth-provider of user from kubeconfig. [example](https://github.com/kbterm/kubeterm/issues/9#issuecomment-2480673477)
- Cluster dashboard: cluster status and resource statistics.
- Resource Viewer: List & describe kubernetes resource, just like *kubectl*.
- Resource operations: create, edit and delete resource, as well as many other operations, such as scaling, restart, node cordon/uncordon/drain, etc.
- Resource metrics: CPU/memory, kubernetes metrics server installation required.
- Inspect container's log, including searching, highlighting, and downloading logs, etc.
- Exec command in container.
- Debug Node or Pod with running a debugging container.
- Copy files from/to Pod.
- Port forwarding, forward requests to pod.
- Helm charts/release management: install, uninstall, upgrade and rollback.
- Multiple platforms supported, mobile and desktop.

## Get started
- Download for desktop (macOS and Windows):

    [Latest releases](https://github.com/kbterm/kubeterm/releases/latest)

- Download for mobile devices:

    <a href="https://apps.apple.com/us/app/kubeterm-kubernetes-client/id6450548861"><img src="https://developer.apple.com/news/images/download-on-the-app-store-badge.png" alt="Get it on AppStore" width='120px'/></a>
    <a href='https://play.google.com/store/apps/details?id=com.kubeterm'><img alt='Get it on Google Play' src='https://upload.wikimedia.org/wikipedia/commons/7/78/Google_Play_Store_badge_EN.svg' width='135px' /></a>

- You also can download macOS version on App Store :

    <a href="https://apps.apple.com/us/app/kubeterm-kubernetes-client/id6450548861"><img src="https://developer.apple.com/news/images/download-on-the-app-store-badge.png" alt="Get it on AppStore" width='120px'/></a>

    > Currently, due to restriction of Apple app sandbox, the version downloaded from Apple Store doesn't support executing credential plugin in kubeconfig, please get versions from github directly if you need such functionality.

## FAQ


## Contact us

- Create and track [issues](https://github.com/kbterm/kubeterm/issues).
- Join us on [Discord](https://discord.gg/Jv4zEEBMR2)
- Follow us on [Twitter](https://twitter.com/kubeterm)
