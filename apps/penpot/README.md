# Penpot

Penpot 是面向产品团队的开源设计与原型协作平台，支持矢量设计、交互原型、设计系统和实时协作。

本应用使用官方 **2.18.6** 镜像，包含前端、后端、导出服务、管理控制台、MCP、PostgreSQL 15 和 Valkey 8.1，支持 amd64 和 arm64。

## 安装

1. 更新 1Panel 本地应用列表，找到 **Penpot** 并安装。
2. HTTP 端口默认 `9001`。访问地址必须填写浏览器实际使用的完整地址，例如 `http://192.168.1.100:9001` 或 `https://penpot.example.com`，不要使用容器内部地址。修改端口或域名时，也要同步修改访问地址。
3. 保留自动生成的数据库密码，并填写应用密钥。可在安装 1Panel 的服务器运行以下命令生成密钥：

   ```bash
   python3 -c 'import secrets; print(secrets.token_urlsafe(64))'
   ```

4. 启动后等待数据库初始化和迁移完成，再通过配置的访问地址打开 Penpot，注册第一个账号。没有预设登录账号。

## 功能开关与邮件

默认允许注册、免邮箱验证，支持 HTTP 登录，启用管理控制台、MCP 和管理命令接口。管理命令接口仅在应用专用网络内使用。

默认不启用 SMTP，无需配置邮件服务即可注册和登录。邀请新用户、发送密码重置邮件等功能需要填写 SMTP 服务器、端口、用户名、密码和发件人邮箱，并在“功能开关”中追加 `enable-smtp`。端口 `587` 通常使用 STARTTLS；端口 `465` 通常使用 SSL，按邮件服务商要求选择对应开关。

如需验证注册邮箱，配置可用的 SMTP 后，移除 `disable-email-verification`。如需关闭公开注册，在功能开关中追加 `disable-registration`。

## 域名与 HTTPS

在 1Panel 中为应用创建反向代理，指向应用的 HTTP 端口，并启用 WebSocket 转发。使用 HTTPS 域名后，将“访问地址”修改为对应的 `https://` 地址，同时从“功能开关”中移除 `disable-secure-session-cookies`。

导入较大文件时，反向代理的请求体大小上限应不低于 `350 MiB`（`367001600` 字节）。

## 数据与升级

- 数据库保存在 Compose 项目对应的 `penpot_postgres_v15` 命名卷中。
- 上传的图片、字体和设计资源保存在同一项目的 `penpot_assets` 命名卷中，由前端和后端共享。
- 升级前备份数据库和资源卷，同时保存应用的环境配置；仅备份应用安装目录不足以备份命名卷中的数据。具体操作参见[官方备份说明](https://help.penpot.app/technical-guide/getting-started/docker/#backup-penpot)。
- 保持数据库密码和应用密钥不变，按官方升级说明逐步升级。

## 官方链接

- [项目仓库](https://github.com/penpot/penpot)
- [Docker 部署说明](https://help.penpot.app/technical-guide/getting-started/docker/)
- [配置说明](https://help.penpot.app/technical-guide/configuration/)
