# Atoms Demo

一个类似 Atoms 的智能体驱动应用生成 demo。用户通过类 GPT 的 Builder 对话输入需求，系统会逐步追问项目目标、视觉风格、页面内容与功能范围，然后调用后端 AI 接口生成应用预览、代码文件结构、构建日志和可导出的项目文件。

## 功能概览

- 左侧 `Projects` 侧边栏：初始为空，完成一次生成后自动加入项目。
- 中间 Builder 对话：分阶段收集需求，包括项目目标、视觉风格、页面内容和功能范围。
- 右侧 Preview 工作区：仅在用户完成需求收集并生成项目后显示。
- `Preview / Code / Logs` 三视图：
  - `Preview` 展示 AI 生成的网站页面预览。
  - `Code` 展示生成文件树，包括 `README.md`、`build.json`、`frontend/package.json`、`frontend/index.html`、`frontend/src/App.jsx`、`frontend/src/styles.css`。
  - `Logs` 展示 AI 调用状态、Planner / Coder / Verifier 构建日志。
- 支持 `Enter` 发送，`Shift + Enter` 换行。
- 支持重新运行、移动端预览、导出 zip、发布状态更新。
- 后端支持 OpenAI-compatible API，未配置或调用失败时会使用 deterministic fallback，保证 demo 可用。

## 技术栈

前端：

- React
- Vite
- lucide-react
- CSS 原生布局与响应式适配
- Vercel 部署

后端：

- FastAPI
- Uvicorn
- Pydantic
- httpx
- Docker
- Hugging Face Space 部署

AI 接口：

- 支持 OpenAI-compatible `/chat/completions`
- 通过环境变量配置：

```bash
AI_API_BASE_URL=https://api.openai.com/v1
AI_API_KEY=your_api_key
AI_MODEL=gpt-4o-mini
```

## 项目结构

```text
.
├── atoms-frontend/              # Vercel 前端项目
│   ├── src/
│   │   ├── main.jsx             # React 主入口和核心交互
│   │   └── styles.css           # 页面样式
│   ├── index.html
│   ├── package.json
│   └── vercel.json
├── atoms-backend/               # Hugging Face 后端项目
│   ├── app.py                   # FastAPI API 与 AI 调用逻辑
│   ├── Dockerfile
│   ├── requirements.txt
│   └── README.md                # HF Space 配置
├── .github/workflows/
│   └── deploy-hf-backend.yml    # 自动部署后端到 Hugging Face
├── vercel.json                  # 从 repo 根目录部署 Vercel 的配置
├── package.json                 # 根目录 Vercel build 入口
└── ATOMS_DEMO_DEPLOYMENT.md     # 部署说明
```

## 后端 API

- `GET /`
- `GET /health`
- `GET /api/ai/status`
- `GET /api/templates`
- `GET /api/projects`
- `POST /api/builds`
- `POST /api/builds/rerun`
- `GET /api/builds/{build_id}`
- `GET /api/builds/{build_id}/export`
- `POST /api/publish`

核心接口是 `POST /api/builds`。它会接收用户 prompt、当前模板、构建模式和对话历史，调用 AI 生成：

- preview 页面数据
- React 代码
- 生成文件列表
- 构建日志
- AI 调用诊断信息

## 本地运行

后端：

```bash
cd atoms-backend
pip install -r requirements.txt
uvicorn app:app --reload --port 7860
```

前端：

```bash
cd atoms-frontend
npm install
npm run dev
```

前端本地环境变量 `atoms-frontend/.env.local`：

```bash
VITE_API_BASE_URL=http://localhost:7860
```

## Vercel 部署

可以从仓库根目录部署，根目录已经提供 `vercel.json`。

推荐配置：

```text
Install Command: cd atoms-frontend && npm install
Build Command: npm run build
Output Directory: atoms-frontend/dist
```

Vercel 环境变量：

```bash
VITE_API_BASE_URL=https://your-huggingface-space.hf.space
```

## Hugging Face 后端部署

后端部署为 Hugging Face Docker Space。

Space 根目录使用 `atoms-backend/` 内容，`atoms-backend/README.md` 顶部包含 Space 配置：

```yaml
sdk: docker
app_port: 7860
```

Hugging Face 环境变量：

```bash
FRONTEND_ORIGIN=https://your-vercel-project.vercel.app
AI_API_BASE_URL=https://api.openai.com/v1
AI_API_KEY=your_api_key
AI_MODEL=gpt-4o-mini
```

## GitHub Actions 自动部署后端

`.github/workflows/deploy-hf-backend.yml` 会在 `main` 分支中 `atoms-backend/**` 变化时，将后端目录推送到已有 Hugging Face Space。

GitHub Actions Secrets:

```text
HF_TOKEN=your_huggingface_token
```

GitHub Actions Variables:

```text
HF_USERNAME=your_huggingface_username
HF_SPACE_ID=your_huggingface_username/your_space_name
```

## 当前完成程度

已完成：

- 前后端分离。
- Vercel 前端部署配置。
- Hugging Face Docker 后端部署配置。
- GitHub Actions 后端自动部署工作流。
- 用户需求分阶段收集。
- AI 调用接口与诊断信息。
- Preview 渲染。
- Code 文件树视图。
- Logs 构建日志视图。
- 导出 zip。
- 发布状态模拟。

尚未完成或仍是 demo 级别：

- 当前生成代码主要是前端页面代码，不包含真实数据库、鉴权、后端业务 API 生成。
- Preview 是结构化数据驱动的模拟预览，不是真正运行生成项目后的 iframe 沙箱。
- 文件树是生成的示例项目结构，尚未支持完整多文件工程生成、安装依赖、运行构建和错误修复。
- 用户交互仍偏线性，尚不支持复杂多轮修改、局部编辑、选择页面元素后让 AI 修改。
- 发布功能目前是状态模拟，不是真正把生成项目发布到独立 URL。

## 后续扩展方向

优先级 1：

- 增强后端生成能力：让 AI 输出更完整的多文件项目，包括 routes、components、styles、mock data、API schema。
- 增加生成结果校验：检查 JSON schema、文件路径、代码语法，失败时自动要求模型修复。
- 支持编辑式交互：用户可以说“修改首页 hero”“添加登录页”“把风格改成深色”，系统基于已有 build 增量更新。

优先级 2：

- 增加真实预览沙箱：将生成代码写入临时项目，构建后通过 iframe 或静态产物展示。
- 增加后端应用生成：生成 FastAPI/Node API、数据库 schema、鉴权逻辑和接口文档。
- 增加项目版本历史：保存每次 build，支持回滚和对比。

优先级 3：

- 增加协作与发布：真实部署生成项目、分享链接、下载完整源码。
- 增加 UI 元素级编辑：点击预览中的组件后，右侧显示属性并支持 AI 修改。
- 增加模板市场：按 SaaS、电商、教育、Portfolio 等场景提供 starter。
