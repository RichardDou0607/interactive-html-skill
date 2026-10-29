# Harness 与模型接入

这个仓库不绑定任何模型、Agent 或 Harness。它只要求 Node.js 18+；模型负责整理项目证据并生成 JSON，Harness 负责执行生成器，浏览器负责展示结果。

## 1. Agent Skills 宿主

保留仓库内的 `SKILL.md`、`assets/`、`scripts/`、`references/` 和 `examples/` 相对结构。

- Kimi Code / 兼容 `.agents` 的宿主：将本仓库目录放到项目的 `.agents/skills/interactive-html/`。
- Claude Code：将本仓库目录放到项目的 `.claude/skills/interactive-html/`。
- 其他 Harness：按宿主的 Skill 加载约定安装，或直接采用下面的“上下文 + 工具”方式。

安装后可以请求：

> 使用 interactive-html 为当前项目创建交互式 HTML 页面

## 2. 任意模型作为上下文输入

不依赖特定模型 API。向模型提供：

1. `SKILL.md`：执行流程和完成条件。
2. `references/DATA_CONTRACT.md`：JSON 数据结构。
3. 需要可视化的项目证据：README、manifest、文档、测试、发布记录、任务清单。

模型输出符合契约的 JSON 后，Harness 执行：

```bash
node scripts/generate.mjs --input /path/to/project-data.json --output /path/to/observatory.html
```

任何能读取 Markdown、生成 JSON 的模型都可参与；不需要模型供应商专用 SDK。

## 3. 通用函数调用 / 工具 Schema

`examples/tool-schema.json` 提供与厂商无关的参数结构。Harness 可以将工具调用映射为：

```text
input  → --input 的路径
output → --output 的路径
force  → 可选的 --force 参数
```

伪代码：

```js
const args = ['scripts/generate.mjs', '--input', input, '--output', output];
if (force === true) args.push('--force');
const result = await harness.runCommand('node', args);
```

`runCommand` 是示意接口，不是本仓库要求安装的库。实际 Harness 应使用自己的命令执行、权限审批和工作目录机制。

## 4. 无模型使用

开发者也可以直接编写 JSON 并运行生成器。页面无需模型、联网、API Key 或账户。

## 5. 数据与权限边界

- 静态 JSON 是快照，不是实时监控。需要更新时重新生成快照和 HTML。
- 规划、未知、估算和合成数据必须明确标注。
- 不为丰富页面而读取 `.env`、token、凭证库或私钥。
- 生成器默认拒绝覆盖已有文件，只有调用方明确允许时才传 `force`。
- 生成后由 Harness 或开发者打开实际 HTML，验证视图、筛选、搜索、主题和窄屏交互。
