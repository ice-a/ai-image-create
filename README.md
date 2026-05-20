# GPT-Image-2 Web Studio

<div align="center">

![GPT-Image-2 Web Studio](https://img.shields.io/badge/Vue.js-3.5.13-brightgreen.svg)
![Ant Design Vue](https://img.shields.io/badge/Ant%20Design%20Vue-4.2.6-blue.svg)
![Vite](https://img.shields.io/badge/Vite-6.4.2-orange.svg)
![License](https://img.shields.io/badge/license-MIT-green.svg)

一个基于 Vue 3 和 Ant Design Vue 构建的现代化 AI 图像生成 Web 应用

[English](#english) | [中文](#中文)

</div>

---

## ✨ 特性

- 🎨 **直观的图像生成界面** - 简洁美观的 UI 设计
- 🤖 **GPT-Image-2 支持** - 专为 GPT-Image-2 模型优化的生成参数
- 📱 **响应式设计** - 完美适配桌面和移动设备
- 🔧 **灵活配置** - 支持自定义 API 端点和密钥
- 🖼️ **实时预览** - 即时查看生成的图像结果
- 💾 **便捷下载** - 一键下载生成的图像
- 📋 **提示词复制** - 快速复制提示词到剪贴板

## 🛠️ 技术栈

- **前端框架**: Vue 3
- **UI 组件库**: Ant Design Vue 4.2.6
- **构建工具**: Vite 6.4.2
- **编程语言**: JavaScript (ES Module)

## 🚀 快速开始

### 环境要求

- Node.js 18.0.0 或更高版本
- npm 或 yarn 包管理器

### 安装步骤

1. **克隆仓库**
   ```bash
   git clone https://github.com/your-username/gpt-image-2-studio.git
   cd gpt-image-2-studio
   ```

2. **安装依赖**
   ```bash
   npm install
   # 或使用 yarn
   yarn install
   ```

3. **启动开发服务器**
   ```bash
   npm run dev
   # 或使用 yarn
   yarn dev
   ```

4. **构建生产版本**
   ```bash
   npm run build
   # 或使用 yarn
   yarn build
   ```

5. **预览生产构建**
   ```bash
   npm run preview
   # 或使用 yarn
   yarn preview
   ```

## 📖 使用说明

### 基础配置

1. **设置 API 信息**
   - **API Base URL**: 输入您的 GPT-Image-2 API 端点（例如：`https://api.openai.com/v1`）
   - **API Key**: 输入您的 API 密钥
   - 点击 "Fetch Models" 获取可用模型列表

2. **选择模型**
   - 从下拉列表中选择合适的图像生成模型
   - 默认推荐使用 `gpt-image-2` 模型

### 图像生成

1. **编写提示词**
   - 在 "Prompt" 文本框中描述您想要生成的图像
   - 示例：`A cinematic portrait of a cyberpunk fox in neon rain, ultra-detailed`

2. **配置生成参数**
   - **尺寸（Size）**: 选择图像尺寸（1024x1024、1536x1024、1024x1536、auto）
   - **质量（Quality）**: 设置生成质量（low、medium、high、auto）
   - **格式（Format）**: 选择输出格式（png、webp、jpeg）
   - **数量（Count）**: 设置生成图像数量（1-4张）

3. **生成图像**
   - 点击 "Generate" 按钮开始生成
   - 等待生成完成，图像将显示在右侧预览区域

### 结果操作

- **下载图像**: 点击 "Download" 按钮下载单张图像
- **复制提示词**: 点击 "Copy Prompt" 复制当前提示词到剪贴板

## 📁 项目结构

```
gpt-image-2-studio/
├── public/              # 静态资源
├── src/                 # 源代码
│   ├── App.vue          # 主应用组件
│   └── main.js          # 应用入口
├── index.html           # HTML 模板
├── package.json         # 项目配置和依赖
├── vite.config.js       # Vite 配置文件
└── README.md           # 项目文档
```

## 🔧 配置选项

### API 配置

应用支持自定义 OpenAI API 配置：

- **默认 API 端点**: `https://api.openai.com/v1`
- **认证方式**: Bearer Token
- **支持代理**: 可配置自定义代理服务器

### 生成参数

| 参数 | 可选值 | 默认值 | 说明 |
|------|--------|--------|------|
| Size | 1024x1024, 1536x1024, 1024x1536, auto | 1024x1024 | 图像尺寸 |
| Quality | low, medium, high, auto | medium | 生成质量 |
| Format | png, webp, jpeg | png | 输出格式 |
| Count | 1-4 | 1 | 生成数量 |

## 🤝 贡献指南

欢迎提交 Issue 和 Pull Request 来帮助改进项目！

1. Fork 项目
2. 创建功能分支 (`git checkout -b feature/amazing-feature`)
3. 提交更改 (`git commit -m 'Add some amazing feature'`)
4. 推送到分支 (`git push origin feature/amazing-feature`)
5. 提交 Pull Request

## 📄 许可证

本项目采用 MIT 许可证 - 查看 [LICENSE](LICENSE) 文件了解详情

## 🙏 致谢

- [Vue.js](https://vuejs.org/) - 渐进式 JavaScript 框架
- [Ant Design Vue](https://antdv.com/) - 企业级 UI 组件库
- [Vite](https://vitejs.dev/) - 下一代前端构建工具
- [OpenAI](https://openai.com/) - GPT-Image-2 模型

---

<div id="english">

# English Version

</div>

## ✨ Features

- 🎨 **Intuitive Image Generation Interface** - Clean and beautiful UI design
- 🤖 **GPT-Image-2 Support** - Optimized generation parameters for GPT-Image-2 model
- 📱 **Responsive Design** - Perfect adaptation for desktop and mobile devices
- 🔧 **Flexible Configuration** - Support for custom API endpoints and keys
- 🖼️ **Real-time Preview** - Instant viewing of generated image results
- 💾 **Convenient Download** - One-click download of generated images
- 📋 **Prompt Copy** - Quick copy prompts to clipboard

## 🛠️ Tech Stack

- **Frontend Framework**: Vue 3
- **UI Component Library**: Ant Design Vue 4.2.6
- **Build Tool**: Vite 6.4.2
- **Programming Language**: JavaScript (ES Module)

## 🚀 Quick Start

### Requirements

- Node.js 18.0.0 or higher
- npm or yarn package manager

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/your-username/gpt-image-2-studio.git
   cd gpt-image-2-studio
   ```

2. **Install dependencies**
   ```bash
   npm install
   # or using yarn
   yarn install
   ```

3. **Start development server**
   ```bash
   npm run dev
   # or using yarn
   yarn dev
   ```

4. **Build for production**
   ```bash
   npm run build
   # or using yarn
   yarn build
   ```

5. **Preview production build**
   ```bash
   npm run preview
   # or using yarn
   yarn preview
   ```

## 📖 Usage Guide

### Basic Configuration

1. **Set up API information**
   - **API Base URL**: Enter your GPT-Image-2 API endpoint (e.g., `https://api.openai.com/v1`)
   - **API Key**: Enter your API key
   - Click "Fetch Models" to get available model list

2. **Select Model**
   - Choose appropriate image generation model from dropdown
   - Default recommended model: `gpt-image-2`

### Image Generation

1. **Write Prompt**
   - Describe the image you want to generate in the "Prompt" textarea
   - Example: `A cinematic portrait of a cyberpunk fox in neon rain, ultra-detailed`

2. **Configure Generation Parameters**
   - **Size**: Choose image dimensions (1024x1024, 1536x1024, 1024x1536, auto)
   - **Quality**: Set generation quality (low, medium, high, auto)
   - **Format**: Select output format (png, webp, jpeg)
   - **Count**: Set number of images to generate (1-4)

3. **Generate Images**
   - Click "Generate" button to start generation
   - Wait for completion, images will appear in the preview area

### Result Operations

- **Download Image**: Click "Download" button to download individual images
- **Copy Prompt**: Click "Copy Prompt" to copy current prompt to clipboard

<div id="中文">

中文版本内容已在上方提供

</div>

---

**Made with ❤️ by the GPT-Image-2 Studio Team**
