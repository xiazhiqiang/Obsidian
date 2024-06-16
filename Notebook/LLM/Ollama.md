- 是什么：ollama是一个开源的大型语言模型服务工具，它允许用户在自己的硬件环境中轻松部署和使用大规模预训练模型。
- [下载](https://ollama.com/download)

## 本地使用

- 命令行下载模型
```sh
ollama pull qwen2:7b
```
- 命令行chat模式，运行模型进行对话
```sh
ollama run qwen2:7b
```
- 命令行输入prompt，运行模型返回结果
```sh
ollama run qwen2:7b "中国在地球哪里？"
```
- 命令行输入promp包含文件，运行模型返回结果
```sh
ollama run qwen2:7b "总结一下文档$(cat test.txt)"
```
- 命令行运行模型结束后，展示执行效率的一些参数
```sh
ollama run qwen2:7b "prompt" --verbose
```

## 高级使用

- API调用使用
```sh
# 流式输出
curl http://localhost:11434/api/chat \
-d '{
	"model": "qwen2:7b",
	"messages": [
		{
			"role": "user",
			"content": "中国在地球哪里？"
		}
	]
}'
```
```sh
# 非流式输出
curl http://localhost:11434/v1/chat/completions \
-H "Content-Type: application/json" \
-d '{
	"model": "qwen2:7b",
	"messages": [
		{
			"role": "user",
			"content": "中国在地球哪里？"
		}
	]
}'
```
- 查看模型文件
```sh
ollama show qwen2:7b --modelfile
```
- 基于模型，修改参数，并生成新的模型
```gguf
// modelFile
FROM qwen2:7b
SYSTEM "以女主播的口吻作答。"
PARAMETER temperature 0.1
```
```sh
ollama create qwen2-7b-custom -f ./modelFile
```