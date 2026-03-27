<div align="center">

<img src="https://cwd.js.org/icon.png" width="111" />

# CWD (Cloudflare Workers Discuss)

[![GitHub release](https://img.shields.io/github/v/release/anghunk/cwd?display_name=tag&style=flat-square)](https://github.com/anghunk/cwd/releases)
[![GitHub stars](https://img.shields.io/github/stars/anghunk/cwd?style=flat-square)](https://github.com/anghunk/cwd)
[![License](https://img.shields.io/github/license/anghunk/cwd?style=flat-square)](./LICENSE)
[![Docs](https://img.shields.io/badge/Docs-cwd.js.org-brightgreen?style=flat-square)](https://cwd.js.org)

![Cloudflare Workers](https://img.shields.io/badge/Cloudflare-Workers-F38020?logo=cloudflare&logoColor=white&style=flat-square)
![Cloudflare D1](https://img.shields.io/badge/Cloudflare-D1-F38020?logo=cloudflare&logoColor=white&style=flat-square)
![Node.js](https://img.shields.io/badge/node-%3E=20.0.0-339933?logo=node.js&logoColor=white&style=flat-square)

基于 Cloudflare Workers 与全球边缘网络的免服务器、极速安全、即插即用评论系统。

数据存储在 Cloudflare D1 数据库中，通过 Worker 与数据库交互。

[使用文档](https://cwd.js.org) · [社区交流](https://zs.discourse.group)

</div>

## 部署到 Cloudflare

这个仓库的后端位于 [`cwd-api`](./cwd-api)，默认就是 Cloudflare Workers + D1 + KV 的部署形态。

### 需要准备

- 一个 Cloudflare 账号
- 一个 D1 数据库
- 一个 KV Namespace
- `ADMIN_NAME` 和 `ADMIN_PASSWORD` 两个后台登录变量

### 推荐流程

1. 进入后端目录

```bash
cd cwd-api
```

2. 安装依赖

```bash
npm install
```

3. 登录 Cloudflare 并创建资源

```bash
npx wrangler login
npx wrangler d1 create CWD_DB
npx wrangler d1 execute CWD_DB --remote --file=./schemas/comment.sql
npx wrangler kv namespace create CWD_AUTH_KV
```

4. 把 `cwd-api/wrangler.jsonc` 里的 `database_id` 和 KV `id` 改成你自己的资源 ID

5. 使用 Cloudflare secrets 写入 `ADMIN_NAME`、`ADMIN_PASSWORD`

```bash
npx wrangler secret put ADMIN_NAME
npx wrangler secret put ADMIN_PASSWORD
```

6. 部署 Worker

```bash
npm run deploy
```

### 评论端接入

评论组件的接入方式和参数说明见 [`docs/guide/frontend-config.md`](./docs/guide/frontend-config.md)。

如果你只是要把 CWD 作为网站评论系统，核心就是：

- 后端 API 部署到 Cloudflare Workers
- 网站前端引入 `cwd-widget`
- `apiBaseUrl` 指向你部署后的 Worker 地址
- 站点多语言或多路径时，用 `postSlug` 做聚合控制

## Preview

**评论端**
![](https://github.com/user-attachments/assets/34096ce5-512d-4ddb-b409-edef6a2674ed)


**后台管理**
![](https://github.com/user-attachments/assets/7592e339-3a4f-4dd2-a71c-ff6d97621fd9)

<details><summary>更多截图</summary>
<p>

![](https://github.com/user-attachments/assets/59ef1a42-75f7-4efa-8a0c-beae71322484)
![](https://github.com/user-attachments/assets/2bffdfaf-4af4-4bd8-929f-a178bc4acb78)
![](https://github.com/user-attachments/assets/37cff92b-d6c7-4d1e-a9dd-856bac6ec8d3)

</p>
</details> 

## Thanks

感谢以下项目和人员的贡献：

- [Cloudflare Workers](https://workers.cloudflare.com/)
- [Cloudflare D1](https://www.cloudflare.com/products/database/)
- [Momo-Backend](https://github.com/Motues/Momo-Backend)

## LICENSE

开源协议：[Apache-2.0](./LICENSE)
