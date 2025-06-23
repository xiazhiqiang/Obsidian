## OpenAI使用
### http方式：

```python
from openai import OpenAI
import os
client = OpenAI(
    api_key=os.getenv("DASHSCOPE_API_KEY"),
    base_url="https://dashscope.aliyuncs.com/compatible-mode/v1",
)
def get_qwen_response(prompt):
    response = client.chat.completions.create(
        model="qwen-plus-0919",
        messages=[
            # system message 用于设置大模型的角色和任务
            {"role": "system", "content": "你负责教育内容开发公司的答疑，你的名字叫公司小蜜，你要回答同事们的问题。"},
            # user message 用于输入用户的问题
            {"role": "user", "content": prompt}
        ]
    )
    return response.choices[0].message.content
response = get_qwen_response("我们公司项目管理应该用什么工具")
print(response)
```

### stream方式：
```python
from openai import OpenAI
import os
client = OpenAI(
    api_key=os.getenv("DASHSCOPE_API_KEY"),
    base_url="https://dashscope.aliyuncs.com/compatible-mode/v1",
)
def get_qwen_response(prompt):
    response = client.chat.completions.create(
        model="qwen-plus-0919",
        messages=[
            # system message 用于设置大模型的角色和任务
            {"role": "system", "content": "你负责教育内容开发公司的答疑，你的名字叫公司小蜜，你要回答同事们的问题。"},
            # user message 用于输入用户的问题
            {"role": "user", "content": prompt}
        ]
    )
    return response.choices[0].message.content
response = get_qwen_response("我们公司项目管理应该用什么工具")
print(response)
```

### 多轮对话方式：
```python
import os
from openai import OpenAI


def get_response(messages):
    client = OpenAI(
        # 若没有配置环境变量，请用阿里云百炼API Key将下行替换为：api_key="sk-xxx",
        api_key=os.getenv("DASHSCOPE_API_KEY"),
        base_url="https://dashscope.aliyuncs.com/compatible-mode/v1",
    )
    # 模型列表：https://help.aliyun.com/zh/model-studio/getting-started/models
    completion = client.chat.completions.create(model="qwen-plus", messages=messages)
    return completion

# 初始化一个 messages 数组
messages = [
    {
        "role": "system",
        "content": """你是一名阿里云百炼手机商店的店员，你负责给用户推荐手机。手机有两个参数：屏幕尺寸（包括6.1英寸、6.5英寸、6.7英寸）、分辨率（包括2K、4K）。
        你一次只能向用户提问一个参数。如果用户提供的信息不全，你需要反问他，让他提供没有提供的参数。如果参数收集完成，你要说：我已了解您的购买意向，请稍等。""",
    }
]
assistant_output = "欢迎光临阿里云百炼手机商店，您需要购买什么尺寸的手机呢？"
print(f"模型输出：{assistant_output}\n")
while "我已了解您的购买意向" not in assistant_output:
    user_input = input("请输入：")
    # 将用户问题信息添加到messages列表中
    messages.append({"role": "user", "content": user_input})
    assistant_output = get_response(messages).choices[0].message.content
    # 将大模型的回复信息添加到messages列表中
    messages.append({"role": "assistant", "content": assistant_output})
    print(f"模型输出：{assistant_output}")
    print("\n")
```


## 大模型意图识别

借助大模型先进行意图识别：对用户的问题进行分类，如果是文档审查、内容翻译等任务，会直接输入给大模型来生成答案，如果是内部知识查询问题，才经过RAG 链路来生成答案。

大模型进行意图识别也有以下两种方法：

- 使用提示词：通过设计特定的提示词，引导大模型生成符合预期的回答。这种方法不需要修改模型本身的参数，而是依靠构造的输入来激发模型内部已有的知识。
- 对模型进行微调：基于一个经过预训练的基础模型，使用特定的标注数据进一步训练该模型，使其更好的对意图进行分类。微调涉及调整模型的部分或全部参数。

意图识别带来的好处：

- 节省资源：对于检查文档错误的问题，大模型其实可以直接回复，并不需要检索参考资料，之前的实现存在资源浪费。
- 避免误解：之前的实现每次会检索参考资料，这些被召回的相关文本段可能会干扰大模型理解问题，导致答非所问。

## 推理大模型

推理模型相较于通用大模型多出了“`思考过程`”，就像解数学题时有人会先在草稿纸上一步步推导，而不是直接报答案，减少模型出现“拍脑袋”的错误，同时在分步思考过程中，如果某一步骤发现矛盾，还可以回头检查并重新调整思路，展示推理步骤还可以方便人们理解，顺着模型的思考路线验证逻辑。  
相较于通用大模型，推理大模型通常在解决复杂问题时更可靠，比如在数学解题、代码编写、法律案件分析等需要严谨推理的场景。

推理模型还是通用模型？如何选择？以下是一些推荐：

- **明确的通用任务**：对于明确定义的问题，**通用模型**一般能够很好地处理。
- **复杂任务**：对于非常复杂的任务，且需要给出相对**更精确和可靠**的答案，推荐使用**推理模型**。这些任务可能有：
    - 模糊的任务：任务相关信息很少，你无法提供模型相对明确的指引。
    - 大海捞针：传递大量非结构化数据，提取最相关的信息或寻找关联/差别。
    - 调试和改进代码：需要审查并进一步调试、改进大量代码。
- **速度和成本**：一般来说推理模型的推理时间较长，如果你对于时间和成本敏感，且任务复杂度不高，**通用模型**可能是更好的选择。
还可以在你的应用中结合使用两种模型：使用推理模型完成Agent的规划和决策，使用通用模型完成任务执行。推理模型进行“慢思考”完成规划或推理，通用模型进行“快思考”或调用工具完成指定动作。

## 自动化评测

- 查看 RAG 检索结果排查问题：确认大模型回答问题前，召回的参考资料含有相关资料。
- 大模型可以自动检测是否有效地回答用户问题：只要在提示词中提供参考信息并限制回答样式即可。
```python
from chatbot import llm

def test_answer(question, answer):
    prompt = ("你是一个测试人员。\n"
        "你需要检测下面的这段回答是否有效回答了用户的问题。\n"
        "回复只能是：有效回答 或者 无效回答。请勿给出其他信息。\n"
        "------"
        f"回答是 {answer}"
        "------"
        f"问题是： {question}"
    )
    return llm.invoke(prompt,model_name="qwen-max")

test_answer("张伟是哪个部门的", "根据提供的信息，没有提到张伟所在的部门。如果您能提供更多关于张伟的信息，我可能能够帮助您找到答案。")
```
- 在RAG应用中，除了回答的有效性，你还需要确保检索到的参考信息是否有用。

自动化评测指标：

- **召回质量 (Retrieval Quality)**： RAG系统是否检索到了正确且相关的文档片段？
- **答案忠实度 (Faithfulness)**： 生成的答案是否完全基于检索到的上下文，没有“胡编乱造”（幻觉）？
- **答案相关性 (Answer Relevance)**： 生成的答案是否准确地回答了用户的问题？
- **上下文利用率/效率 (Context Utilization/Efficiency)**： 大模型是否有效地利用了所有提供给它的上下文信息？（这与我们之前提到的“Lost in the Middle”和“知识浓度”密切相关）

- 整体回答质量的评估：
    - Answer Correctness，用于评估 RAG 应用生成答案的准确度。
- 生成环节的评估：
    - Answer Relevancy，用于评估 RAG 应用生成的答案是否与问题相关。
    - Faithfulness，用于评估 RAG 应用生成的答案和检索到的参考资料的事实一致性。
- 召回阶段的评估：
    - Context Precision，用于评估 contexts 中与准确答案相关的条目是否排名靠前、占比高（信噪比）。
    - Context Recall，用于评估有多少相关参考资料被检索到，越高的得分意味着更少的相关参考资料被遗漏。

- Groundedness：生成的答案是否是来自于检索召回的知识
- Answer Relevance：生成的答案是否与问题相关
- Context Relevance：评估召回的知识是否跟问题相关。

指标总结：Relavance都是跟问题相关性；Context都是跟标准答案的对比；

做指标评测的最终目的不是为了拿到分数，而是根据这些分数确定优化的方向。

context recall 值较低解决：
- 知识库补充：确保测试样本都能有匹配的知识回答。
- 更换更好的Embedded模型：好的embedding模型可以理解文本的深层次语义。
- 用户query的改写：设计通用prompt模板，使用大模型来改写query，提升召回准确率。
context precision 值较低解决：
- 在context recall优化基础上，尝试加入检索结果的重排rerank，提升相关文本的排名
answer correctness指标评测的是RAG系统整体的综合指标。如果该指标得分较低，而前两项分数较高，说明RAG系统在检索阶段表现良好，但是**生成**阶段出了问题。此时可以：
- 如优化prompt
- 调整大模型生成的超参数（如temperature）等
- 微调大模型

评测体系的挑战来自于**构建高质量的评测样本**以及如何**凝练、积累深厚的领域知识**。

## RAG优化

检索阶段没有召回到正确的参考信息：
- 增加召回数量：调高top_k值
- 结构更清晰的上下文信息：markdown结构化文档重建索引

将用户意图空间和知识空间二者相交：
- 重叠区：不断优化算法及高质量内容
- 未被覆盖的用户意图区域：补充缺失的知识
- 未被覆盖的知识空间区域：优化召回算法，定时清理知识库提出无关内容
文档切片方法：
- Token切片
- 句子切片
- 句子窗口切片：每个切片都包含周围的句子作为上下文窗口。
- 语义切片：根据语义相关性自适应地选择切片点。
- markdown切片
向量数据库：
- 内存向量库：无法持久化，受限于内存大小
- 本地向量数据库：可以持久化，但需要自行部署维护
- 云服务向量存储：**DashVector**，**Milvus 版**，**已有数据库的向量能力（PolarDB）**
问题改写方式：
- 问题扩充：利用大模型实现一个问题扩充
- 将单一问题改为多个问题步骤查询
- 用假设文档来增强索引（这就像你在找一本书时，心里已经有了一个大致的内容轮廓，然后用这个轮廓去图书馆匹配相似的书籍）：
	1. 先让大模型基于问题编一个"假想的答案文档"
	2. 用这个假想文档来检索真实文档
	3. 最后用检索到的真实文档来生成实际答案
提取标签增强检索：标签提取有两个关键场景：
1. 建立索引时，从文档切片中提取结构化标签
2. 检索时，从用户问题中提取对应的标签进行过滤