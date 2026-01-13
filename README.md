# Raytonx 开发规范（Dev Guidelines）

为提升团队协作效率与代码长期可维护性，**Raytonx** 在所有项目中统一采用严格的工程化约束体系，通过自动化工具确保代码质量与代码风格的高度一致。

本规范的核心目标是：  
**减少人为约定、避免编辑器差异、让代码在保存时即达到可提交状态。**

---

## 1. 核心校验体系

### 1.1 静态质量分析（`eslint.config.mts`）

- 基于 **ESLint Flat Config（v9+）** 架构，避免传统 `.eslintrc` 的层级歧义。
- 深度集成以下最佳实践规则：
  - **TypeScript**
  - **React**
  - **Next.js**
- 所有规则以「工程级可维护性」为优先，而非单纯语法洁癖。

---

### 1.2 代码风格标准（`.prettierrc`）

- 统一定义缩进、引号、分号、换行等视觉规范。
- 保证代码在不同开发者、不同操作系统下的**字符级一致性**。
- Prettier **只负责格式，不参与代码语义判断**。

---

### 1.3 自动化处理逻辑

在保存阶段自动完成以下处理，无需人工干预：

1. **未使用导入清理**  
   - `eslint-plugin-unused-imports`
2. **导入语句自动排序**  
   - `@trivago/prettier-plugin-sort-imports`
3. **代码风格格式化**  
   - Prettier

---

### 1.4 编辑器深度集成（`.vscode/settings.json`）

- 在按下 **Command + S** 的瞬间，按固定顺序执行：
  1. 清理未使用的 imports
  2. 自动排序 imports
  3. 执行 Prettier 格式化
- **禁用 VSCode 原生的 `formatOnSave`**  
  所有保存时修改均由 **ESLint 插件统一接管**，避免规则冲突或执行顺序不一致的问题。

---

## 2. 快速上手

### 2.1 安装依赖

```bash
# ESLint & TypeScript
pnpm add -D eslint typescript jiti \
  @typescript-eslint/parser \
  @typescript-eslint/eslint-plugin \
  typescript-eslint

# Prettier
pnpm add -D prettier eslint-config-prettier eslint-plugin-prettier

# Import 增强
pnpm add -D @trivago/prettier-plugin-sort-imports \
  eslint-plugin-unused-imports

# React & Next.js 规则
pnpm add -D eslint-plugin-react \
  eslint-plugin-react-hooks \
  @next/eslint-plugin-next
```

### 2.2 编辑器准备

1. 在 VSCode 中安装并启用官方 **ESLint** 扩展（作者：Dirk Baeumer）。
2. 将以下配置文件复制到项目根目录：
   - `.prettierrc`
   - `eslint.config.mts`
3. 在项目根目录创建并配置 VSCode 工作区设置：
   - 路径：`.vscode/settings.json`
   - 确保所有保存时的格式化与修复行为**统一由 ESLint 插件接管**。

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

若以上行为均自动完成，则说明 **Raytonx 开发规范已在当前项目中成功启用**。
