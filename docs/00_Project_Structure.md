# 桌面互动软件 - 项目目录结构规范 (升级版)

项目根目录名称建议：`Desktop_Interact_App`

## 目录树结构视图

```text
Desktop_Interact_App/          # 项目根目录
│
├── main.py                    # 🟢 程序的唯一主入口（组装各模块并启动状态机）
├── .env                       # 🔴 环境变量配置文件（绝密！存放 API Key）
├── requirements.txt           # 📦 项目依赖清单（记录需要的第三方库）
├── .gitignore                 # 🙈 Git 忽略文件（防止上传密钥、缓存图片和临时录音）
│
├── docs/                      # 🗂️ 设计文档区（项目的工程图纸库）
│   ├── 00_Project_Structure.md      # 项目目录结构规范 (本文档)
│   ├── 01_Master_PRD.md             # 总文档 (产品愿景与核心交互流)
│   ├── 02_Screen_Capture.md         # 实时屏幕读取并交互文档
│   ├── 03_Vision_Context.md         # 视觉上下文与元数据定义文档
│   ├── 04_Prompt_Persona.md         # 提示词工程与角色设定文档
│   ├── 05_API_Design.md             # API 设计文档
│   ├── 06_Voice_Interaction.md      # 语音交互文档
│   ├── 07_State_Machine.md          # 核心控制流与状态机文档
│   ├── 08_Frontend_UI.md            # 前端设计文档
│   ├── 09_Memory_Management.md      # 记忆与数据生命周期文档
│   ├── 10_Tech_Stack.md             # 技术栈文档
│   ├── 11_Testing_Plan.md           # 测试文档
│   ├── 12_Project_Schedule.md       # 项目进度文档
│   ├── 13_Deployment.md             # 软件打包与发布文档
│   └── 14_Context_Notes.md          # 上下文补充说明文档
│
├── assets/                    # 🎨 静态资源与动态缓存区
│   ├── images/                # 存放 Q 版角色立绘、动画序列帧、UI 图标
│   ├── sounds/                # 存放系统提示音效
│   └── temp_cache/            # ⚠️ 临时缓存区 (存放不断生成的 current_screen.png 和 user_voice.wav，阅后即焚)
│
└── src/                       # 🧠 核心源代码区
    ├── __init__.py
    │
    ├── ui/                    # 🖥️ 前端展示模块
    │   ├── __init__.py
    │   ├── main_window.py     # 透明悬浮窗绘制、拖拽逻辑
    │   └── animator.py        # 控制 Q 版角色的表情与动作切换 (待机/说话/倾听)
    │
    ├── vision/                # 👁️ 视觉与屏幕感知模块 (新增核心)
    │   ├── __init__.py
    │   ├── screen_capture.py  # 负责定时全屏截图或区域截图
    │   └── context_parser.py  # 负责画面裁剪 (ROI) 与简单的 OCR 文本提取
    │
    ├── audio/                 # 🎙️ 语音处理模块
    │   ├── __init__.py
    │   ├── vad_engine.py      # VAD 语音端点检测（判断是否开口/闭口）
    │   └── audio_io.py        # 负责开启麦克风录音与扬声器播放
    │
    ├── api/                   # 🌐 大模型网络请求模块
    │   ├── __init__.py
    │   ├── vlm_client.py      # 视觉大模型接口 (发送截图+上下文，获取评价)
    │   ├── llm_client.py      # 语言大模型接口 (处理纯语音聊天的上下文)
    │   ├── stt_client.py      # 语音转文字请求
    │   └── tts_client.py      # 文字转语音请求
    │
    ├── core/                  # ⚙️ 中央控制枢纽
    │   ├── __init__.py
    │   ├── state_machine.py   # 控制 [休眠-截图-播报-监听] 状态轮转的交通指挥员
    │   ├── memory_manager.py  # 管理短期对话记忆和 Token 限制
    │   └── prompt_manager.py  # 负责组装 System Prompt 和视觉分析指令
    │
    └── utils/                 # 🧰 通用工具箱
        ├── __init__.py
        ├── logger.py          # 日志记录器（记录报错信息，方便排查 bug）
        └── config_loader.py   # 读取 .env 密钥和用户偏好设置（如截图间隔时间）