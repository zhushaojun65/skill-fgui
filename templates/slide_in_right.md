# 右侧滑入 (Slide In Right)

## 效果描述

元素从屏幕右侧滑入到目标位置，同时伴随淡入效果。使用 `Expo.Out` 缓动函数，起始速度极快、末端缓慢趋停，带有明显的惯性减速感。适合横向布局的列表项、页面切换等场景。

**适用场景**：列表项入场、页面切换、侧边栏展开、卡片横向滑入

## XML 代码

```xml
<transition name="__enter__" frameRate="30">
  <!-- 从右侧滑入：XY 为绝对坐标（相对父组件）。X 从起点动到目标位置 -->
  <item time="0" type="XY" target="TARGET_ID" tween="true" startValue="400,-" endValue="0,-" duration="12" ease="Expo.Out"/>
  <!-- 同步淡入 -->
  <item time="0" type="Alpha" target="TARGET_ID" tween="true" startValue="0" endValue="1" duration="8" ease="Sine.Out"/>
</transition>
```

> **重要（坐标系）**：XY 动效的 `startValue`/`endValue` 是**绝对坐标**（相对父组件），`-` 表示该轴保持当前值不变。即 `startValue="400,-"` 表示 Y 保持、X 设为绝对坐标 400，**不是**「在当前位置 +400」。使用时需换算为绝对坐标：`endValue` 填目标元素的最终 X，`startValue` 填「最终 X + 滑入偏移」（如目标最终 X=0 → 起点 X=400）。下方「偏移量调节建议」中的数值指起点与终点的位移幅度，需据此换算成绝对坐标填入。

## 参数说明

| 参数 | 当前值 | 说明 |
|------|--------|------|
| `frameRate` | `30` | 动画帧率 |
| `name` | `__enter__` | 自定义名称（本库入场命名约定）；引擎不会按名称自动播放，需在代码中 `GetTransition("__enter__").Play()` 触发，用于 Window 时置于 `DoShowAnimation` 内 |
| `TARGET_ID` | 需替换 | 目标元素的 ID |
| X轴起止 | `400`→`0` | 绝对X坐标（相对父组件）；`0`=目标最终X，`400`=滑入起点X |
| 滑动帧数 | `12` | 12帧/30fps = 0.40秒 |
| 淡入帧数 | `8` | 8帧/30fps ≈ 0.27秒（比滑动短，更自然） |

### 偏移量调节建议

| 场景 | X偏移 | 滑动duration | 体感 |
|------|-------|-------------|------|
| 小元素/列表项 | `200` | `10` | 轻巧快速 |
| 标准卡片 | `400` | `12` | 适中（默认） |
| 全屏页面 | `600` | `15` | 大气流畅 |

## 使用示例

将 `TARGET_ID` 替换为实际元素 ID，放置在 `</displayList>` 之后：

```xml
<?xml version="1.0" encoding="utf-8"?>
<component size="1080,2160" overflow="scroll">
  <displayList>
    <component id="n13_mosk" name="loaderBg" src="fz3thc" fileName="组件/BasicBackground.xml" pkg="gmtl205q" xy="0,0">
      <relation target="" sidePair="width-width,height-height"/>
    </component>
    <component id="n14_mosk" name="card" src="moskqfd" fileName="组件/InfoCard.xml" xy="60,400" size="960,300">
      <relation target="" sidePair="center-center"/>
    </component>
  </displayList>
  <transition name="__enter__" frameRate="30">
    <item time="0" type="XY" target="n14_mosk" tween="true" startValue="400,-" endValue="0,-" duration="12" ease="Expo.Out"/>
    <item time="0" type="Alpha" target="n14_mosk" tween="true" startValue="0" endValue="1" duration="8" ease="Sine.Out"/>
  </transition>
</component>
```

## 变体：从左侧滑入

修改 XY 的 startValue 为负值即可改变方向：

```xml
<transition name="__enter__" frameRate="30">
  <!-- 从左侧滑入，X轴偏移-400像素 -->
  <item time="0" type="XY" target="TARGET_ID" tween="true" startValue="-400,-" endValue="0,-" duration="12" ease="Expo.Out"/>
  <item time="0" type="Alpha" target="TARGET_ID" tween="true" startValue="0" endValue="1" duration="8" ease="Sine.Out"/>
</transition>
```
