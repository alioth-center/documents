# Akasha Whisper

![Go 版本](https://img.shields.io/github/go-mod/go-version/alioth-center/akasha-whisper)
![版本发布](https://img.shields.io/github/v/release/alioth-center/akasha-whisper)
![Go 报告卡](https://goreportcard.com/badge/github.com/alioth-center/akasha-whisper)
![GitHub Actions](https://img.shields.io/github/actions/workflow/status/alioth-center/akasha-whisper/build-docker.yml?branch=main)
![许可证](https://img.shields.io/github/license/alioth-center/akasha-whisper)

## 概述

`akasha-whisper` 是一个用 Golang 开发的项目，提供统一、用户友好的 API，用于集成和交互多种 AI 模型。

与 [one-api](https://github.com/songquanpeng/one-api) 相似，`akasha-whisper` 提供了额外的功能：

- 动态的客户端权重配置和智能负载均衡。
- 支持每个模型的多提供商集成，允许接入多种来源（如 OpenAI 和 Azure），并基于预设的权重和价格配置自动选择提供商。

## 文档

详细文档请访问 [Akasha Whisper](https://docs.alioth.center/akasha-whisper.html)。

## 兼容 API

|      名称       |       描述        |           URL            | 状态  |
|:-------------:|:---------------:|:------------------------:|:---:|
|     Chat      | 通过给定提示生成完整的对话内容 |  `v1/chat/completions`   |  ✅  |
|     Model     |     列出可用模型      |       `/v1/models`       |  ✅  |
|  Embeddings   |  获取给定文本的嵌入向量表示  |     `/v1/embeddings`     |  ✅  |
|     Image     |    通过提示生成图片     | `v1/images/generations`  | WIP |
|    Speech     |   生成给定文本的语音音频   |    `v1/audio/speech`     | WIP |
| Transcription |  将给定音频文件转换为文本   | `v1/audio/transcription` | WIP |

## 支持模型

以下模型和提供商已由开发团队充分测试并确认可靠运行。

|   名称    |  提供商   |                              首页                              |                       基础 URL                        |
|:-------:|:------:|:------------------------------------------------------------:|:---------------------------------------------------:|
|   GPT   | OpenAI |                 [OpenAI](https://openai.com)                 |             `https://api.openai.com/v1`             |
|   GLM   |  智谱清言  |               [智谱AI](https://open.bigmodel.cn)               |       `https://open.bigmodel.cn/api/paas/v4`        |
|  qwen   |  阿里云   |              [通义千问](https://tongyi.aliyun.com)               | `https://dashscope.aliyuncs.com/compatible-mode/v1` |
| hunyuan |  腾讯云   | [混元大模型](https://cloud.tencent.com/act/pro/Hunyuan-promotion) |     `https://api.hunyuan.cloud.tencent.com/v1`      |

## 致谢

感谢 JetBrains 提供的 [开源开发许可证](https://www.jetbrains.com/community/opensource/#support)。

## 贡献者

![贡献者](https://contrib.rocks/image?repo=alioth-center/akasha-whisper&max=1000)
