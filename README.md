# SVP TypeScript 项目模板

这是一个使用 **Semantic Voxel Protocol (SVP)** 的 TypeScript 项目模板，演示如何通过分层编译从意图生成代码。

## 项目结构

```
.
├── blueprint.svp.yaml       # L5: 项目蓝图（意图层）
├── .svp/
│   ├── l4/
│   │   └── flows.yaml       # L4: 架构层（AI生成）
│   ├── l3/
│   │   └── domain.yaml      # L3: 逻辑层（AI生成）
│   └── gen/blocks/          # L2: 代码骨架（AI生成）
│       └── *.ts.block
├── src/
│   └── blocks/              # L1: 实现代码（AI生成）
│       └── *.ts
├── .env.example             # 环境变量模板
└── README.md                # 本文件
```

## 快速开始

### 1. 安装 SVP CLI

```bash
# 在项目根目录外安装 CLI
npm install -g @semanticvoxelprotocol/cli

# 或者本地链接（开发模式）
cd /path/to/svp-cli
npm link
```

### 2. 配置 AI

```bash
# 复制环境变量模板
cp .env.example .env

# 编辑 .env，填入你的 API Key
# 支持 OpenAI、DeepSeek、Claude、Ollama 等
```

### 3. 验证配置

```bash
svp config
```

### 4. 查看项目状态

```bash
svp status
```

输出示例：
```
  SVP 项目状态

  层级状态:
    ✓ L5 📋 Blueprint (blueprint.svp.yaml)
    ○ L4 🔗 Logic Chain
    ○ L3 🧩 Logic Blocks (0 files)
    ○ L2 📝 Code Blocks (0 files)
    ○ L1 💻 Generated Code (0 files)

  下一步:
    • 运行 `svp compile --level 5 --ai` 生成 L4
```

### 5. 开始编译（L5 → L4 → L3 → L2 → L1）

```bash
# L5 → L4: 意图 → 架构
svp compile --level 5 --ai

# L4 → L3: 架构 → 逻辑块
svp compile --level 4 --ai

# L3 → L2: 逻辑 → 代码骨架
svp compile --level 3 --ai

# L2 → L1: 骨架 → 实现
svp compile --level 2 --ai
```

### 6. 查看生成的代码

```bash
# 查看 L4 架构
ls -la .svp/l4/flows.yaml

# 查看 L3 逻辑块
cat .svp/l3/domain.yaml

# 查看 L2 代码骨架
ls .svp/gen/blocks/

# 查看 L1 实现代码
ls src/blocks/
```

### 7. 开发模式（监听变化）

```bash
# 监听 L5-L3 变化，自动提示重新编译
svp dev
```

## 演示场景

### 场景 1: 新增功能

假设需要在订单服务中添加"优惠券验证"功能：

1. **编辑 L5**: 在 `blueprint.svp.yaml` 中添加优惠券领域
2. **重新编译 L5 → L4**: `svp compile --level 5 --ai`
3. **审核 L4**: 检查生成的流程是否包含优惠券验证步骤
4. **继续编译**: L4 → L3 → L2 → L1
5. **查看结果**: 新生成的代码自动包含优惠券相关逻辑

### 场景 2: 修改逻辑

假设需要修改订单验证逻辑：

1. **编辑 L3**: 修改 `.svp/l3/domain.yaml` 中的 `validate_order` 块
2. **重新编译 L3 → L2 → L1**: 
   ```bash
   svp compile --level 3 --ai
   svp compile --level 2 --ai
   ```
3. **查看差异**: L1 代码自动更新，反映新的验证逻辑

### 场景 3: 代码审查

```bash
# 查看 L2 代码骨架（带 TODO 标记）
cat .svp/gen/blocks/validate_order_input.ts.block

# 查看 L1 实际实现
cat src/blocks/validate_order_input.ts

# 对比：L2 是骨架，L1 是实现
```

## 分层说明

### L5: Blueprint（人类编写）

- **文件**: `blueprint.svp.yaml`
- **内容**: 项目意图、领域划分、约束条件
- **编辑**: 人类直接编辑

### L4: Logic Chain（AI生成，人类审核）

- **文件**: `.svp/l4/flows.yaml`
- **内容**: 业务流程、接口契约
- **编辑**: AI生成，人类可修改

### L3: Logic Block（AI生成，人类审核）

- **文件**: `.svp/l3/domain.yaml`
- **内容**: 伪代码、契约、类型定义
- **编辑**: AI生成，人类可修改（修改的最后一层）

### L2: Code Block（AI生成，只读）

- **文件**: `.svp/gen/blocks/*.ts.block`
- **内容**: 代码骨架、TODO占位符
- **编辑**: **禁止直接修改**，通过修改L3重新生成

### L1: Code（AI生成，只读）

- **文件**: `src/blocks/*.ts`
- **内容**: 完整可运行代码
- **编辑**: **禁止直接修改**，通过修改L3重新生成

## 关键原则

1. **上层是源数据**：L5-L3 是源数据，提交到 Git
2. **下层是派生产物**：L2-L1 由 AI 生成，可 CI 生成
3. **单向编译**：L5 → L4 → L3 → L2 → L1
4. **修改L3重新编译**：不要直接修改 L1 代码

## 支持的 AI Provider

| Provider | 配置方式 | 状态 |
|----------|---------|------|
| OpenAI | `OPENAI_API_KEY` | ✅ |
| DeepSeek | `OPENAI_BASE_URL=https://api.deepseek.com/v1` | ✅ 已验证 |
| Anthropic Claude | `ANTHROPIC_API_KEY` | ✅ |
| Ollama | `LOCAL_AI_URL=http://localhost:11434/v1` | ✅ |
| OneAPI/NewAPI | `OPENAI_BASE_URL=https://your-endpoint/v1` | ✅ |

## 成本估算

基于 DeepSeek API：
- 完整编译一次：~¥0.03（约25K tokens）
- 增量编译（单Block）：~¥0.005

## 故障排除

### 1. API Key 无效

```
错误: API key not configured
```
**解决**: 检查 `.env` 文件是否正确配置

### 2. 模型不存在

```
错误: Model Not Exist
```
**解决**: 检查模型名称是否与平台支持的一致

### 3. 编译超时

L3→L2 和 L2→L1 可能需要较长时间（多Block串行处理）
**解决**: 正常现象，后续将优化为并行编译

## 更多信息

- [SVP 设计文档](../Design/)
- [AI 配置指南](../Design/05_AI_Configuration.md)
- [使用方式](../Design/04_Usage.md)
- [实现总结](../Design/06_Implementation_Summary.md)

## 演示检查清单

给开发组演示前准备：

- [ ] 确认 `.env` 已配置有效 API Key
- [ ] 运行 `svp config` 验证配置
- [ ] 清理 `.svp/l4/`, `.svp/l3/`, `.svp/gen/`, `src/blocks/` 目录
- [ ] 准备好 blueprint.svp.yaml（示例已提供）
- [ ] 测试完整编译流程

## 许可证

MIT
