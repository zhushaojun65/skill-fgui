# FairyGUI 组件开发指南 (fgui)

> Claude Code Skill - 用于创建和修改 FairyGUI 组件 XML 文件的开发指南

## 简介

本 Skill 提供 FairyGUI Unity SDK 源 XML 的开发参考，并合并了 `fgui-transition` 的通用动效模板库。它帮助开发者快速创建 UI 组件、适配预制 Transition 动效、编写控制器和齿轮绑定。生成 XML 时优先采用 SDK 示例工程中的源 XML 写法，运行时二进制字段只作为语义校验依据。

## 参考资料

- [FairyGUI 官方文档](https://www.fairygui.com/docs/)
- [FairyGUI 编辑器下载](https://www.fairygui.com/download)

## 功能覆盖

| 模块 | 说明 |
|------|------|
| 动效 (Transition) | 入场/退场动画、缓动效果、音效播放 |
| 动效模板库 | 预制入场、退场、循环、特效、单组件状态动效模板 |
| 控制器 (Controller) | 状态管理、页面切换 |
| 齿轮 (Gear) | 元素属性与控制器绑定 |
| 显示元素 | image/text/loader/graph/list/group 等 |
| 扩展组件 | Button/ProgressBar/Slider/ComboBox/Label 等 |
| 关联关系 | 元素自适应布局 |
| 资源包 | package.xml 资源声明、九宫格/tile、movieclip/jta、font/sound/atlas |
| Unity 运行时 | UIPackage/GComponent/Window/虚拟列表/扩展类注册等按需参考 |

## 文件结构

```
fgui/
├── SKILL.md                    # Skill 入口文件
├── references/
│   ├── notes.md               # 通用规则、GObject 属性
│   ├── transition.md          # 动效定义
│   ├── transition_templates.md # 动效模板索引、选择流程、适配规则
│   ├── gear.md                # 控制器、齿轮机制
│   ├── elements.md            # 显示元素类型
│   ├── components.md          # 扩展组件类型
│   ├── relation.md            # 关联关系
│   ├── resources.md           # package.xml 资源声明、九宫格/tile、JTA/字体/音效
│   └── unity_runtime.md       # Unity SDK 运行时接入，按需加载
├── templates/                  # 预制 Transition 动效模板
│   ├── jelly.md                # 果冻入场
│   ├── fade_in.md              # 淡入
│   ├── slide_up.md             # 底部滑入
│   ├── pulse.md                # 脉冲提示
│   ├── badge_beat.md           # 红点/未读徽记心跳
│   └── ...                     # 共 28 个模板
└── README.md                  # 本文件
```

## 使用方式

在 Claude Code 中，当需要处理以下任务时会自动加载此 Skill：

- 创建/修改 FairyGUI 组件 XML 文件
- 添加动画效果 (Transition)
- 选择或适配预制动效模板
- 设置状态切换 (Controller/Gear)
- 添加显示元素 (image/text/loader 等)
- 创建扩展组件 (Button/ProgressBar 等)
- 维护 package.xml 资源声明、九宫格图片、JTA 动画、字体和音效引用
- 处理 Unity SDK 运行时接入 (UIPackage/GComponent/Window/虚拟列表等)

### 触发关键词

- FairyGUI、FGUI、GComponent、UIPackage、组件 XML
- 动画、动效、transition、缓动、ease、动效模板、预制动画、有什么动效
- 入场、退场、循环、特效、单组件、组件状态
- 果冻、弹跳、淡入、淡出、滑入、滑出、脉冲、呼吸、loading、抖动、闪烁、翻转
- 红点、未读、新消息、徽记、角标、badge、pulse
- 控制器、状态切换、gear、页面切换
- 图片、文本、列表、loader、image、text
- Button、进度条、滑块、下拉框
- package.xml、资源声明、九宫格、scale9grid、tile、movieclip、jta、font、sound、atlas

## 按需加载原则

`SKILL.md` 只负责路由。处理任务时先读取 `references/notes.md`，再按任务读取专项模块。涉及 `package.xml`、九宫格、tile、JTA、字体或音效资源声明时读取 `references/resources.md`。涉及预制动效时读取 `references/transition.md` 和 `references/transition_templates.md`，然后只读取实际需要的 `templates/*.md`。涉及 Unity 运行时代码时才读取 `references/unity_runtime.md`。只有维护或审查模板库时才批量读取所有模板。

## 动效模板分类

| 分类 | 数量 | 示例 |
|------|------|------|
| 入场类 | 6 | Jelly、Fade In、Slide Up、Scale Bounce In |
| 退场类 | 5 | Fade Out、Slide Down、Scale Out |
| 循环类 | 5 | Breathing、Pulse、Float、Spin Loop |
| 特效类 | 5 | Shake、Tap Feedback、Flash、Flip |
| 单组件类 | 7 | Component Select Pop、Component Disable Dim、Badge Beat |

## 示例

### 带动效的按钮

```xml
<?xml version="1.0" encoding="utf-8"?>
<component size="160,52" extention="Button">
  <controller name="button" pages="0,up,1,down,2,over,3,selectedOver" selected="0"/>
  <displayList>
    <image id="n1" src="btn_pressed">
      <gearDisplay controller="button" pages="1,3"/>
    </image>
    <image id="n2" src="btn_normal">
      <gearDisplay controller="button" pages="0,2"/>
    </image>
    <text id="n3" name="title" xy="0,7" size="160,45"
          fontSize="18" color="#ffffff" align="center"/>
  </displayList>
  <Button sound="ui://9leh0eyfo4lt7w"/>
  <transition name="t0">
    <item time="0" type="Scale" tween="true"
          startValue="1,1" endValue="0.95,0.95" duration="3"/>
    <item time="3" type="Scale" tween="true"
          startValue="0.95,0.95" endValue="1,1" duration="6" ease="Back.Out"/>
  </transition>
</component>
```



---

## 安装指南 🛠️

本 Skill 旨在为多种主流 AI Agent 提供 FairyGUI 组件开发指南。请根据您所使用的 Agent 选择对应的安装方式。

---

### 1. Claude Code 🤖

[Claude Code](https://docs.anthropic.com/zh-cn/docs/claude-code) 通过读取项目或全局的 `SKILL.md` 文件来自动加载 Skill。

#### 全局安装（推荐）
将本仓库克隆到 Claude Code 的全局 Skill 目录中：

- **macOS / Linux**:
  ```bash
  git clone https://github.com/zhushaojun65/skill-fgui.git ~/.claude/skills/fgui
  ```
- **Windows (Command Prompt / CMD)**:
  ```cmd
  git clone https://github.com/zhushaojun65/skill-fgui.git "%USERPROFILE%\.claude\skills\fgui"
  ```
- **Windows (PowerShell)**:
  ```powershell
  git clone https://github.com/zhushaojun65/skill-fgui.git "$HOME\.claude\skills\fgui"
  ```

#### 项目级安装
若仅在特定项目中使用，可以将其克隆到项目根目录下的 `.claude/skills/` 目录中：

- **Git Bash / Terminal**:
  ```bash
  git clone https://github.com/zhushaojun65/skill-fgui.git .claude/skills/fgui
  ```
- **Windows (PowerShell)**:
  ```powershell
  git clone https://github.com/zhushaojun65/skill-fgui.git .claude\skills\fgui
  ```

#### 验证安装
安装完成后，在与 Claude Code 对话时提及 **FairyGUI** 相关的词汇（例如 "编写一个FairyGUI按钮" 或 "FairyGUI 动效怎么写"），Claude Code 会自动检测并加载该 Skill。

---

### 2. OpenAI Codex 🧠

[Codex CLI](https://github.com/openai/codex) 通过项目根目录或全局的 `AGENTS.md` 配置文件来注入上下文。

#### 全局安装
将本仓库克隆到全局目录：

- **macOS / Linux**:
  ```bash
  git clone https://github.com/zhushaojun65/skill-fgui.git ~/.codex/skills/fgui
  ```
- **Windows (Command Prompt / CMD)**:
  ```cmd
  git clone https://github.com/zhushaojun65/skill-fgui.git "%USERPROFILE%\.codex\skills\fgui"
  ```
- **Windows (PowerShell)**:
  ```powershell
  git clone https://github.com/zhushaojun65/skill-fgui.git "$HOME\.codex\skills\fgui"
  ```

克隆完成后，在全局配置文件 `~/.codex/AGENTS.md`（Windows 下为 `%USERPROFILE%\.codex\AGENTS.md` 或 `$HOME\.codex\AGENTS.md`）中追加引用：

```markdown
## FairyGUI Skill
@include ~/.codex/skills/fgui/SKILL.md
```

#### 项目级安装
克隆到项目目录中：

```bash
git clone https://github.com/zhushaojun65/skill-fgui.git .codex/skills/fgui
```

然后在项目根目录的 `AGENTS.md` 中追加：

```markdown
## FairyGUI Skill
@include .codex/skills/fgui/SKILL.md
```

---

### 3. Antigravity 🚀

[Antigravity](https://antigravity.dev) 会自动扫描全局 Skill 目录（`~/.agent/skills/`）并加载所有可用的 Skill。

#### 安装方法
将仓库克隆到全局 Agent 目录下：

- **macOS / Linux**:
  ```bash
  git clone https://github.com/zhushaojun65/skill-fgui.git ~/.agent/skills/fgui
  ```
- **Windows (Command Prompt / CMD)**:
  ```cmd
  git clone https://github.com/zhushaojun65/skill-fgui.git "%USERPROFILE%\.agent\skills\fgui"
  ```
- **Windows (PowerShell)**:
  ```powershell
  git clone https://github.com/zhushaojun65/skill-fgui.git "$HOME\.agent\skills\fgui"
  ```

#### 验证安装
Antigravity 启动时会自动扫描并注册该 Skill。您可以在已加载的 Skill 列表中看到 `fgui`。

---

### 4. OpenCode 💻

[OpenCode](https://github.com/sst/opencode) 通过全局配置文件或项目根目录下的配置文件来加载外部 Skill 上下文。

#### 全局安装
克隆到全局配置目录中：

- **macOS / Linux**:
  ```bash
  git clone https://github.com/zhushaojun65/skill-fgui.git ~/.config/opencode/skills/fgui
  ```
- **Windows (Command Prompt / CMD)**:
  ```cmd
  git clone https://github.com/zhushaojun65/skill-fgui.git "%APPDATA%\opencode\skills\fgui"
  ```
- **Windows (PowerShell)**:
  ```powershell
  git clone https://github.com/zhushaojun65/skill-fgui.git "$env:APPDATA\opencode\skills\fgui"
  ```

在全局配置文件 `~/.config/opencode/config.json`（Windows 下为 `%APPDATA%\opencode\config.json`）中注册该 Skill：

```json
{
  "skills": [
    "~/.config/opencode/skills/fgui/SKILL.md"
  ]
}
```

#### 项目级安装
克隆到项目本地：

```bash
git clone https://github.com/zhushaojun65/skill-fgui.git .opencode/skills/fgui
```

在项目根目录的 `opencode.json` 中配置：

```json
{
  "skills": [
    ".opencode/skills/fgui/SKILL.md"
  ]
}
```

---

### 安装配置汇总 📊

| Agent | 全局目录 | 项目目录 | 配置文件 / 载入方式 |
| :--- | :--- | :--- | :--- |
| **Claude Code** | `~/.claude/skills/fgui` | `.claude/skills/fgui` | 自动识别并发现 |
| **OpenAI Codex** | `~/.codex/skills/fgui` | `.codex/skills/fgui` | 在 `AGENTS.md` 中使用 `@include` 引入 |
| **Antigravity** | `~/.agent/skills/fgui` | 支持配置路径 | 启动时自动发现并注册 |
| **OpenCode** | `~/.config/opencode/skills/fgui` | `.opencode/skills/fgui` | 在 `config.json` 或 `opencode.json` 中配置 |
