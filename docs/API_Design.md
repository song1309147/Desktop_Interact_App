# 桌面互动软件 - API 接口设计与调用规范

## 1. 核心网络架构
本系统属于典型的“重前端、轻本地、调云端”架构。本地 Python 客户端主要负责设备控制（麦克风/扬声器）和 UI 渲染，繁重的 AI 计算全部通过 HTTP/WebSocket 请求交由云端大模型处理。

**推荐使用的 Python 核心网络库：** `requests` (用于同步请求) 或 `httpx` / `aiohttp` (用于异步请求，防止界面卡顿)。

## 2. API 调用链路全景
在“流畅模式”下，完成一次对话需要顺序调用三个核心 API：

1. **[录音文件]** ──▶ **STT (语音转文字) API** ──▶ 返回: `用户说的话 (String)`
2. **[用户说的话]** ──▶ **LLM (大语言模型) API** ──▶ 返回: `AI 的回复内容 (String)`
3. **[AI 的回复内容]** ──▶ **TTS (文字转语音) API** ──▶ 返回: `音频流 (Audio Buffer)`

---

## 3. 接口详细定义

### 3.1 核心大脑：大语言模型 (LLM) 接口
*由于你之前成功跑通了 DeepSeek，且目前大多数模型（含 Qwen）都兼容 OpenAI 的接口格式，这里采用通用 Chat Completions 标准。*

* **请求方式：** `POST`
* **目标 URL：** `https://api.deepseek.com/v1/chat/completions` (以 DeepSeek 为例)
* **Header 请求头：**
  * `Content-Type`: `application/json`
  * `Authorization`: `Bearer YOUR_API_KEY` (千万不要把真实 Key 写死在代码里，用 `.env` 文件读取)
* **Body 请求体 (JSON)：**
  ```json
  {
    "model": "deepseek-chat",
    "messages": [
      {"role": "system", "content": "你是一个傲娇但聪明的桌面游戏助手。你的回复必须简短，适合语音朗读。"},
      {"role": "user", "content": "哎呀，我刚才那波操作又失误了！"}
    ],
    "temperature": 0.7,
    "max_tokens": 150
  }