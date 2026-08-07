# 常见问题

[English](FAQ.md) | 简体中文

[← 返回 README](README.zh-CN.md)


## 为什么 macOS App Store 版本无法使用我 kubeconfig 中的凭证插件？

App Store 版本运行在 Apple 的 App Sandbox 沙盒环境中，无法启动外部可执行文件，因此 kubeconfig 中使用
`exec` 凭证插件的配置 —— 例如 EKS 的 `aws eks get-token` 或 GKE 的 `gke-gcloud-auth-plugin` ——
无法运行。请改用 [GitHub Releases](https://github.com/kbterm/kubeterm/releases/latest) 或
Homebrew 提供的 macOS 版本，这些版本不受沙盒限制：

```
brew install --cask kubeterm
```

OIDC 是个例外，详见[下面关于 OIDC 的问题](#如何连接使用-oidc-认证的集群)。


## 如何连接使用 OIDC 认证的集群？

Kubeterm 内置了 OIDC 支持，无需安装 `kubelogin` 可执行文件。选择该集群时会打开浏览器完成登录 ——
或在应用内窗口中登录，取决于你的「在外部浏览器中打开」设置。

以下两种 kubeconfig 写法 Kubeterm 都能识别。**`auth-provider` 形式：**

```yaml
users:
  - name: my-user
    user:
      auth-provider:
        name: oidc
        config:
          idp-issuer-url: https://accounts.google.com
          client-id: <client-id>
          client-secret: <client-secret>   # 可选，参见下方 PKCE 说明
```

**`exec` + kubelogin 形式** —— Kubeterm 能识别 `kubectl oidc-login get-token`，并直接读取其中的参数，
而不会真正执行该命令。这也是它在沙盒版 App Store 构建中同样可用的原因：

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

最少需要提供 client ID，并且**二选一**：要么提供 issuer URL（此时会通过
`/.well-known/openid-configuration` 自动发现各端点），要么显式指定授权端点和 token 端点
（`idp-authorization-endpoint` / `idp-token-endpoint`，exec 形式下为 `--oidc-authorization-endpoint`
/ `--oidc-token-endpoint`）。

此外还支持：`extra-scopes`、用于私有 CA 签发证书的 IdP 的 `idp-certificate-authority-data`、
`use-access-token`、`insecure-skip-tls-verify`，以及回调地址覆盖（`redirect-url`，exec 形式下为
`--listen-address` / `--oidc-redirect-url-hostname`）。

注意事项：

- **client secret 是可选的。** Kubeterm 始终使用 PKCE，这对那些强制轮换 client secret 的 IdP 很有帮助。
- **exec 形式必须是 `command: kubectl`**，且参数中包含 `oidc-login` 和 `get-token` —— 也就是
  kubelogin 官方文档生成的写法。其他写法不会被识别为 OIDC。
- **Google：** 会自动附加 `access_type=offline`，以确保能获取到 refresh token。


## 如何启用 Prometheus 查看历史指标？

集群仪表盘展示的是来自 Metrics Server 的实时数据。接入 Prometheus 后，可以额外查看 Pod 和节点的历史曲线。
该配置按集群单独设置：

1. 打开集群的详情页，选择 **Prometheus**。
2. 点击 **Auto-detect from cluster**（从集群自动探测）。Kubeterm 会查找 Prometheus Operator 相关资源、
   带有 `app.kubernetes.io/name=prometheus` 或 `app=prometheus` 标签的 Service，以及
   `monitoring`、`prometheus`、`observability`、`kube-system`、`kube-prometheus-stack`
   这几个命名空间下的常见服务名，然后逐个探测可用性。
3. 选择一个已验证通过的结果，或者自行填写 URL：
   - `kube-proxy://<namespace>/<service>:<port>` —— 通过 Kubernetes API Server 代理访问，
     无需 Ingress，也无需端口转发。
   - `https://…` —— 适用于你本来就能直接访问的 Prometheus。
4. 按需配置认证方式（Bearer Token 或 Basic Auth），然后点击 **Test Connection** 测试并保存。

如果无法正常工作：

- **提示 "Permission denied — ask admin for services/proxy RBAC"** —— `kube-proxy://` 形式需要通过
  API Server 代理的权限：

  ```yaml
  - apiGroups: [""]
    resources: ["services/proxy"]
    verbs: ["get"]
  ```

- **节点图表为空，但 Pod 图表正常** —— 请将 **Node label** 从 `node` 改为 `instance`。
  某些 node_exporter 部署方式会按抓取目标地址而非节点名来打标签。
- **提示 "Reachable but not Prometheus"** —— 通常是该端口上运行着其他服务，或者严格 mTLS 的服务网格
  拒绝了来自 API Server 的连接。


## 集群仪表盘不显示 CPU 和内存指标。

指标数据来自 Kubernetes Metrics Server，而并非所有发行版都默认安装该组件。在集群中安装
`metrics-server` 后，仪表盘即可正常显示数据。


## AI 助手会发送哪些数据？发送到哪里？

仅发送处理该请求所需的上下文：当前选中的集群、资源详情，以及涉及的 YAML 或日志片段。这些数据会直接发送给你所配置的
AI 服务商，并使用你自己的 API key —— Kubeterm 不做任何中转。选择 Ollama 则所有数据都保留在本机。任何会修改集群的操作，
都需要你先明确授权。

关于哪些数据会离开你的设备，完整说明见[安全与隐私](README.zh-CN.md#安全与隐私)。


## 能否连接使用自签名证书或私有 CA 证书的集群？

可以 —— Kubeterm 会使用 kubeconfig 中的 CA 证书数据。如果连接仍然因 TLS 错误失败，请先确认已升级到最新版本，
然后附上错误信息[提交 issue](https://github.com/kbterm/kubeterm/issues)。


---

这里没有你要的答案？欢迎在 [Discord](https://discord.gg/Jv4zEEBMR2) 提问，或提交
[issue](https://github.com/kbterm/kubeterm/issues)。
