# 向左滑出 (Slide Out Left)

## 效果描述

元素从当前位置向左侧滑出屏幕，同时伴随淡出效果。使用 `Expo.In` 缓动函数，起始缓慢加速、末端极快冲出，带有果断的退场感。与"右侧滑入"形成天然配对。

**适用场景**：列表项移除、页面切换退场、卡片滑出、面板收起

## XML 代码

```xml
<transition name="__exit__" frameRate="30">
  <!-- 向左滑出：XY 为绝对坐标（相对父组件）。X 从当前位置动到屏幕外 -->
  <item time="0" type="XY" target="TARGET_ID" tween="true" startValue="0,-" endValue="-400,-" duration="10" ease="Expo.In"/>
  <!-- 同步淡出 -->
  <item time="2" type="Alpha" target="TARGET_ID" tween="true" startValue="1" endValue="0" duration="8" ease="Sine.In"/>
</transition>
```

> **重要（坐标系）**：XY 动效的 `startValue`/`endValue` 是**绝对坐标**（相对父组件），`-` 表示该轴保持当前值不变。即 `endValue="-400,-"` 表示 Y 保持、X 设为绝对坐标 -400，**不是**「在当前位置 -400」。退场时 `startValue` 填目标元素当前最终 X，`endValue` 填「最终 X - 滑出偏移」（如当前 X=0 → 终点 X=-400）。淡出延迟 2 帧启动，让元素先开始移动再渐隐，视觉上更流畅。

## 参数说明

| 参数 | 当前值 | 说明 |
|------|--------|------|
| `frameRate` | `30` | 动画帧率 |
| `name` | `__exit__` | 自定义名称（本库退场命名约定）；引擎不会按名称自动播放，需在代码中 `GetTransition("__exit__").Play()` 触发，用于 Window 时置于 `DoHideAnimation` 内 |
| `TARGET_ID` | 需替换 | 目标元素的 ID |
| X轴起止 | `0`→`-400` | 绝对X坐标（相对父组件）；`0`=当前最终X，`-400`=滑出终点X |
| 滑出帧数 | `10` | 10帧/30fps ≈ 0.33秒 |
| 淡出帧数 | `8` | 8帧/30fps ≈ 0.27秒 |
| 淡出延迟 | `2` | 延迟2帧再开始淡出 |

### 偏移量调节建议

| 场景 | X偏移 | 滑出duration | 体感 |
|------|-------|-------------|------|
| 小元素/列表项 | `-200` | `8` | 轻巧快速 |
| 标准卡片 | `-400` | `10` | 适中（默认） |
| 全屏页面 | `-600` | `12` | 从容退出 |

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
  <transition name="__exit__" frameRate="30">
    <item time="0" type="XY" target="n14_mosk" tween="true" startValue="60,-" endValue="-340,-" duration="10" ease="Expo.In"/>
    <item time="2" type="Alpha" target="n14_mosk" tween="true" startValue="1" endValue="0" duration="8" ease="Sine.In"/>
  </transition>
</component>
```

## 变体：向右滑出

修改 endValue 为正值即可：

```xml
<transition name="__exit__" frameRate="30">
  <item time="0" type="XY" target="TARGET_ID" tween="true" startValue="0,-" endValue="400,-" duration="10" ease="Expo.In"/>
  <item time="2" type="Alpha" target="TARGET_ID" tween="true" startValue="1" endValue="0" duration="8" ease="Sine.In"/>
</transition>
```
