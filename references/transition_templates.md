# FairyGUI 通用动效模板库

> 预制的高质量动效模板，可直接复制到 FairyGUI 组件 XML 中使用。

## 加载方式

本模块由原 `fgui-transition` 技能合并而来，是当前 `fgui` 技能的动效模板索引层。使用时遵循动态披露：

1. 先加载 `references/notes.md` 和 `references/transition.md`，确认基础 XML 规则和 Transition 语法。
2. 再加载本文件，确定模板分类、候选模板和适配规则。
3. 只读取用户需要的具体模板文件，例如 `templates/badge_beat.md` 或 `templates/slide_up.md`。
4. 只有在审查、迁移、合并、扩充模板库时，才批量读取 `templates/` 下全部模板。

模板目录共 28 个文件，覆盖入场、退场、循环、特效、单组件五类常见动画。模板默认显式使用 `frameRate="30"`；当前 FairyGUI Unity SDK 示例 XML 基本省略 `frameRate`，按编辑器默认帧率处理。若项目要求贴近 SDK 示例默认写法，应删除或调整 `frameRate`，并同步换算所有 `time` / `duration`，不要只改帧率属性。

## 简介

本模块提供预制的高质量 FairyGUI Transition 动效模板，也提供一系列 FairyGUI Transition 动效 XML 代码片段，涵盖入场、退场、循环、特效、单组件五类常见动画效果。模板可直接复制并嵌入到 FairyGUI 组件中，再按目标组件的元素 ID、尺寸、坐标和项目帧率做局部适配。

## 使用方法

1. **选择模板**：根据需求从上表选择合适的动效模板；如果用户需求明确，直接定位到对应模板。
2. **复制 XML**：打开对应的模板文件，例如 `templates/jelly.md`，复制其中的 `<transition>` XML 代码片段。
3. **适配参数**：替换 `TARGET_ID`，并按组件大小调整 `frameRate`、缩放幅度、位移幅度和动效名称。
4. **嵌入组件**：将 `<transition>` 块插入到组件 XML 的 `</displayList>` 之后。

## 技术说明

- 所有模板基于 FairyGUI 的 Transition XML 格式。
- 模板默认帧率为 `30fps`，模板内通过 `frameRate="30"` 显式声明；SDK 示例源 XML 通常不写该属性。
- `frameRate` 会改变 `time` / `duration` 的换算，不能在不换算帧数的情况下随意删除或替换。
- 动效使用 FairyGUI 内置的缓动曲线（Ease）。
- 模板设计遵循移动端交互最佳实践。

## 模板索引

### 🟢 入场类

| 模板名称 | 文件 | 适用场景 | 帧率 |
|----------|------|----------|------|
| 果冻效果 (Jelly) | templates/jelly.md | 入场弹跳、弹窗出现、按钮反馈 | 30fps |
| 淡入 (Fade In) | templates/fade_in.md | 通用入场、页面切换、元素渐显 | 30fps |
| 底部滑入 (Slide Up) | templates/slide_up.md | 弹窗入场、底部面板、操作菜单 | 30fps |
| 弹性缩放入场 (Scale Bounce In) | templates/scale_bounce_in.md | 弹窗出现、卡片弹出、奖励展示 | 30fps |
| 右侧滑入 (Slide In Right) | templates/slide_in_right.md | 列表项入场、页面切换、侧边栏展开 | 30fps |
| 顶部掉落 (Drop In) | templates/drop_in.md | 通知条出现、Toast弹出、徽章掉落 | 30fps |

### 🔴 退场类

| 模板名称 | 文件 | 适用场景 | 帧率 |
|----------|------|----------|------|
| 淡出 (Fade Out) | templates/fade_out.md | 通用退场、叠加层消失、元素隐藏 | 30fps |
| 向下滑出 (Slide Down) | templates/slide_down.md | 弹窗退场、底部面板关闭、菜单收起 | 30fps |
| 缩小消失 (Scale Out) | templates/scale_out.md | 弹窗关闭、卡片消失、对话框退场 | 30fps |
| 向左滑出 (Slide Out Left) | templates/slide_out_left.md | 列表项移除、页面切换退场、卡片滑出 | 30fps |
| 向上飞出 (Fly Up) | templates/fly_up.md | 消息发送、元素移除、气泡飞出 | 30fps |

### 🔵 循环类

| 模板名称 | 文件 | 适用场景 | 帧率 |
|----------|------|----------|------|
| 呼吸效果 (Breathing) | templates/breathing.md | 关注引导、按钮提示、等待状态 | 30fps |
| 脉冲提示 (Pulse) | templates/pulse.md | 新消息提醒、未读标记、引导高亮 | 30fps |
| 浮动效果 (Float) | templates/float.md | 悬浮按钮、引导箭头、装饰元素 | 30fps |
| 旋转加载 (Spin Loop) | templates/spin_loop.md | 加载指示器、刷新图标、处理中状态 | 30fps |
| 摇摆效果 (Swing) | templates/swing.md | 铃铛图标、通知提示、注意力引导 | 30fps |

### 🟡 特效类

| 模板名称 | 文件 | 适用场景 | 帧率 |
|----------|------|----------|------|
| 抖动效果 (Shake) | templates/shake.md | 密码错误、输入验证失败、操作拒绝 | 30fps |
| 点击反馈 (Tap Feedback) | templates/tap_feedback.md | 按钮点击、可交互元素反馈 | 30fps |
| 闪烁高亮 (Flash) | templates/flash.md | 获得道具、解锁提示、数值变化强调 | 30fps |
| 弹跳提醒 (Bounce Attention) | templates/bounce_attention.md | 新功能提示、引导点击、图标提醒 | 30fps |
| 翻转效果 (Flip) | templates/flip.md | 卡片翻转、抽卡揭示、状态切换 | 30fps |

### 🟣 单组件类

| 模板名称 | 文件 | 适用场景 | 帧率 |
|----------|------|----------|------|
| 单组件选中弹起 (Component Select Pop) | templates/component_select_pop.md | Tab选中、卡片选中、列表项聚焦 | 30fps |
| 单组件取消选中回落 (Component Deselect Settle) | templates/component_deselect_settle.md | 取消选中、失焦、恢复普通态 | 30fps |
| 单组件恢复可用 (Component Enable Ready) | templates/component_enable_ready.md | 冷却结束、技能可用、奖励可领 | 30fps |
| 单组件置灰禁用 (Component Disable Dim) | templates/component_disable_dim.md | 按钮禁用、资源不足、条件未满足 | 30fps |
| 单组件数值跳动 (Component Value Bump) | templates/component_value_bump.md | 数值刷新、金币变化、战力增加 | 30fps |
| 单组件解锁弹出 (Component Unlock Pop) | templates/component_unlock_pop.md | 功能解锁、章节解锁、奖励激活 | 30fps |
| 徽记心跳提示 (Badge Beat) | templates/badge_beat.md | 红点、未读徽记、新消息角标 | 30fps |

### 配对建议

| 入场模板 | 推荐退场模板 |
|----------|-------------|
| 淡入 (Fade In) | 淡出 (Fade Out) |
| 底部滑入 (Slide Up) | 向下滑出 (Slide Down) |
| 弹性缩放入场 (Scale Bounce In) | 缩小消失 (Scale Out) |
| 果冻效果 (Jelly) | 缩小消失 (Scale Out) 或 淡出 (Fade Out) |
| 右侧滑入 (Slide In Right) | 向左滑出 (Slide Out Left) |
| 顶部掉落 (Drop In) | 向上飞出 (Fly Up) 或 淡出 (Fade Out) |

## 使用规则

当本模块被加载时，按以下交互式流程操作：

### 第1步：展示类型菜单

向用户展示以下分类选项，让用户选择动效类型：

```
请选择动效类型：

  🟢 1. 入场类 — 元素出现时的动画（共6个）
  🔴 2. 退场类 — 元素消失时的动画（共5个）
  🔵 3. 循环类 — 持续播放的循环动画（共5个）
  🟡 4. 特效类 — 事件触发的一次性特效（共5个）
  🟣 5. 单组件类 — 单个控件自身状态变化（共7个）

请输入编号或类型名称：
```

> 如果用户的需求已经明确指向某个类型（如"入场动画"、"loading效果"），可跳过此步直接进入第2步。

### 第2步：展示动效列表

根据用户选择的类型，展示该类型下所有可用的动效模板，让用户选择：

**入场类：**
```
请选择入场动效：

  1. 果冻效果 (Jelly)        — 弹性压扁回弹，弹窗/按钮反馈
  2. 淡入 (Fade In)           — 透明度渐显，通用入场
  3. 底部滑入 (Slide Up)      — 从底部滑入，弹窗/面板
  4. 弹性缩放入场 (Scale Bounce In) — 缩放+过冲回弹，卡片/奖励
  5. 右侧滑入 (Slide In Right) — 从右侧横向滑入，列表项/页面
  6. 顶部掉落 (Drop In)       — 从上方掉落弹跳，通知/Toast

请输入编号或动效名称：
```

**退场类：**
```
请选择退场动效：

  1. 淡出 (Fade Out)          — 透明度渐隐，通用退场
  2. 向下滑出 (Slide Down)     — 向下滑出屏幕，面板关闭
  3. 缩小消失 (Scale Out)      — 缩放到0消失，弹窗关闭
  4. 向左滑出 (Slide Out Left) — 向左加速滑出，列表项移除
  5. 向上飞出 (Fly Up)         — 蓄力向上飞出，消息发送

请输入编号或动效名称：
```

**循环类：**
```
请选择循环动效：

  1. 呼吸效果 (Breathing) — 缩放呼吸，关注引导/等待
  2. 脉冲提示 (Pulse)     — 缩放脉冲，消息提醒/未读
  3. 浮动效果 (Float)     — 上下悬浮，装饰/引导箭头
  4. 旋转加载 (Spin Loop) — 匀速旋转，Loading指示器
  5. 摇摆效果 (Swing)     — 钟摆摇摆，铃铛/通知图标

请输入编号或动效名称：
```

**特效类：**
```
请选择特效动效：

  1. 抖动效果 (Shake)          — 水平抖动衰减，密码错误/验证失败
  2. 点击反馈 (Tap Feedback)    — 按下缩小弹回，按钮/图标点击
  3. 闪烁高亮 (Flash)           — 明暗闪烁衰减，获得道具/解锁
  4. 弹跳提醒 (Bounce Attention) — Y轴弹跳衰减，新功能/引导
  5. 翻转效果 (Flip)            — X轴缩放翻转，卡片/抽卡/状态切换

请输入编号或动效名称：
```

**单组件类：**
```
请选择单组件动效：

  1. 单组件选中弹起 (Component Select Pop)       — Tab/卡片/列表项进入选中态
  2. 单组件取消选中回落 (Component Deselect Settle) — 取消选中、失焦、恢复普通态
  3. 单组件恢复可用 (Component Enable Ready)      — 冷却结束、奖励可领、技能可用
  4. 单组件置灰禁用 (Component Disable Dim)       — 禁用、资源不足、条件未满足
  5. 单组件数值跳动 (Component Value Bump)        — 金币/战力/等级/进度变化
  6. 单组件解锁弹出 (Component Unlock Pop)        — 功能/章节/道具解锁
  7. 徽记心跳提示 (Badge Beat)                    — 红点、未读、新消息角标

请输入编号或动效名称：
```

> 如果用户已经明确说出了动效名称（如"果冻效果"、"shake"），可跳过选择直接进入第3步。

### 第3步：加载并适配模板

1. **加载模板**：读取对应的模板文件
2. **适配修改**：根据实际组件调整以下参数：
   - `target`：替换为实际目标元素的 ID
   - `frameRate`：根据项目要求调整帧率
   - 缩放/位移幅度：根据组件大小适当调整
   - 单组件状态动效：优先只驱动目标组件自身，不扩散到父容器或兄弟节点
3. **嵌入组件**：将 `<transition>` 块插入到组件 XML 的 `</displayList>` 之后
4. **配对提示**：如果用户选择了入场动效，主动推荐配对的退场动效（参考配对建议表）

> 注意：Transition 挂在当前组件上，`target` 只是被动画驱动的显示对象。代码触发时应从承载该 transition 的 `GComponent` 调用 `GetTransition("名称")`。
> 对红点、未读提示、新消息提示等必须持续可见的元素，优先使用 `Scale` / `XY` 类循环，不要默认使用 `Alpha` 淡隐。
> 单组件选中/禁用/解锁等状态变化，如果项目已有 controller/gear 管显隐，应让 transition 只负责过渡表现，不抢状态权威。

---

## ⚠️ 最高优先级规则：延迟补间必须锁起始关键帧（time=0）

**这条规则是 FairyGUI 动效最易踩、且会反复犯的 bug 源，每次适配模板时都必须检查，无例外。**

### 问题现象

当你给某元素写了 `time > 0` 的延迟补间（例如标题在底板弹出后才淡入），元素会在延迟期间**先以设计静止态闪现一下**，然后突然跳回 `startValue` 再开始动画 —— 用户看到一帧"先出现再消失再淡入"的闪烁。

### 根因

FairyGUI 的 `startValue` 只在补间**开始的那一刻**生效，它**不会提前接管**延迟期间的属性。在 `0 → time` 这段延迟里，属性值 = 元素当前实际值（通常是设计静止态，Alpha=1 / Scale=1 / 原坐标）。所以延迟入场若 startValue=0，延迟期间元素其实是 Alpha=1 满不透明显示着。

### 规则（强制）

**任何 `time > 0` 的补间，其涉及的每个动画属性，都必须在 `time="0"` 补一条 `tween="false"` 的起始关键帧，把该属性锁成它在 `startValue` 里声明的同一个值。**

### 正确写法

```xml
<transition name="__enter__" frameRate="30">
  <!-- ✅ 起始关键帧锁：延迟元素在 t=0 就被设成 startValue，杜绝闪现 -->
  <item time="0" type="Alpha" target="n_title" tween="false" value="0"/>
  <item time="0" type="XY"    target="n_title" tween="false" value="100,-50"/>
  <item time="0" type="Alpha" target="n_btn"   tween="false" value="0"/>
  <item time="0" type="Scale" target="n_btn"   tween="false" value="0.4,0.4"/>
  <!-- 正常补间 -->
  <item time="0"  type="Alpha" target="n_bg"    tween="true" startValue="0" endValue="1" duration="6" ease="Sine.Out"/>
  <item time="5"  type="Alpha" target="n_title" tween="true" startValue="0" endValue="1" duration="6" ease="Sine.Out"/>
  <item time="10" type="Scale" target="n_btn"   tween="true" startValue="0.4,0.4" endValue="1,1" duration="6" ease="Back.Out"/>
</transition>
```

### 自检清单（写完每个 transition 后逐条核对）

| 补间场景 | 是否需要 time=0 锁帧 | 锁成什么值 |
|---------|---------------------|-----------|
| 入场补间 `time=0` | 通常可省；若元素可能被其他动效/控制器改过，建议锁帧 | startValue 或明确的归一化起点 |
| 入场补间 `time>0`，startValue ≠ 静止态 | ✅ **必须** | 锁成 startValue（Alpha=0、Scale=0.3 等） |
| 退场补间 `time=0` | 通常可省；若可能重播、中断或承接入场残留，建议锁帧 | 退场确知起点（Alpha=1、Scale=1、原坐标等） |
| 退场补间 `time>0` | ⚠️ **建议** | 锁成静止态（Alpha=1, Scale=1, 原坐标）—— 归一化可能被中断的入场残留状态 |

> **适配模板时的强制步骤**：本库大多数模板默认 `time=0`（单元素即时入场），不触发此 bug。但当你像第3步那样把模板**组合成多元素的串联入场/退场**（不同元素 time 错开）时，**必须**为每个 `time>0` 的延迟元素补 time=0 锁帧。这是适配阶段不可省略的动作。

## 模板文件规范

每个模板文件应包含以下结构：
- **效果描述**：动画视觉效果的文字说明
- **XML 代码**：可直接使用的 transition XML 片段
- **参数说明**：可调整的关键参数及其含义
- **使用示例**：在完整组件中的嵌入示例

## 新增模板指南

向 `templates/` 目录添加新的 `.md` 文件，遵循上述模板文件规范，并在本文件的模板索引表中添加对应条目。

## 维护要求

- 模板设计遵循移动端交互最佳实践，默认幅度要克制，避免大范围位移影响布局判断。
- 新增或修改模板后，同步更新本文件的模板索引、分类菜单和配对建议。
- 如果模板使用 `Scale` 或 `Rotation`，在参数说明或示例里提醒目标元素配置合适的 `pivot` / `anchor`。
- 如果模板或使用示例包含 `XY`，必须让 `startValue` / `endValue` 与示例元素的实际 `xy` 绝对坐标一致。不要在目标元素 `xy` 非 0 时继续使用 `0`、`300`、`-400` 这类仅适用于最终坐标为 0 的占位值。
- 如果模板示例包含 C# 调用，默认从承载 transition 的 `GComponent` 调用 `GetTransition("名称")`，不要在被动画的子元素上调用。
