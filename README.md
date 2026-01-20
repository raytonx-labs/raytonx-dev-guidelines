# Raytonx 开发规范（Dev Guidelines）

为提升团队协作效率与代码长期可维护性，**Raytonx** 在所有 Typescript 项目中统一采用严格的工程化约束体系，通过自动化工具确保代码质量与代码风格的高度一致。

本规范的核心目标是：  
**减少人为约定、避免编辑器差异、让代码在保存时即达到可提交状态。**

---

## 1. 核心校验体系

本项目使用 **ESLint + Prettier + Husky** 统一代码规范和自动化处理：

- **静态质量分析**：ESLint (`eslint.config.mts`) 控制目录规则，保证工程可维护性  
- **代码风格**：Prettier (`.prettierrc`) 保证跨开发者一致性  
- **自动化处理**：保存时清理未使用导入、自动排序导入、格式化代码  
- **提交前检查**：Husky + ESLint / lint-staged 检查并自动修复提交文件  
- **编辑器集成**：VSCode 通过 ESLint 插件接管保存操作，禁用原生 `formatOnSave`

## 2. 使用说明

### 2.1 安装依赖

```bash
pnpm install

# 新项目需要安装以下依赖
pnpm install -D \
  @eslint/js \
  @next/eslint-plugin-next \
  @typescript-eslint/parser \
  @trivago/prettier-plugin-sort-imports \
  eslint-config-prettier \
  eslint-plugin-prettier \
  eslint-plugin-react \
  eslint-plugin-react-hooks \
  eslint-plugin-unused-imports \
  typescript
```

### 2.2 编辑器准备

1. 在 VSCode 中安装并启用官方 **ESLint** 扩展。
2. 将以下配置文件复制到项目根目录：
  - `.prettierrc`
  - `eslint.config.mts`
  - VSCode 工作区设置`.vscode/settings.json`
3. 启用husky
  - 在 `package.json` 添加

      ```json
      "lint-staged": {
        "*.{ts,tsx,js,jsx}": ["eslint --fix"]
      }
      ```
  
  - 复制文件 `.husky/pre-commit`
---

### 2.3 生效验证

1. 打开任意 `.ts` / `.tsx` 文件。
2. 手动执行以下任一操作：
   - 打乱 `import` 语句顺序
   - 引入但未使用的变量或模块
3. 按下 **Command + S**，期望行为如下：
   - 未使用的 `import` 被自动移除
   - `import` 顺序被自动整理
   - 代码整体格式符合 `.prettierrc` 规范
4. 将修改或打乱顺序的代码添加到 git 暂存区，然后命令行执行 `npx lint-staged`，或者直接提交，代码会直接修改错误
