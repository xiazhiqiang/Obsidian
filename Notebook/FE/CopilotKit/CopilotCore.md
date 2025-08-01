## @copilot/react-core

![[deepwiki.com_CopilotKit_CopilotKit_3.1-react-core-package.png]]

## CopilotKitProvider

[代码](https://github.com/CopilotKit/CopilotKit/blob/main/CopilotKit/packages/react-core/src/context/copilot-context.tsx)

![[deepwiki.com_CopilotKit_CopilotKit_3-core-frontend-framework.png]]

### 1. **组件功能概述**

- **CopilotKit 组件**：
    
    - 通常包裹整个应用或部分子树。
    - 提供上下文 CopilotContext，包含各种状态、工具函数、API 配置等。
    - 使用 ToastProvider 和 CopilotErrorBoundary 包裹内部组件，提供提示和错误边界支持。
- **CopilotKitInternal 组件**：
    
    - 实际实现逻辑的组件，被 CopilotKit 包裹以提供错误边界和 Toast 支持。
    - 包含了状态管理、上下文提供、API 配置、功能调用处理器等。

---

### 2. **核心状态与工具**

- **状态管理**：
    
    - actions: 存储前端动作（FrontendAction）的映射。
    - coAgentStateRenders: 存储 Co-Agent 状态渲染器。
    - chatComponentsCache: 缓存聊天组件。
    - chatInstructions: 存储聊天指令。
    - `authStates`: 认证状态。
    - extensions: 扩展输入。
    - additionalInstructions: 附加指令。
    - chatSuggestionConfiguration: 聊天建议配置。
    - availableAgents: 可用代理（Agent）列表。
    - coagentStates: Co-Agent 状态。
    - agentSession: 当前代理会话。
    - threadId: 线程 ID。
    - runId: 运行 ID。
    - langGraphInterruptAction: LangGraph 中断动作。
    - suggestions: 建议项。
- **工具函数**：
    
    - setAction / removeAction: 添加/移除动作。
    - setCoAgentStateRender / removeCoAgentStateRender: 添加/移除 Co-Agent 状态渲染器。
    - getContextString: 生成上下文字符串。
    - addContext / removeContext / getAllContext: 上下文管理。
    - getFunctionCallHandler: 生成函数调用处理器。
    - addDocumentContext / removeDocumentContext: 文档上下文管理。
    - setLangGraphInterruptAction / removeLangGraphInterruptAction: LangGraph 中断动作管理。

| Category             | Key Properties                                         | Description                                  |
| -------------------- | ------------------------------------------------------ | -------------------------------------------- |
| **Function Calling** | `actions`, `setAction`, `removeAction`                 | Manages frontend actions that AI can execute |
| **CoAgent State**    | `coAgentStateRenders`, `agentSession`, `coagentStates` | Handles agent state rendering and management |
| **Text Context**     | `addContext`, `removeContext`, `getContextString`      | Manages contextual information for AI        |
| **Chat State**       | `isLoading`, `messages`, `chatInstructions`            | Core chat functionality and state            |
| **Runtime**          | `runtimeClient`, `copilotApiConfig`, `threadId`        | Communication with backend runtime           |

## Hook

### useCopilotChat

![[deepwiki.com_CopilotKit_CopilotKit_3-core-frontend-framework (3).png]]
