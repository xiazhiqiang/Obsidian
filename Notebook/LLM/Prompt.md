https://zhuanlan.zhihu.com/p/628254170

## 提示词工程指南

https://www.promptingguide.ai/zh

提示词可以包含以下任意要素：
- **指令**：想要模型执行的特定任务或指令。
- **上下文**：包含外部信息或额外的上下文信息，引导语言模型更好地响应。
- **输入数据**：用户输入的内容或问题。
- **输出要求**：指定输出的类型或格式。

提示越具描述性和详细，结果越好。特别是当对生成的结果或风格有要求时，这一点尤为重要。
不存在什么特定的词元（tokens）或关键词（tokens）能确定带来更好的结果。
事实上，在提示中提供良好格式和描述性的提示词对于获得特定格式的期望输出非常有效。
- 避免不明确：一定要要简明直接
- 避免说不要做什么，而要直接说要做什么

### 提示词技术

- 零样本提示：无需提供样本示例，直接进行任务和输入；当零样本不起作用时，建议在提示中提供演示或示例，这就引出了少样本提示。
- 少样本提示：在提示中提供演示以引导模型实现更好的性能；但在处理更复杂的推理任务时，不总是有效。
- 链式思考（CoT）：提示通过中间推理步骤实现了复杂的推理能力。
	- 零样本COT：将“让我们逐步思考”添加到原始提示中，大模型会进行链式思考。
	- 自动思维连（Auto-Cot）
- 自我一致性：通过少样本 CoT 采样多个不同的推理路径，并使用生成结果选择最一致的答案。
- 生成知识提示
![[Pasted image 20250615173844.png]]

![[Pasted image 20250622213109.png]]
通过RAG方式，一个典型的提示词模板为：`请根据以下信息回答用户的问题：{召回文本段}。用户的问题是：{question}。`

## ChatGPT Prompt Engineering for Developers

https://learn.deeplearning.ai/courses/chatgpt-prompt-eng/lesson/dfbds/introduction

Principle1：写法清晰且明确的说明
- 使用分割线：

```
总结以下文本。
文本：
'''
文本内容
'''
```

- 结构化输出格式：

```
生成一个书列表。通过json格式返回，字段名为：book_id,title,genre.
```

- 检查条件是否满足：

```
<任务描述>

如果内容不包括步骤，则返回没有步骤提供。
```

- Prompt给出成功示例

Principle2：给模型时间去思考（思考过程）
- 具体说明完成任务所需要的步骤

```
<任务描述>

步骤1：
步骤2：
```

## 其他资料

- https://qianduan.shop/blogs/detail/245
- [chatgpt-prompt-framework](https://learningprompt.wiki/docs/chatGPT/tutorial-extras/chatGPT-prompt-framework)
- [提示词技巧](https://dye87dshnj.feishu.cn/wiki/Ett8wud9KiagqYkJzOJcXRiVnuf)
- [github好的提示词学习](https://github.com/EmbraceAGI/awesome-chatgpt-zh/blob/main/docs/ChatGPT_prompts.md)

## 工具

- https://github.com/linshenkx/prompt-optimizer/tree/master
- [中文prompt调教](https://github.com/PlexPt/awesome-chatgpt-prompts-zh/blob/main/prompts-zh.json)
