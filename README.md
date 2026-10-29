# Interactive HTML Skill

Turn any project's evidence into a **standalone, dependency-free interactive HTML dashboard** — with any agent harness, any model, or no model at all.

将任意项目的文档、代码状态、版本和任务数据生成一个可直接打开的交互式 HTML 页面。无需框架、CDN、后端、API Key 或模型服务。

## Features

- `cards` — architecture, modules, capabilities, and evidence details.
- `metrics` — before / after comparisons with honest measurement notes.
- `timeline` — releases, incremental improvements, and capability boundaries.
- `tasks` — progress, blockers, next actions, and status filters.
- `table` — semantic tables that become cards on narrow screens.
- Global search, hash navigation, dark / light themes, keyboard support, and reduced-motion support.
- One HTML file as output. No runtime dependencies.

## Quick start

Requirements: **Node.js 18+**.

```bash
git clone https://github.com/RichardDou0607/interactive-html-skill.git
cd interactive-html-skill
node scripts/test.mjs
node scripts/generate.mjs \
  --input examples/project.json \
  --output /tmp/acme-observatory.html
```

Open `/tmp/acme-observatory.html` in a browser, or open the included `examples/observatory.html` directly. All example content is explicitly labeled as synthetic.

You can also run:

```bash
npm test
npm run example
```

No `npm install` is needed because this repository has no package dependencies.

## Use with any Harness or model

- **Agent Skills**: install this directory in your host's skills directory, such as `.agents/skills/interactive-html/` or `.claude/skills/interactive-html/`.
- **Any model**: provide `SKILL.md` and `references/DATA_CONTRACT.md` as instructions/context, then let your Harness run the generator.
- **Function calling**: map `examples/tool-schema.json` to your Harness's command execution interface.
- **No model**: write the JSON yourself and run the generator directly.

See [Harness and model integration](docs/INTEGRATION.md) for complete instructions and a command-mapping example.

## Generate for your project

Create a JSON document following [the data contract](references/DATA_CONTRACT.md), then run:

```bash
node scripts/generate.mjs \
  --input /path/to/project-data.json \
  --output /path/to/observatory.html
```

The generator refuses to overwrite an existing file. Add `--force` only when replacing the generated file is intentional.

Open the result:

- macOS: `open /path/to/observatory.html`
- Windows: `start /path/to/observatory.html`
- Linux: `xdg-open /path/to/observatory.html`

## Repository layout

```text
SKILL.md                      Agent workflow and completion criteria
assets/template.html          Shared interactive page template
scripts/generate.mjs          JSON validation and HTML generation
scripts/test.mjs              Dependency-free self-test
references/DATA_CONTRACT.md   Input schema and evidence boundaries
docs/INTEGRATION.md           Harness / model integration
examples/project.json         Synthetic example covering all five views
examples/tool-schema.json     Vendor-neutral tool parameters
examples/observatory.html     Generated example page
```

## Evidence and safety

- Static JSON is a snapshot, not live telemetry. Regenerate it to update the page.
- Planned, unknown, estimated, and synthetic states must be labeled explicitly.
- Do not read or include `.env`, tokens, credential stores, or private keys.
- Validate real output in a browser: views, filters, search, theme, keyboard, and narrow layouts.

## License

MIT — see [LICENSE](LICENSE).
