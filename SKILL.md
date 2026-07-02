---
name: fgui
description: Use when 用户需要创建、修改或审查 FairyGUI/FGUI 组件 XML、资源包声明或 Unity FairyGUI UI 接入，且任务涉及 GComponent、UIPackage、Transition 动效、预制动画模板、controller、gear、relation、loader、Button、ProgressBar、Slider、package.xml、红点/未读/徽记、入场/退场/循环/特效/状态动效等。
---

# FairyGUI 组件开发

本 Skill 提供 FairyGUI Unity SDK 源 XML 的开发参考。生成或修改 XML 时优先采用 SDK 示例工程中的源 XML 写法；运行时二进制字段只作为语义校验依据。

## 动态披露目标

SKILL.md 只负责判断任务类型、选择引用资料和约束读取顺序。不要把 SDK 细节、完整 XML 示例或模板内容直接放在入口上下文里；处理任务时只读取当前任务需要的 references/ 或 templates/ 文件。

## 模块索引

| 模块 | 路径 | 适用场景 |
|------|------|----------|
| 注意事项 | references/notes.md | 通用规则、GObject 属性、颜色格式、帧率、混合模式 |
| 组件结构 | references/component_structure.md | component 根节点、滚动属性、overflow、margin、mask、hitTest、designImage |
| 资源包 | references/resources.md | package.xml 资源声明、publish/atlas、image 九宫格/tile、movieclip/jta、font/sound/TMP 字体 |
| 动效 | references/transition.md | 创建/修改 Transition 动画、缓动效果 |
| 动效模板库 | references/transition_templates.md | 选择/适配预制动效模板，包含入场、退场、循环、特效、单组件状态 |
| 齿轮 | references/gear.md | 控制器定义、状态切换、齿轮绑定 |
| 元素 | references/elements.md | image/text/loader/graph/group/list 等显示元素 |
| 扩展组件 | references/components.md | Button/ProgressBar/Slider/ComboBox/Label/ScrollBar/Tree 等 |
| 关联关系 | references/relation.md | 定义元素之间的自适应关系 |
| Unity 运行时 | references/unity_runtime.md | UIPackage、GComponent、Window、虚拟列表、UIPanel/UIPainter、手势、扩展类注册 |

## 使用规则

1. 先判断任务是否需要 FGUI 专项资料；若只是安装、仓库维护或普通说明，不要加载业务引用文件。
2. 生成、修改或审查源 XML 前，先读取 references/notes.md。
3. 再按任务关键词读取最少专项模块；不要因为可能用到而预读全部资料。
4. 涉及预制动效模板时，读取 references/transition.md 和 references/transition_templates.md 后，只读取被选中的 templates/*.md。
5. 只有在审查、迁移、合并或维护模板库本身时，才批量读取所有模板。
6. 多模块时按依赖顺序加载：注意事项 > 组件结构 > 资源包 > 动效 > 动效模板库 > 齿轮 > 元素 > 扩展组件 > 关联关系 > Unity 运行时。
7. README.md 面向安装和人工说明；执行 FGUI 技术任务时优先使用 SKILL.md 路由到 references/，不要把 README 当作技术来源。

## 任务类型映射

| 任务关键词 | 加载模块 |
|------------|----------|
| component 根节点、组件基本结构、size、extention、overflow、scroll、margin、mask、hitTest、designImage、滚动容器 | 注意事项 + 组件结构 |
| 动画、动效、transition、缓动、ease、入场、退场 | 注意事项 + 动效 |
| 动效模板、模板库、预制动画、有什么动效、果冻、弹跳、淡入、淡出、滑入、滑出、脉冲、呼吸、loading、抖动、闪烁、翻转 | 注意事项 + 动效 + 动效模板库 + 对应模板 |
| 红点、未读、新消息、徽记、角标、提示动效、badge、pulse | 注意事项 + 动效 + 动效模板库 + 对应提示模板 |
| 单组件、选中、禁用、恢复可用、数值跳动、解锁、按钮状态、组件状态 | 注意事项 + 动效 + 动效模板库 + 对应单组件模板 |
| package.xml、资源声明、publish、atlas、九宫格、scale9grid、scale=""、tile、duplicatePadding、movieclip、jta、font、texture、renderMode、samplePointSize、sound、swf | 注意事项 + 资源包 |
| 控制器、状态切换、gear、页面切换、按钮状态 | 注意事项 + 齿轮 |
| 图片、文本、列表、loader、image、text、graph、richtext、textinput、movieclip、jta | 注意事项 + 元素；若涉及 package.xml 资源声明再加资源包 |
| Button、进度条、滑块、下拉框、ScrollBar、Tree | 注意事项 + 扩展组件 |
| 关联、自适应、跟随、居中、对齐 | 注意事项 + 关联关系 |
| Unity SDK、UIPackage、GComponent、Window、虚拟列表、SetPackageItemExtension、UIPanel、UIPainter、手势、TypingEffect、TMP、Spine、DragonBones、LuaSupport | 注意事项 + Unity 运行时 |

## 动效模板按需加载

fgui-transition 的模板库已合并到当前 Skill：

- 模板索引与选择流程：references/transition_templates.md
- 具体模板文件：templates/*.md

使用模板时先根据用户需求确定分类和名称；如果需求已经明确，例如“给红点加心跳提示”或“做一个 slide up 入场”，直接读取对应模板文件并适配目标组件。只有当用户询问“有什么动效可用”或需求不明确时，才展示分类菜单让用户选择。
