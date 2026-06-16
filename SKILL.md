---
name: fgui
description: FairyGUI/FGUI 组件开发与动效模板指南。用于创建、修改、审查 FairyGUI 组件 XML，处理 Transition 动效、预制动画模板、入场/退场/循环/特效/单组件状态动效、红点/未读/新消息提示、控制器 Controller、齿轮 Gear、显示元素、扩展组件和关联关系。当用户提到 FairyGUI、FGUI、GComponent、UIPackage、transition、动效、动画、缓动、ease、果冻、弹跳、淡入、淡出、滑入、滑出、脉冲、呼吸、loading、抖动、闪烁、翻转、红点、未读、徽记、按钮状态、组件 XML、controller、gear、loader、Button、ProgressBar、Slider 等相关任务时使用此 Skill。
---

# FairyGUI 组件开发

> 本 Skill 提供 FairyGUI Unity SDK 源 XML 的开发参考。优先按 SDK 示例工程中的 XML 写法生成内容；运行时二进制字段只作为语义校验依据。

## 模块索引

| 模块 | 路径 | 适用场景 |
|------|------|----------|
| 注意事项 | references/notes.md | 通用规则、GObject 属性、颜色格式、帧率、混合模式 |
| 动效 | references/transition.md | 创建/修改 Transition 动画、缓动效果 |
| 动效模板库 | references/transition_templates.md | 选择/适配预制动效模板，包含入场、退场、循环、特效、单组件状态 |
| 齿轮 | references/gear.md | 控制器定义、状态切换、齿轮绑定 |
| 元素 | references/elements.md | image/text/loader/graph/group/list 等显示元素 |
| 扩展组件 | references/components.md | Button/ProgressBar/Slider/ComboBox/Label/ScrollBar/Tree 等 |
| 关联关系 | references/relation.md | 定义元素之间的自适应关系 |

## 使用规则

1. **分析任务**，确定涉及哪些模块
2. **总是先加载 `notes.md`**（包含 GObject 通用属性和基本规则）
3. **再加载需要的专项模块**（读取对应 `.md` 文件）
4. **涉及预制动效模板时**，先加载 `transition.md` 和 `transition_templates.md`，再只读取被选中的 `templates/*.md`
5. **不要一次性加载全部模板**，除非任务是审查、迁移、合并或维护模板库本身
6. **多模块时按优先级加载**：注意事项 > 动效 > 动效模板库 > 齿轮 > 元素 > 扩展组件 > 关联关系

## 任务类型映射

| 任务关键词 | 加载模块 |
|------------|----------|
| 动画、动效、transition、缓动、ease、入场、退场 | 注意事项 + 动效 |
| 动效模板、模板库、预制动画、有什么动效、果冻、弹跳、淡入、淡出、滑入、滑出、脉冲、呼吸、loading、抖动、闪烁、翻转 | 注意事项 + 动效 + 动效模板库 + 对应模板 |
| 红点、未读、新消息、徽记、角标、提示动效、badge、pulse | 注意事项 + 动效 + 动效模板库 + 对应提示模板 |
| 单组件、选中、禁用、恢复可用、数值跳动、解锁、按钮状态、组件状态 | 注意事项 + 动效 + 动效模板库 + 对应单组件模板 |
| 控制器、状态切换、gear、页面切换、按钮状态 | 注意事项 + 齿轮 |
| 图片、文本、列表、loader、image、text、graph、richtext、textinput | 注意事项 + 元素 |
| Button、进度条、滑块、下拉框、ScrollBar、Tree | 注意事项 + 扩展组件 |
| 关联、自适应、跟随、居中、对齐 | 注意事项 + 关联关系 |

## 动效模板按需加载

`fgui-transition` 的模板库已合并到当前 Skill：

- 模板索引与选择流程：`references/transition_templates.md`
- 具体模板文件：`templates/*.md`

使用模板时先根据用户需求确定分类和名称；如果需求已经明确（例如“给红点加心跳提示”或“做一个 slide up 入场”），直接读取对应模板文件并适配目标组件。只有当用户询问“有什么动效可用”或需求不明确时，才展示分类菜单让用户选择。

---

## 组件基本结构

```xml
<?xml version="1.0" encoding="utf-8"?>
<component size="宽度,高度" [extention="扩展类型"]
           [overflow="visible|hidden|scroll"] [opaque="true"]
           [margin="上,右,下,左"] [clipSoftness="x,y"]
           [mask="遮罩元素ID"] [reversedMask="false"]
           [hitTest="resourceId,x,y"]>
  <controller ... />       <!-- 控制器定义 -->
  <displayList>            <!-- 显示列表 -->
    ...                    <!-- 显示元素 -->
  </displayList>
  <transition ... />       <!-- 动效定义 -->
  <ExtensionTag/>          <!-- 扩展类型标记 -->
</component>
```

### 组件属性

| 属性 | 说明 | 默认值 |
|------|------|--------|
| `size` | 组件尺寸 `"宽,高"` | 必填 |
| `extention` | 扩展类型 | 无 |
| `overflow` | 溢出处理: `visible`, `hidden`, `scroll` | `"visible"` |
| `opaque` | 是否不透明（点击空白区域是否穿透） | `true` |
| `margin` | 内边距 `"上,右,下,左"` | 无 |
| `clipSoftness` | 裁剪软边 `"x,y"` | 无 |
| `mask` | 遮罩元素 ID | 无 |
| `reversedMask` | 是否反向遮罩 | `false` |
| `hitTest` | 像素级点击测试 `"resourceId,x,y"` | 无 |
| `bgColor` / `bgColorEnabled` | 组件背景色与是否启用背景色 | 无 / `false` |
| `initName` | 初始化名称，源 XML 可出现 | 无 |
| `idnum` | 编辑器生成 ID 计数，保留即可，手写通常不需要 | 无 |

### 滚动属性（overflow="scroll" 时）

| 属性 | 说明 | 默认值 |
|------|------|--------|
| `scroll` | 滚动方向: `horizontal`, `vertical`, `both` | `"vertical"` |
| `scrollBar` | 滚动条策略: `default`, `visible`, `auto`, `hidden` | `"default"` |
| `scrollBarFlags` | 滚动条标志位 (int) | `0` |
| `scrollBarMargin` | 滚动条边距 `"上,右,下,左"` | 无 |
| `scrollBarRes` | 滚动条资源 `"垂直资源ID,水平资源ID"` | 无 |
| `ptrRes` | 下拉刷新资源 `"headerResId,footerResId"` | 无 |
| `pageController` | 滚动页同步到控制器 | 无 |
| `scrollItemToViewOnClick` | 点击子项时滚动到可见区域 | `false` |
| `foldInvisibleItems` | 折叠不可见子项占位 | `false` |

常用 `scrollBarFlags` 位含义：`2`=snapToItem，`8`=pageMode，`16/32`=强制开启/关闭 touchEffect，`64/128`=强制开启/关闭 bounceback，`256`=禁用惯性，`512`=禁用遮罩裁剪，`1024`=滚动条浮动，`2048`=不裁剪 margin。

---

## 快速示例

### 带动效的按钮

```xml
<?xml version="1.0" encoding="utf-8"?>
<component size="160,52" extention="Button">
  <controller name="button" pages="0,up,1,down,2,over,3,selectedOver" selected="0"/>
  <displayList>
    <image id="n1" src="btn_pressed">
      <gearDisplay controller="button" pages="1,3"/>
      <relation target="" sidePair="width-width,height-height"/>
    </image>
    <image id="n2" src="btn_normal">
      <gearDisplay controller="button" pages="0,2"/>
      <relation target="" sidePair="width-width,height-height"/>
    </image>
    <text id="n3" name="title" xy="0,7" size="160,45"
          fontSize="18" color="#ffffff"
          align="center" vAlign="middle">
      <relation target="" sidePair="width-width,height-height"/>
    </text>
  </displayList>
  <Button sound="ui://9leh0eyfo4lt7w"/>
  <transition name="t0">
    <item time="0" type="Scale" tween="true"
          startValue="1,1" endValue="0.95,0.95"
          duration="3"/>
    <item time="3" type="Scale" tween="true"
          startValue="0.95,0.95" endValue="1,1"
          duration="6" ease="Back.Out"/>
  </transition>
</component>
```
