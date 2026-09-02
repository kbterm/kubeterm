# Kubeterm

[English](README.md) | 简体中文

[![Release](https://img.shields.io/github/v/release/kbterm/kubeterm?label=release)](https://github.com/kbterm/kubeterm/releases/latest)
[![Platforms](https://img.shields.io/badge/platforms-macOS%20%7C%20Windows%20%7C%20Linux%20%7C%20iOS%20%7C%20Android-blue)](#快速开始)
[![Discord](https://img.shields.io/badge/Discord-join-5865F2?logo=discord&logoColor=white)](https://discord.gg/Jv4zEEBMR2)

Kubeterm 是一款图形化的 Kubernetes 集群管理工具。
它让你清晰地掌握集群状态，轻松管理集群资源与应用，并进行故障排查。

Kubeterm 作为本地应用运行，无需在集群中安装任何组件。
支持桌面端与移动端等多个平台。

![screenshot](images/screenshot.png)

## 功能特性

- **集群接入与认证：**
  - 零配置自动加载默认的 kubeconfig。
  - 集成云厂商账号（GCP 和 Azure），只需登录即可导入集群。
  - 支持云厂商凭证，包括 AWS Secret Access Key、GCP 服务账号和 Azure 服务主体。
  - 内置 OIDC 认证，通过读取 kubeconfig 中 `user` 部分的 `auth-provider` 字段实现。[示例](https://github.com/kbterm/kubeterm/issues/9#issuecomment-2480673477)
- **AI 助手：**
  - 内置 AI 助手，用于 Kubernetes 故障排查与运维操作。
  - 具备集群上下文感知能力，包括当前选中的集群、资源详情、YAML 和日志片段。
  - 执行任何会修改集群的操作前，都需要明确授权确认。
- **集群仪表盘：** 查看集群状态与实时资源指标（需要 Kubernetes Metrics Server）。
- **Prometheus 集成：** 接入 Prometheus，查看 Pod 和节点的历史指标曲线。支持自动探测集群内的实例，并可通过 API Server 代理访问，无需 Ingress 或端口转发。[配置方法](FAQ.zh-CN.md#如何启用-prometheus-查看历史指标)
- **资源查看：** 列出并查看 Kubernetes 资源的详细信息，包括自定义资源（CRD）。
- **资源操作：** 创建、编辑和删除资源，以及扩缩容、重启、节点 cordon/uncordon/drain 等高级操作。
- **调试：**
  - 通过运行临时容器（ephemeral container）调试节点或 Pod。
  - 直接在容器内执行命令。
  - 查看容器日志，支持搜索、高亮、实时跟踪以及下载到本地。
- **文件传输：** 在 Pod 与本地之间复制文件。
- **端口转发：** 将本地请求转发到 Pod/Service。
- **Pod 文件浏览器：** 在应用内直接打开 Pod 的文件浏览器。
- **Helm 管理：** 安装、卸载、升级和回滚 Helm chart/release。
- **跨平台支持：** 移动端与桌面端均可使用。
- **数据同步：** 通过 iCloud 在多个客户端之间同步 kubeconfig（iOS/macOS）。


## 快速开始

### 移动端版本

- **在移动设备上下载：**

  <a href="https://apps.apple.com/us/app/kubeterm-kubernetes-client/id6450548861"><img src="https://developer.apple.com/news/images/download-on-the-app-store-badge.png" alt="Get it on AppStore" width='120px'/></a>
  <a href='https://play.google.com/store/apps/details?id=com.kubeterm'><img alt='Get it on Google Play' src='https://upload.wikimedia.org/wikipedia/commons/7/78/Google_Play_Store_badge_EN.svg' width='135px' /></a>


### 桌面端版本（macOS、Windows 和 Linux）

- **从 GitHub Releases 下载：**

  [最新版本](https://github.com/kbterm/kubeterm/releases/latest)


- **通过 Homebrew 安装 macOS 版本：**

  ```
  brew install --cask kubeterm
  ```

- **从 App Store 下载 macOS 版本：**

  <a href="https://apps.apple.com/us/app/kubeterm-kubernetes-client/id6450548861"><img src="https://developer.apple.com/news/images/download-on-the-app-store-badge.png" alt="Get it on AppStore" width='120px'/></a>

  > ℹ️ 注意：该版本运行在 Apple 的 App Sandbox 沙盒环境中，因此无法执行 kubeconfig 中引用的凭证插件（`exec` 配置块）。
  > 如果你依赖这类插件，请改用上面两种方式 —— 通过 GitHub Releases 或 Homebrew 获取的版本不受沙盒限制。


## AI 助手

Kubeterm 内置了用于 Kubernetes 运维与故障排查的 AI 助手。
该助手在 Kubeterm 内部运行，可以结合当前选中的集群或资源上下文工作，帮助你直接在界面中排查问题。

![Kubeterm AI Agent Demo](https://raw.githubusercontent.com/kbterm/kubeterm/main/images/kubeterm-ai.gif)


## 安全与隐私

Kubeterm 会接触集群凭证，因此有必要明确说明你的数据流向。

- **集群数据保留在本机。** kubeconfig 和云厂商凭证均在本地读取和存储，Kubeterm 直接与集群的 API Server 通信（如果你配置了 Prometheus，也会直接与你自己的 Prometheus 通信），链路中不存在任何 Kubeterm 服务器。
- **无遥测数据。** Kubeterm 不收集任何使用数据、分析数据或崩溃报告。
- **AI 助手是唯一的对外例外。** 使用时，请求所需的上下文会从你的机器直接发送给你所配置的 AI 服务商 —— Anthropic、OpenAI、Gemini 或 DeepSeek —— 并使用你自己的 API key 进行认证。
- **修改类操作需要授权。** AI 助手在执行任何会改变集群状态的操作前，都必须先征求你的同意。
- **可选的 iCloud 同步。** 如果你在 iOS/macOS 上启用了 kubeconfig 同步，数据会通过你自己的 iCloud 账号传输。该功能默认关闭，需手动开启。
- **macOS 版本经过 Apple 签名与公证。**


## 常见问题

解答见 [FAQ](FAQ.zh-CN.md)。


## 用户案例

在团队中使用 Kubeterm？欢迎把你的团队添加到 [ADOPTERS.md](ADOPTERS.md)，
这有助于我们确定平台与功能的优先级，也能为正在评估 Kubeterm 的人提供参考。


## 联系我们

- 提交并跟踪 [issues](https://github.com/kbterm/kubeterm/issues)。
- 加入我们的 [Discord](https://discord.gg/Jv4zEEBMR2)。
- 在 [X](https://x.com/kubeterm) 上关注我们。


## 关于本仓库

本仓库用于托管 Kubeterm 的 issue 跟踪、版本发布和文档。
Kubeterm 应用本身并非开源软件。


## 版权与许可

Copyright (c) 2025 kubeterm —— 本仓库的内容以 MIT 许可证发布。
