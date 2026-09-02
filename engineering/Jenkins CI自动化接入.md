# 工程实践-Jenkins CI 自动化接入（案例模板）

> **用途**：对接示例企业 Jenkins（Remote Access API）实现 CI 镜像构建自动化——触发构建 → 提取镜像 tag → argocd 部署 → SLS 观察。可复用为任意服务 CI 抓手。
> **案例**：TICKET-002 example-service（2026-08-27 落地，脚本 `.claude/scripts/jenkins-ci-build.sh`）。
> **前置**：浏览器登录 Jenkins（`https://jenkins.example.com`，用 "Login with IDP" 跳企业统一认证）。

---

## 一、认证方式（3 种，推荐 API Token）

| 方式 | 配置 | 优点 | 缺点 |
|------|------|------|------|
| **API Token**（推荐） | 用户 `<user>` → **Security** → **API Token** → 添加新 Token → 生成；`curl -u 'user:token'` | 长期有效（30 天）、稳定、不随会话过期 | 需手动生成/续期 |
| **Cookie** | 浏览器 F12 → Jenkins 任意请求 → 复制 `Cookie` 头（JSESSIONID）| 即时可用 | **易过期**（登录态失效即 403/跳登录） |
| **SSO（Login with IDP）** | Jenkins 登录页 → "Login with IDP" → 企业统一认证（账号 + 密码不落盘）| 无需密码存储 | 仅浏览器交互，不适合脚本 |

> ⚠️ **Cookie 会过期**：重登后 F12 刷新；脚本用 API Token 最稳（认证优先级：API Token > Cookie，见脚本）。

## 二、常用 API 清单（Jenkins Remote Access API 2.x）

| 端点 | 方法 | 用途 |
|------|:---:|------|
| `/job/{job}/api/json?tree=name,buildable,property[parameterDefinitions[name]]` | GET | 查 job 状态 + 参数定义 |
| `/job/{job}/buildWithParameters` | POST | 触发构建（成功返回 302/303 重定向或 201） |
| `/job/{job}/lastBuild/api/json?tree=number,building,result` | GET | 最新构建状态 |
| `/job/{job}/{n}/consoleText` | GET | 构建日志（**提取镜像 tag**） |
| `/job/{job}/{n}/stop` | POST | 取消构建 |
| `/crumbIssuer/api/json` | GET | CSRF crumb（Jenkins 未启用则返回空，可省） |
| `/user/{user}/security/` | GET | **API Token 管理页**（生成/删除） |

> 认证参数：API Token 用 `-u user:token`；cookie 用 `-b "COOKIE"`。

## 三、构建参数（ci_fs_example-service 示例）

| 参数 | 值 | 说明 |
|------|-----|------|
| `BRANCH_NAME` | `qa-uat` | 构建分支（决定镜像版本） |
| `ENABLE_SONAR_CHECK` | `false` | **默认关**（快构建），需时开 |
| `WAIT_SONAR_RESULT` | `false` | 同上 |
| `ENABLE_UNIT_TEST` | `false` | 默认关 |
| `PUSH_TO_ALI` | `true` | **必开**（推 ACR 供 argocd 拉取） |

## 四、脚本（`.claude/scripts/jenkins-ci-build.sh`）

```bash
# 触发 qa-uat 构建 + 自动提取镜像 tag（--watch）
bash .claude/scripts/jenkins-ci-build.sh ci_fs_example-service qa-uat --watch
# 跳过 sonar 快速构建（默认已跳过，需 sonar 用 --sonar）
```

- 配置 `~/.jenkins-ci.env`（chmod 600）：`JENKINS_URL` / `JENKINS_USER` / `JENKINS_API_TOKEN`（或 `JENKINS_COOKIE`）
- 流程：查 job → `buildWithParameters`（BRANCH_NAME 等）→ 轮询 `lastBuild` → `consoleText` grep 镜像 tag（`qa-uat-x.x.x.x-commit`）
- 镜像 tag 拿到后：改 argocd `values-example-service.yaml` `Image.Tag` → push 触发部署 → SLS 观察

## 五、完整链路（TICKET-002 验证通过的流程）

```
改代码推 qa-uat → jenkins-ci-build.sh 触发 CI → 构建 #N SUCCESS → 提取镜像 tag
→ argocd 改 Image.Tag push → 自动部署 UAT → SLS 观察启动 → curl 业务验证
```

## 六、坑（BCI 沉淀）

| 坑 | 现象 | 解决 |
|----|------|------|
| **Cookie 过期** | crumb 返回登录重定向、job api 403 | 换 API Token（推荐）或刷新 cookie |
| `buildWithParameters` 误判 | 触发成功返回 302/303/201 | 脚本接受 200/201/302/303 为成功 |
| macOS `head -n -1` 不支持 | 提取 HTTP 码报错 | 用 `awk 'END{print}'` |
| eval 引号地狱 | 变量带乱码（`HTTP�`） | 不用 eval，直接 curl 数组参数 |

## 版本历史

| 日期 | 变更 | 变更人 |
|------|------|--------|
| 2026-08-27 | 新建：Jenkins CI 自动化接入案例模板（认证/API/脚本/链路/坑） | XingTu（AI 辅助） |
