sidebar_position: 5

# 简介

进迭时空 Spacemit Robotics 团队构建了统一的 Model Zoo 模型库，集成多种经过优化的深度学习模型与开箱即用的参考方案，覆盖视觉、多模态、语音识别、语音合成、大语言模型等多种应用场景。Model Zoo 的目标是：

- 为开发者提供 即取即用 的高性能模型与完整示例工程
- 在 Spacemit 平台上实现 端侧高效推理与部署
- 通过统一接口与文档，降低模型迁移与集成成本

目前 Model Zoo 主要包含以下组件：

## 1. VISION 组件

仓库地址：<https://github.com/spacemit-robotics/model-zoo-vision>
本组件为 SpacemiT 计算机视觉模型部署库，基于 ONNX Runtime，在 C++ 与 Python 下提供统一的推理与封装接口，便于集成到机器人、嵌入式与服务器等场景。功能特性如下：

| 类别     | 支持                                                         |
| -------- | ------------------------------------------------------------ |
| 模型类型 | 检测、分类、分割、跟踪、人脸、姿态等，一套接口接入           |
| 后端     | ONNX Runtime（含 SpaceMIT 等定制运行时）                     |
| 接口     | C++（`vision_service.h` 类接口，`cv::Mat` 入参）、Python（ModelFactory / 各模型类） |
| 可扩展   | 抽象基类与公共工具（预处理、NMS、绘图等）便于接入新模型      |

## 2. LLM 组件

仓库地址：<https://github.com/spacemit-robotics/model-zoo-llm>
本组件是方便开发者快速基于进迭时空的平台体验大语言模型。同时组件基于llama.cpp引擎封装了一层通用的C++接口并提供的示例应用，方便用户基于API接口调用实现Ai Agent应用，其组件接口支持的功能特性如下：

| 类别     | 支持                                                         |
| -------- | ------------------------------------------------------------ |
| 推理方式 | 单轮 `complete()`、多轮 `chat()`；同步、异步 `complete_async()`、流式 `complete_stream()` / `chat_stream()` |
| 后端     | OpenAI 兼容 HTTP API（vLLM / SGLang / llama-server 等）；可扩展自定义 Backend |
| 高级能力 | 多轮对话、Tool Calling（OpenAI function calling 格式）       |
| 可观测性 | 延迟、调用次数等 `get_metrics()`                             |

## 3. ASR 组件

仓库地址：<https://github.com/spacemit-robotics/model-zoo-asr>

本组件为通用 ASR 封装，提供统一的 C++ 接口与 Python 绑定，支持本地与云端多种识别引擎，便于集成到 AI Agent 等应用中。当前已支持 SenseVoice（本地 ONNX），接口可扩展其他本地或云端后端。功能特性如下：

| 类别     | 支持                                                         |
| -------- | ------------------------------------------------------------ |
| 部署方式 | **本地**（如 ONNX 推理）、**云端**（可扩展 HTTP/API 等）     |
| 识别方式 | 文件/内存阻塞识别 `Call()`、`Recognize()`；流式识别 `Start()` + `SendAudioFrame()` + `Flush()` / `Stop()` |
| 后端     | 当前支持 SenseVoice（本地 ONNX）；接口可扩展其他本地/云端引擎 |
| 语言     | 中文、英文、日文、韩文、粤语、自动检测                       |
| 接口     | C++（`include/asr_service.h`）、Python（`spacemit_asr`）     |

## 4. TTS 组件

仓库地址：<https://github.com/spacemit-robotics/model-zoo-tts>
本组件为通用 TTS 封装，提供统一的 C++ 接口与 Python 绑定，支持本地多种合成引擎，便于集成到 AI Agent 等应用中。当前已支持 Matcha-TTS、Kokoro 等（本地 ONNX），接口可扩展其他后端。功能特性如下：

| 类别     | 支持                                                         |
| -------- | ------------------------------------------------------------ |
| 部署方式 | **本地**（ONNX 推理）                                        |
| 合成方式 | 文本阻塞合成 `Call()`、`CallToFile()`；流式合成 `StreamingCall()`、`StartDuplexStream()` |
| 后端     | MATCHA_ZH / MATCHA_EN / MATCHA_ZH_EN / KOKORO                |
| 语言     | 中文、英文、中英混合                                         |
| 接口     | C++（`include/tts_service.h`）、Python（`spacemit_tts`）     |

支持的后端：

| 后端         | 语言      | 采样率  | 状态 |
| ------------ | --------- | ------- | ---- |
| MATCHA_ZH    | 中文      | 22050Hz | ✓    |
| MATCHA_EN    | 英文      | 22050Hz | ✓    |
| MATCHA_ZH_EN | 中英混合  | 16000Hz | ✓    |
| KOKORO       | 中文/英文 | 24000Hz | ✓    |

## 5. VAD 组件

仓库地址：<https://github.com/spacemit-robotics/model-zoo-vad>
本组件为通用 VAD 封装，提供统一的 C++ 接口与 Python 绑定，支持本地语音活动检测，便于集成到 ASR、TTS、AI Agent 等应用中。当前已支持 Silero VAD（本地 ONNX），接口可扩展其他后端。功能特性如下：

| 类别     | 支持                                                         |
| -------- | ------------------------------------------------------------ |
| 部署方式 | **本地**（ONNX 推理）                                        |
| 检测方式 | 单帧检测 `Detect()`；流式检测 `Start()` + `SendAudioFrame()` + `Stop()` |
| 后端     | Silero VAD（当前）；接口可扩展 ENERGY、WEBRTC 等             |
| 接口     | C++（`include/vad_service.h`）、Python（`spacemit_vad`）     |

支持的后端与配置：Silero（默认，16kHz，窗口 512 样本）；阈值 `trigger_threshold`/`stop_threshold`、平滑等详见 [API.md](https://github.com/spacemit-robotics/model-zoo-vad/blob/main/API.md)。