# SVP TypeScript Template

一个使用 SVP 分层编译模型的 TypeScript 项目模板。

## 结构

```
.
├── blueprint.svp.yaml       # L5: 项目蓝图（从这里开始）
├── .svp/
│   ├── l4/                  # L4: 流程定义
│   ├── l3/                  # L3: 逻辑块
│   └── gen/                 # L2: 生成的代码骨架
├── src/
│   └── blocks/              # L1: 最终代码
└── package.json
```

## 快速开始

```bash
# 1. 使用此模板创建项目
npx degit semanticvoxelprotocol/template-ts my-project
cd my-project

# 2. 编辑蓝图
vim blueprint.svp.yaml

# 3. 初始化 SVP
svp init

# 4. 编译（使用 AI）
svp compile --level 5 --ai
svp compile --level 4 --ai
svp compile --level 3 --ai

# 5. 查看生成的代码
cat src/blocks/*.ts
```

## 工作流程

1. **编辑 L5** (`blueprint.svp.yaml`): 定义项目意图、领域、约束
2. **编译到 L4**: AI 生成流程图，人类审核
3. **编辑 L4** (`.svp/l4/`): 细化流程步骤
4. **编译到 L3**: AI 生成逻辑块，人类审核
5. **编辑 L3** (`.svp/l3/`): 完善伪代码和契约
6. **编译到 L2-L1**: AI 生成最终代码

## 原则

- **永远不要直接修改 `src/` 里的代码** —— 它们是从 L3 编译的
- **需要改代码？改 L3 然后重新编译**
- **L5-L3 提交到 Git，L2-L1 可以在 CI 生成**
