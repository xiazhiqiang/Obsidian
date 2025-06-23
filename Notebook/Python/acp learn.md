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