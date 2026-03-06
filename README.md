# ✨ LucidMark - 光影档案

一款为创作者打造的"划选即绘图"本地视觉助手。基于 Manifest V3 的 Chrome 扩展，支持多模型无缝切换，提供顶级审美的交互体验与本地资产管理。

## 🌟 核心特性

### 🎨 智能配图生成
- **上下文感知**：自动提取选中文本、网页标题、前后语境
- **AI 增强 Prompt**：将中文文本转化为专业级图像生成提示词，深度整合艺术风格
- **富文本支持**：保持表格、Markdown 等选中内容的格式显示
- **多模型双配置**：Gemini（单配置）/ Custom OpenAI（文本+图像双配置分离）
- **8 种风格预设**：电影感、吉卜力风、赛博朋克等

### 🖱️ 多维触发方式
- **快捷键**：`Cmd/Ctrl + Shift + L` 一键生成
- **右键菜单**：选中文本后右键"发送至 LucidMark"
- **自动捕获**：智能提取上下文信息

### 💎 极致审美体验
- **玻璃拟态 UI**：现代感的毛玻璃界面设计
- **微光动效**：优雅的 Shimmer Loading 动画
- **Framer Motion**：流畅的交互动效
- **响应式布局**：瀑布流画廊展示

### 🔐 安全与本地优先
- **零遥测**：无任何用户行为追踪
- **本地存储**：API Key 仅存储在 chrome.storage.local
- **IndexedDB**：图片以 Blob 格式本地存储，避免内存泄漏
- **密码安全**：API Key 显示为 `sk-...xxxx` 格式
- **删除保护**：所有删除操作都有二次确认弹窗
- **一键导出**：ZIP 打包所有生成图片

---

## 🚀 快速开始

### 1. 安装依赖

```bash
npm install
```

### 2. 开发模式

```bash
# Chrome
npm run dev

# Firefox
npm run dev:firefox
```

### 3. 构建生产版本

```bash
# Chrome
npm run build

# Firefox
npm run build:firefox
```

构建产物位于 `.output/chrome-mv3/` 目录。

### 4. 构建和加载扩展

```bash
# 清理和构建
npm run build

# 开发模式热重载
npm run dev
```

**加载扩展**：
1. 打开 Chrome，访问 `chrome://extensions/`
2. 开启"开发者模式"
3. 点击"加载已解压的扩展程序"
4. 选择 `.output/chrome-mv3/` 目录

**首次使用验证**：
1. 打开设置面板，正确配置 AI 服务
2. 验证 Custom OpenAI 双配置（Text+Image）的正确性
3. 尝试选中文本进行生成测试

### 5. 配置 API Key

1. 点击浏览器工具栏的 LucidMark 图标
2. 点击右上角设置按钮 ⚙️
3. 选择 Provider 并配置：
   - **Gemini**：只需填入 API Key
   - **Custom OpenAI**：
     - **文本模型配置**：Base URL、API Key、模型名称（如 gpt-4o）用于提示词优化
     - **图像模型配置**：Base URL、API Key、模型名称（如 dall-e-3）用于图像生成
     - ⚠️ 两部分都需要正确配置才能使用

---

## 📖 使用指南

### 方式一：快捷键触发
1. 在任意网页划选文本
2. 按下 `Cmd/Ctrl + Shift + L`
3. SidePanel 自动打开并开始生成

### 方式二：右键菜单
1. 在网页中选中文本
2. 右键点击 → "✨ 发送至 LucidMark 生成配图"
3. 等待生成完成

### 图片管理
- **1792x1024 宽屏生成**：原生高清大图，保留文字清晰度
- **中文优化支持**：增强中文文本在生成图像中的显示质量
- **富文本选中展示**：表格、Markdown 等内容格式不变
- **批量下载**：多选图片打包下载
- **预览删除**：图片预览页面提供详情查看和安全删除
- **一键下载**：列表页简易操作体验

---

## 🏗️ 技术架构

### 核心技术栈

| 类别 | 技术 | 特性 |
|------|------|------|
| 扩展框架 | WXT (Vite 驱动 MV3) | 现代化 Manifest V3 开发体验 |
| 前端视图 | React 19 + Tailwind CSS | 最新 React 特性，原子化样式 |
| 动效 | Framer Motion | 流畅的物理动效系统 |
| 状态管理 | Zustand + Persist Middleware | 轻量级状态管理，本地持久化 |
| 本地存储 | Dexie.js (IndexedDB) | 高效的图片 Blob 存储 |
| 文件处理 | JSZip + FileSaver.js | 批量图片打包下载 |
| AI SDK | @google/generative-ai | Gemini API 原生支持 |
| UI 组件 | Lucide React + 自定义 | 完整图标库，简约设计 |

### 项目结构

```
LucidMark/
├── entrypoints/
│   ├── background.ts          # Service Worker
│   ├── content.ts              # Content Script (划选监听)
│   └── sidepanel/              # SidePanel React 应用
│       ├── App.tsx
│       ├── main.tsx
│       └── style.css
├── src/
│   ├── core/
│   │   ├── ai/                 # AI Provider-Adapter 架构
│   │   │   ├── interfaces.ts
│   │   │   ├── AIProviderFactory.ts
│   │   │   └── adapters/
│   │   │       ├── GeminiAdapter.ts
│   │   │       └── CustomAdapter.ts
│   │   ├── ai-service.ts       # AI 服务封装
│   │   ├── db.ts               # Dexie 数据库
│   │   └── presets.ts          # 风格预设配置
│   ├── components/
│   │   ├── settings/           # 设置面板
│   │   ├── ui/                 # UI 组件
│   │   └── GenerationPanel.tsx # 生成流程面板
│   ├── store/                  # Zustand 状态管理
│   ├── types/                  # TypeScript 类型定义
│   └── utils/                  # 工具函数
└── wxt.config.ts               # WXT 配置
```

### AI Provider-Adapter 架构

支持灵活扩展多种 AI 模型，支持双配置分离架构：

```typescript
interface AIProvider {
  id: string;
  name: string;
  type: 'gemini' | 'custom';
  
  // Prompt 增强：深度整合风格信息到提示词
  enhancePrompt(text, context, style, credentials, language): Promise<string>;
  
  // 图片生成：独立配置图像生成服务
  generateImage(prompt, credentials): Promise<Blob>;
}
```

**提示词优化流程**：
1. 文本提取 + 风格信息整合 → AI 模型
2. AI 深度理解风格要求 → 生成风格融合的提示词
3. 智能后缀补充 → 增强特定视觉元素
4. 专业级图像生成提示词 → 图像模型

---

## 🎨 风格预设

| 风格 | 描述 | 提示词关键词 |
|------|------|--------------|
| 智能推断 | AI 自动判断风格 | - |
| 电影感 | 电影级画面质感 | cinematic lighting, dramatic atmosphere |
| 等距 3D | 等距视角 3D 插画 | isometric 3D render, minimal geometric |
| 极简矢量 | 扁平化矢量风格 | minimalist flat design, clean lines |
| 吉卜力风 | 宫崎骏动画风格 | Studio Ghibli style, hand-drawn |
| 赛博朋克 | 未来科技感 | cyberpunk, neon lights, futuristic |
| 水彩画 | 柔和水彩艺术 | watercolor painting, soft edges |
| 日系动漫 | 动漫绘画风格 | anime style, vibrant colors |

---

## 🔧 配置说明

### Gemini Provider

```typescript
{
  apiKey: string;  // Google AI Studio API Key
}
```

**使用的模型**：
- Prompt 增强：`gemini-2.0-flash`
- 图片生成：`gemini-2.0-flash-exp-image-generation`

### Custom OpenAI Provider（双配置分离）

```typescript
{
  textModel: {
    apiKey: string;      // 文本生成 API Key
    baseUrl: string;     // API Base URL (如 https://api.openai.com/v1)
    modelName: string;   // 文本模型 (如 gpt-4o、gpt-4-turbo)
  };
  imageModel: {
    apiKey: string;      // 图像生成 API Key（可能与文本相同）
    baseUrl: string;     // API Base URL（可能与文本相同或不同）
    modelName: string;   // 图像模型 (如 dall-e-3、stable-diffusion)
  }
}
```

**注意**：
- 两部分配置都需要正确填写才能使用 Custom OpenAI
- Base URL 必须以 `/v1` 结尾（OpenAI 兼容格式）
- 支持使用不同模型、不同 API 服务的技术栈分离

---

## � 本次开发更新日志（2026-03）

### 架构改进
1. **Custom OpenAI 双配置分离**：
   - 文本模型（Prompt 增强）和图像模型独立配置
   - 支持不同 Base URL、API Key、模型名称
   - 完整的两部分验证机制

2. **提示词优化重构**：
   - AI 深度理解并整合风格信息到提示词中
   - 避免生硬的风格后缀拼接
   - 防止返回空内容或无意义优化

### 用户体验改进
1. **富文本选择支持**：
   - HTML/Markdown 内容保持格式显示
   - 表格、代码块等特殊格式正确渲染

2. **图片质量提升**：
   - 1792x1024 宽屏高清图片生成
   - 增强中文文字的图像渲染清晰度
   - 保留原始尺寸下载

3. **操作安全优化**：
   - 删除操作二次确认弹窗
   - 列表页操作简化，专注下载功能
   - API Key 安全显示保护

### 代码质量
1. **TypeScript 类型安全增强**
2. **错误处理和边界条件完善**
3. **UI 组件状态管理优化**

---

## �📦 依赖说明

### 生产依赖
- `react` / `react-dom` - UI 框架
- `zustand` - 状态管理
- `dexie` / `dexie-react-hooks` - IndexedDB 封装
- `@google/generative-ai` - Gemini SDK
- `framer-motion` - 动效库
- `lucide-react` - 图标库
- `jszip` / `file-saver` - 文件打包导出
- `clsx` / `tailwind-merge` - 样式工具

### 开发依赖
- `wxt` - 扩展框架
- `typescript` - 类型检查
- `tailwindcss` / `@tailwindcss/postcss` - CSS 框架

---

## 🛡️ 技术特性与中文优化

### 隐私与安全
- ✅ **零遥测**：不收集任何用户数据
- ✅ **本地存储**：所有配置仅存储在本地
- ✅ **API Key 安全**：不显示明文，仅展示 `sk-...xxxx`
- ✅ **网络白名单**：仅允许请求配置的 API 端点

### 中文专项优化
- ✅ **中文文本提取**：准确捕获中文上下文信息
- ✅ **中文图像生成**：优化提示词确保中文文字清晰显示
- ✅ **中文字符支持**：增强图像生成模型的中文渲染能力
- ✅ **中文界面**：全中文本地化界面体验

### 稳定性和错误处理
- ✅ **双重验证**：Text+Image 配置完整验证
- ✅ **错误重试**：网络异常自动重试机制
- ✅ **内容安全**：防止返回空内容或无效优化
- ✅ **用户确认**：关键操作（删除）都有弹窗确认

---

## 📝 开发计划

- [ ] 划选悬浮标（Tooltip Logo）
- [ ] 图片预览放大
- [ ] Prompt 手动编辑
- [ ] 更多 AI 模型支持（Midjourney、Stable Diffusion）
- [ ] 图片缓存自动清理
- [ ] 深色/浅色主题切换
- [ ] 多语言支持

---

## 🤝 贡献

欢迎提交 Issue 和 Pull Request！

---

## 📄 License

MIT License

---

## 🙏 致谢

- [WXT](https://wxt.dev/) - 优秀的扩展开发框架
- [Google Gemini](https://ai.google.dev/) - 强大的 AI 能力
- [Framer Motion](https://www.framer.com/motion/) - 流畅的动效库
- [shadcn/ui](https://ui.shadcn.com/) - 设计灵感来源

---

<p align="center">
  Made with ❤️ by LucidMark Team
</p>