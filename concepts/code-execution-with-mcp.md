---
title: 通过 MCP 实现代码执行
created: 2026-04-21
updated: 2026-04-21
type: concept
tags: [mcp, tool-use, architecture, agent, efficiency]
sources:
  - https://www.anthropic.com/engineering/code-execution-with-mcp
---

## Definition

通过 MCP 实现代码执行（Code execution with MCP）是一种让 Agent 通过编写代码来调用 MCP 服务器工具的技术方案。该方案将 MCP 服务器从"直接工具调用"模式转变为"代码 API"模式，让 Agent 能够按需发现和调用工具，从而在处理更多工具的同时使用更少的 token。

## Key techniques

### 1. 工具定义过载问题的解决

**问题**：传统 MCP 客户端预先加载所有工具定义到上下文，导致：
- 工具描述占据大量上下文窗口空间
- Agent 需要处理数十万 token 才能开始工作
- 响应时间增加，成本上升

**解决方案**：代码执行模式允许 Agent 编写代码来动态发现和调用工具，而不是预先加载所有定义：

```javascript
// 渐进式工具发现 - 按需加载
const client = new MCPClient({
  servers: ['./servers/gdrive', './servers/salesforce', './servers/slack'],
  lazyLoadTools: true  // 启用懒加载
});
```

### 2. 中间结果本地处理

**问题**：直接工具调用中，每次工具执行的结果都通过上下文窗口传递，累积大量 token 消耗。

**解决方案**：中间结果默认保留在执行环境中，Agent 只能看到显式记录或返回的内容：

```javascript
// 中间结果不进入上下文
const sheet = await gdrive.getSheet({ sheetId: 'abc123' });
for (const row of sheet.rows) {
  await salesforce.updateRecord({...});
}
// 只返回汇总信息
console.log(`Updated ${sheet.rows.length} leads`);
```

### 3. 隐私保护令牌化

对于敏感数据，Agent 框架可以自动对 PII（个人身份信息）进行令牌化处理：

```javascript
// 数据在到达模型之前被拦截并令牌化
// What the agent would see:
[
  { salesforceId: '00Q...', email: '[EMAIL_1]', phone: '[PHONE_1]', name: '[NAME_1]' },
  ...
]
// 真实数据从 Google Sheets 流向 Salesforce，但不经过模型
```

### 4. 复杂控制流的代码化

循环、条件判断和错误处理使用熟悉的代码模式：

```javascript
let found = false;
while (!found) {
  const messages = await slack.getChannelHistory({ channel: 'C123456' });
  found = messages.some(m => m.text.includes('deployment complete'));
  if (!found) await new Promise(r => setTimeout(r, 5000));
}
console.log('Deployment notification received');
```

## Related entities and concepts

- **相关实体**：[[model-context-protocol|Model Context Protocol (MCP)]]（基础协议）、[[claude-code|Claude Code]]（实现案例）
- **相关概念**：
  - [[context-engineering|上下文工程]]（token 管理的核心理念）
  - [[skills-agent-capability-reuse|Skills]]（可重用代码的组织模式）
  - [[memory (待创建)|记忆]]（状态持久化相关）
- **对比**：
  - 直接工具调用 vs 代码执行（各有权衡）
  - 沙箱化（sandboxing）是代码执行的安全基础
