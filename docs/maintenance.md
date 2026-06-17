# fgui 技能维护说明

## 动态披露边界

`SKILL.md` 只维护模块索引、加载顺序和任务关键词映射。新增 SDK 细节、组件属性、模板适配规则或运行时代码说明时，优先放入 `references/` 或 `templates/`，只在入口文件补最小路由。

## 当前维护规则

- 源 XML 写法以 FairyGUI Unity SDK 示例工程的 `UIProject/assets/**/*.xml` 为准。
- Unity 运行时代码接入按需加载 `references/unity_runtime.md`，不要把运行时 API 展开到 `SKILL.md`。
- 带 `XY` 的模板示例必须使用绝对坐标，示例元素的 `xy` 应与 transition 的最终坐标一致。
- 红点、未读、徽记等持续可见提示默认不使用 `Alpha` 淡隐作为主效果。
