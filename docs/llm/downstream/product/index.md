---
title: 产品形态
icon: lucide/shopping-cart
---

了解 AI 厂商提供的产品形态，往往是了解 AI 下游应用的有效手段。本文就以目前顶级 AI 公司提供的产品为例。

## Anthropic

[Anthropic 的产品](https://claude.com/product/overview) 均被命名为 Claude。

![Claude 产品矩阵](https://cdn.dwj601.cn/images/20260520212419808.png)

### Claude (Web)

网页应用，单纯的 Chat 功能。当然目前也逐渐更新出 Agent、多模态和文件读写等功能。

### Claude (App)

需要额外下载软件。支持复杂工作流。

### Claude (CLI)

大名鼎鼎的 [Claude Code](./claude-code.md)（GUI 这里不予介绍），需要下载 CLI，目前位于 Code Agent 第一梯队。

## OpenAI

OpenAI 的产品非常丰富，在 Web、App、CLI 的基础之上，还有 LaTeX 科研平台 Prism、AI 浏览器 Atlas 等等，但是最近逐渐被 Claude 的编程能力抢了风头，所以又将资源集中到了 Coding 能力上。

### ChatGPT (Web)

网页应用，单纯的 Chat 功能。

### ChatGPT (CLI)

大名鼎鼎的 Codex（GUI 这里不予介绍），需要下载 CLI，同样位于 Code Agent 第一梯队。

## Google

自 Google I/O 2026 以后，Google 的 AI 产品均被命名为 Antigravity。

### Antigravity (Web)

网页应用，单纯的 Chat 功能。目前了解到的有 Gemini Web、Google AI Studio。

### Antigravity (App)

需要额外下载软件，支持复杂工作流。同时支持非 Google 的模型（例如 Claude 和 GPT）

### Antigravity (IDE)

集成开发环境，fork 自 VSCode。

### Antigravity (CLI)

原本的 Gemini CLI，目前 Code Agent 能力相较于前两家略逊一筹，但是在我的印象里是早于前两者被研发出来的？

## FAQ

### 登录 Antigravity 显示地区被禁用

如果使用 Google 账号登陆 Antigravity 时遇到了类似以下的禁用信息：

> Your current account is not eligible for Antigravity, because it is not currently available in your location.

如果确认网络没有问题，那就是 Google 账号有问题。解决方案如下：

进入 [Google 服务条款](https://policies.google.com/terms) 页面查看当前 Google 账号的地区，如果不在可用地区里（例如中国），继续下面的操作。

进入 [账号关联地区更改请求](https://policies.google.com/country-association-form) 页面填写表单，请求更换当前 Google 账号的地区。

地区选择你节点的地址，可以使用 [ping0.cc](https://ping0.cc) 查看你的节点地址。

![地区选择](https://cdn.dwj601.cn/images/20260520193829589.png)

原因选择「其他原因」：

![选择其他原因](https://cdn.dwj601.cn/images/20260520193858853.png)

内容模板：

> To access US-only developer tools and early access programs required for my current project, I need to update my account region. Please assist me in changing my location to <你的节点地址>.

例如我的节点是美国华盛顿，就填写为：

> To access US-only developer tools and early access programs required for my current project, I need to update my account region. Please assist me in changing my location to Washington, USA.
