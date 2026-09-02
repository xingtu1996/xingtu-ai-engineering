# 方法-编译测试模板

> 编译门禁 / 构建验证（Build/Compile Verification）· 提交前必过

## 定义

编译测试（Build/Compile Verification）：提交前用统一构建入口将改动模块编译为可运行产物，验证「编译零错误 + 依赖可解析 + 产物已生成」。它是测试金字塔最底层（L0 构建门禁）、Constitution §七 质量门禁第一项，比单元测试更早更机械——编译不过，后续测试无从谈起。

## 深入浅出

一句话本质：**编译零错误是提交底线，构建即第一道门禁**——机器用编译错误代替人眼审查，写错的代码当场拦下，不让坏代码进入测试/部署。

反模式：
- ❌ **裸 `mvn`**：默认 settings 缺内部 CAE 依赖、JDK 高版本致 Lombok 编译失败 → 必须走 `build.sh`
- ❌ **跳过编译直接部署**：改完不编译就提交，错误留到 CI/SIT 才暴露 → 本地 TDD 左移，编译过才合并（RED-8）

## 示例企业实践

- **统一入口**：`bash .claude/scripts/build.sh <mvn 参数>`（自动 JDK8 zulu-8 + settings-fs.xml → 阿里云 rdc 内部仓库）
- **编译命令**：`bash .claude/scripts/build.sh clean install -DskipTests`（单模块等价 mvn clean install，-DskipTests 专注编译）
- **依赖解析**：CAE 内部依赖从阿里云 rdc 拉取；失败先怀疑 settings 再怀疑缺包（memory/build-entry）
- **产物验证**：编译通过 `target/*.jar` 生成，可 `spring-boot:run` 启动 + curl 验证（RED-8：编译→启动→curl→合并）

## 骨架

1. **编译命令**：`build.sh clean install -DskipTests` → 观察 `BUILD SUCCESS/FAILURE`
2. **错误修复**：按 javac 定位（缺依赖/语法/注解/Lombok）→ 修复后重跑
3. **依赖检查**：确认无 `Could not resolve`；失败查 settings-fs / 内部仓库
4. **产物验证**：`ls target/` 确认 jar 生成，多模块逐一确认 install 成功

## 检查清单

□ `build.sh` 编译零错误（BUILD SUCCESS，非裸 mvn）？
□ 依赖解析成功（无 Could not resolve）？
□ 构建产物生成（target/*.jar 存在）？
□ 已编译验证未跳过（RED-8：未跳过编译直接部署）？

## 引用备案（背书双轨）

**外部权威**：

| 权威来源 | 作者/年 | URL | 要点 |
|---------|--------|-----|------|
| Maven 官方《Introduction to the Build Lifecycle》 | Apache Maven 项目 | https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html | 默认生命周期含 `compile` 阶段（绑定 `compiler:compile`），`mvn compile` 先执行全部前置阶段——编译是构建门禁的官方定义 |
| Maven Compiler Plugin 官方文档 | Apache Maven 项目 | https://maven.apache.org/plugins/maven-compiler-plugin/ | `compile`/`testCompile` 两 goal，编译错误当场拦下——"编译零错误是提交底线"的机器依据 |

**内部实证（示例企业落地）**：

- `.claude/scripts/build.sh` — 统一构建入口实证：固定 JDK8 zulu-8 + settings-fs.xml → 阿里云 rdc 内部仓库；裸 `mvn` 因缺内部 CAE 依赖 / JDK 高版本致 Lombok 编译失败（见根 CLAUDE.md 构建命令节）
- `.claude/rules/constitution.md` §七（质量门禁第一项 = 编译零错误）+ §三 RED-8（本地 TDD 左移：编译→启动→curl→合并）— 编译门禁已机械入宪法
- 用户级 memory/build-entry.md — 构建入口教训沉淀（依赖失败先怀疑 settings 再怀疑缺包）

## 版本历史

| 日期 | 版本 | 变更 |
|------|:---:|------|
| 2026-08-25 | V1.0 | 新建 |
