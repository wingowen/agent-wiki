---
title: Skills（Agent 技能复用模式）
created: 2026-04-21
updated: 2026-04-21
type: concept
tags: [agent, architecture, memory, tool-use]
sources:
  - https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview
  - https://www.anthropic.com/engineering/code-execution-with-mcp
---

## Definition

Skills（技能）是一种将可重用的指令、脚本和资源打包在文件夹中，形成模型可以在专业任务中引用的结构化能力单元的设计模式。这种模式允许 Agent 将自己编写的代码保存为可重用函数，构建高级能力的工具箱。

## Key techniques

### 1. 代码持久化与复用

Agent 可以将成功的任务实现保存为可重用的函数：

```javascript
// In ./skills/save-sheet-as-csv.ts
import * as gdrive from './servers/google-drive';
export async function saveSheetAsCsv(sheetId: string) {
  const data = await gdrive.getSheet({ sheetId });
  const csv = data.map(row => row.join(',')).join('\\n');
  await fs.writeFile(`./workspace/sheet-${sheetId}.csv`, csv);
  return `./workspace/sheet-${sheetId}.csv`;
}
```

### 2. SKILL.md 文件

为保存的函数添加 SKILL.md 文件可以创建结构化的技能，模型可以引用和使用：

```
skills/
├── save-sheet-as-csv/
│   ├── index.ts
│   └── SKILL.md  # 技能描述和使用说明
```

### 3. 与状态持久化的结合

Skills 可以利用文件系统访问来维护状态，使 Agent 能够在操作之间保持连续性：

```javascript
const leads = await salesforce.query({ query: 'SELECT Id, Email FROM Lead LIMIT 1000' });
const csvData = leads.map(l => `${l.Id},${l.Email}`).join('\\n');
await fs.writeFile('./workspace/leads.csv', csvData);

// Later execution picks up where it left off
const saved = await fs.readFile('./workspace/leads.csv', 'utf-8');
```

## Related entities and concepts

- **相关实体**：[[claude-code|Claude Code]]（使用 Skills 的 Agent 实现）
- **相关概念**：[[context-engineering|上下文工程]]、[[memory (待创建)|记忆]]、tool-use
- **扩展阅读**：MCP 的 [[model-context-protocol|代码执行模式]] 配合 Skills 可以实现更高效的 Agent 能力积累
