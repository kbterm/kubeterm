# Kubeterm

Kubeterm is a graphical (GUI) management tool for kubernetes clusters. It provides a good visibility into kubernetes cluster to enable user manage cluster resource and applications easily, and do troubleshooting actions.

Running as a local application, it doesn't require anything installed into your cluster. Multiple platforms, including desktop and mobile device, are supported.

![screenshot](images/screenshot.png)

## Features

- Import and manage clusters:
  - Load local kubeconfig.
  - Import cluster by credentials, including AWS secret access key, GCP service account, and Azure service principal.
  - Login with your account of cloud provider ( GCP and Azure ) which host your kubernetes cluster.
- Cluster dashboard, view cluster status and resource statistics.
- List & describe details of kubernetes resource, just like *kubectl*.
- Manage kubernete resource: create, edit and delete resource, as well as many resource operations, such as scaling, restart, node cordon/uncordon/drain, etc.
- View resource (CPU/memory) metrics.
- Inspect container's log.
- Exec command on container.
- Install and manage Helm charts/release: install, uninstall, upgrade and rollback.
- Multiple platforms supported, mobile and desktop.

## Get started

- Download for mobile devices:

    <a href="https://apps.apple.com/us/app/kubeterm-kubernetes-client/id6450548861"><img src="https://developer.apple.com/news/images/download-on-the-app-store-badge.png" alt="Get it on AppStore" width='120px'/></a>
    <a href='https://play.google.com/store/apps/details?id=com.kubeterm'><img alt='Get it on Google Play' src='https://upload.wikimedia.org/wikipedia/commons/7/78/Google_Play_Store_badge_EN.svg' width='135px' /></a>

- Download on App Store for macOS:

    <a href="https://apps.apple.com/us/app/kubeterm-kubernetes-client/id6450548861"><img src="https://developer.apple.com/news/images/download-on-the-app-store-badge.png" alt="Get it on AppStore" width='120px'/></a>

- Download latest release for desktop directly (macOS and Windows):

    [Latest releases](https://github.com/kbterm/kubeterm/releases/latest)



## Contact us

- Create and track [issues](https://github.com/kbterm/kubeterm/issues).
- Join us on [Discord](https://discord.gg/Jv4zEEBMR2)
- Follow us on [Twitter](https://twitter.com/kubeterm)
