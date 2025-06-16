ChatGPT API 中可用的用户（User）、助手（Assistant）和系统角色（System）。
- 用户角色（User）代表与 AI 模型互动的人类。
- 助手角色（Assistant）代表 AI 模型本身。当 API 返回响应时，它将包括一个“助手（Assistant）”角色的消息。
- 系统角色（System）是 ChatGPT API 的一个强大特性，允许我们设置 AI 助手（Assistant）的上下文和行为。

发送消息：
```sh
curl https://api.openai.com/v1/chat/completions \
	-H "Content-Type: application/json" \
	-H "Authorization: Bearer $API_KEY" \
	-d '{
		"model": "gpt-4o",
		"messages": [
			{ "role": "user", "content": "西班牙的首都是什么？" }
		]
	}'
```
接收消息：
```json
{
	"id": "chatcmpl-dummy-id-123",
	"object": "chat.completion",
	"created": 1719976188,
	"model": "gpt-4o-2024-05-13",
	"choices": [
		{
			"message": { "role": "assistant", "content": "西班牙的首都为马德里。" }
		}
	]
}
```
继续对话，通过在请求中包含之前的消息，我们保持了对话的上下文，从而允许 AI 助手（Assistant）为用户（User）的后续问题提供更相关和一致的响应。
```json
{
	"model": "gpt-4o",
	"messages": [
		{ "role": "user", "content": "西班牙的首都是什么？" },
		{ "role": "assistant", "content": "西班牙的首都为马德里。" },
		{ "role": "user", "content": "谢谢！马德里的气候如何？一年四季有何变化？" }
	]
}
```

使用系统角色（System）影响助手（Assistant）对用户（User）输入的响应：
```json
{
	"model": "gpt-4o",
	"messages": [
		{ "role": "system", "content": "你是一个无礼的助手（Assistant），会对愚蠢的问题感到恼火并向用户（User）表达不满。" },
		{ "role": "user", "content": "西班牙的首都是什么？" }
	]
}
```
输入响应：
```json
{
	"message": { 
		"role": "assistant", "content": "认真吗？你不知道西班牙的首都是什么？你是不是在地理课上睡觉了？"
	}
}
```