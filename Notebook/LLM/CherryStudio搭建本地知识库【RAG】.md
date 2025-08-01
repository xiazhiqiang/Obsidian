## 软件下载

- [下载 CherryStudio](https://www.cherry-ai.com/download)
- [下载 Ollama](https://ollama.com/download)
- [下载 LM Studio](https://lmstudio.ai/download)

## 模型下载

###  [ollama 下载模型](https://ollama.com/search)

- Embed model 下载：这里选择 ollama 中比较通用的 `nomic-embed-text` 下载

```sh
ollama pull nomic-embed-text:v1.5
```

- 【非必要】Rerank model 下载：

```sh
ollama pull qllama/bge-reranker-v2-m3:latest
```

### LM Studio 下载模型

- 通过软件搜索模型下载即可。
- 下载完成后，在开发者窗口开启服务，并加载模型。

## CherryStudio 添加模型

- 在设置中模型服务开启 LLM Studio，并添加模型。
- 在知识库中，新增知识库，选择上一步添加的 LLM Studio 模型，等待解析完成后，搜索测试一下关键词是否有召回结果。
- 在模型会话中，选择会话模型及本地知识库进行问答。
