# 底部滑入 (Slide Up)

## 效果描述

元素从屏幕下方滑入到目标位置，同时伴随淡入效果。使用 `Expo.Out` 缓动函数，起始速度极快、末端缓慢趋停，带有明显的惯性减速感。

**适用场景**：弹窗入场、底部面板、操作菜单、消息通知

## XML 代码

```xml
<transition name="__enter__" frameRate="30">
  <!-- 从下方滑入：XY 为绝对坐标（相对父组件）。Y 从起点动到目标位置 -->
  <item time="0" type="XY" target="TARGET_ID" tween="true" startValue="-,300" endValue="-,0" duration="12" ease="Expo.Out"/>
  <!-- 同步淡入 -->
  <item time="0" type="Alpha" target="TARGET_ID" tween="true" startValue="0" endValue="1" duration="8" ease="Sine.Out"/>
</transition>
```

> **重要（坐标系）**：XY 动效的 `startValue`/`endValue` 是**绝对坐标**（相对父组件），`-` 表示该轴保持当前值不变。即 `startValue="-,300"` 表示 X 保持、Y 设为绝对坐标 300，**不是**「在当前位置 +300」。使用时需换算为绝对坐标：`endValue` 填目标元素的最终 Y，`startValue` 填「最终 Y + 滑入偏移」（如目标最终 Y=0 → 起点 Y=300）。下方「偏移量调节建议」中的数值指起点与终点的位移幅度，需据此换算成绝对坐标填入。

## 参数说明

| 参数 | 当前值 | 说明 |
|------|--------|------|
| `frameRate` | `30` | 动画帧率 |
| `name` | `__enter__` | 自定义名称（本库入场命名约定）；引擎不会按名称自动播放，需在代码中 `GetTransition("__enter__").Play()` 触发，用于 Window 时置于 `DoShowAnimation` 内 |
| `TARGET_ID` | 需替换 | 目标元素的 ID |
| Y轴起止 | `300`→`0` | 绝对Y坐标（相对父组件）；`0`=目标最终Y，`300`=滑入起点Y |
| 滑动帧数 | `12` | 12帧/30fps = 0.40秒 |
| 淡入帧数 | `8` | 8帧/30fps ≈ 0.27秒（比滑动短，更自然） |

### 偏移量调节建议

| 场景 | Y偏移 | 滑动duration | 体感 |
|------|-------|-------------|------|
| 小元素/通知条 | `150` | `10` | 轻巧快速 |
| 标准弹窗 | `300` | `12` | 适中（默认） |
| 全屏面板 | `500` | `15` | 大气沉稳 |

## 使用示例

将 `TARGET_ID` 替换为实际元素 ID，放置在 `</displayList>` 之后：

```xml
<?xml version="1.0" encoding="utf-8"?>
<component size="1080,2160" overflow="scroll">
  <displayList>
    <component id="n13_mosk" name="loaderBg" src="fz3thc" fileName="组件/BasicBackground.xml" pkg="gmtl205q" xy="0,0">
      <relation target="" sidePair="width-width,height-height"/>
    </component>
    <component id="n14_mosk" name="panel" src="moskqfd" fileName="组件/BottomPanel.xml" xy="60,1600" size="960,500">
      <relation target="" sidePair="center-center"/>
    </component>
  </displayList>
  <transition name="__enter__" frameRate="30">
    <item time="0" type="XY" target="n14_mosk" tween="true" startValue="-,300" endValue="-,0" duration="12" ease="Expo.Out"/>
    <item time="0" type="Alpha" target="n14_mosk" tween="true" startValue="0" endValue="1" duration="8" ease="Sine.Out"/>
  </transition>
</component>
```

## 变体：从左侧滑入

修改 XY 的 startValue 即可改变滑入方向：

```xml
<transition name="__enter__" frameRate="30">
  <!-- 从左侧滑入，X轴偏移-400像素 -->
  <item time="0" type="XY" target="TARGET_ID" tween="true" startValue="-400,-" endValue="0,-" duration="12" ease="Expo.Out"/>
  <item time="0" type="Alpha" target="TARGET_ID" tween="true" startValue="0" endValue="1" duration="8" ease="Sine.Out"/>
</transition>
```
