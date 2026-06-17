# 顶部掉落 (Drop In)

## 效果描述

元素从屏幕上方掉落到目标位置，使用 `Bounce.Out` 缓动产生落地弹跳效果——到达目标位置后反弹数次逐渐趋停。同时伴随快速淡入，视觉上如同物体从天而降。

**适用场景**：通知条出现、Toast 弹出、徽章掉落、成就解锁、奖励掉落

## XML 代码

```xml
<transition name="__enter__" frameRate="30">
  <!-- 从上方掉落：XY 为绝对坐标（相对父组件）。Y 从起点动到目标位置 -->
  <item time="0" type="XY" target="TARGET_ID" tween="true" startValue="-,-300" endValue="-,0" duration="18" ease="Bounce.Out"/>
  <!-- 快速淡入 -->
  <item time="0" type="Alpha" target="TARGET_ID" tween="true" startValue="0" endValue="1" duration="6" ease="Sine.Out"/>
</transition>
```

> **重要（坐标系）**：XY 动效的 `startValue`/`endValue` 是**绝对坐标**（相对父组件），`-` 表示该轴保持当前值不变。即 `startValue="-,-300"` 表示 X 保持、Y 设为绝对坐标 -300，**不是**「在当前位置 -300」。使用时需换算为绝对坐标：`endValue` 填目标元素的最终 Y，`startValue` 填「最终 Y - 掉落偏移」（如目标最终 Y=0 → 起点 Y=-300）。`Bounce.Out` 缓动自带弹跳效果，无需手动编排多段动画。

## 参数说明

| 参数 | 当前值 | 说明 |
|------|--------|------|
| `frameRate` | `30` | 动画帧率 |
| `name` | `__enter__` | 自定义名称（本库入场命名约定）；引擎不会按名称自动播放，需在代码中 `GetTransition("__enter__").Play()` 触发，用于 Window 时置于 `DoShowAnimation` 内 |
| `TARGET_ID` | 需替换 | 目标元素的 ID |
| Y轴起止 | `-300`→`0` | 绝对Y坐标（相对父组件）；`0`=目标最终Y，`-300`=掉落起点Y |
| 掉落帧数 | `18` | 18帧/30fps = 0.60秒（含弹跳） |
| 淡入帧数 | `6` | 6帧/30fps = 0.20秒（快速显现） |
| 缓动 | `Bounce.Out` | 落地弹跳效果 |

### 偏移量调节建议

| 场景 | Y偏移 | 掉落duration | 体感 |
|------|-------|-------------|------|
| 小通知/Toast | `-150` | `15` | 轻巧短促 |
| 标准掉落 | `-300` | `18` | 适中（默认） |
| 大元素掉落 | `-500` | `24` | 大幅掉落，弹跳明显 |

## 使用示例

将 `TARGET_ID` 替换为实际元素 ID，放置在 `</displayList>` 之后：

```xml
<?xml version="1.0" encoding="utf-8"?>
<component size="1080,200">
  <displayList>
    <component id="n8_toast" name="toast" src="toast01" fileName="组件/ToastBar.xml" xy="540,100" size="900,120">
      <relation target="" sidePair="center-center"/>
    </component>
  </displayList>
  <transition name="__enter__" frameRate="30">
    <item time="0" type="XY" target="n8_toast" tween="true" startValue="-,-200" endValue="-,100" duration="18" ease="Bounce.Out"/>
    <item time="0" type="Alpha" target="n8_toast" tween="true" startValue="0" endValue="1" duration="6" ease="Sine.Out"/>
  </transition>
</component>
```

## 变体：掉落 + 缩放冲击

到达目标时伴随轻微缩放挤压，增强落地冲击感：

```xml
<transition name="__enter__" frameRate="30">
  <!-- 掉落 -->
  <item time="0" type="XY" target="TARGET_ID" tween="true" startValue="-,-300" endValue="-,0" duration="18" ease="Bounce.Out"/>
  <!-- 淡入 -->
  <item time="0" type="Alpha" target="TARGET_ID" tween="true" startValue="0" endValue="1" duration="6" ease="Sine.Out"/>
  <!-- 落地压扁：延迟到接近落地时触发 -->
  <item time="8" type="Scale" target="TARGET_ID" tween="true" startValue="1,1" endValue="1.06,0.94" duration="3" ease="Sine.Out"/>
  <item time="11" type="Scale" target="TARGET_ID" tween="true" startValue="1.06,0.94" endValue="1,1" duration="4" ease="Sine.InOut"/>
</transition>
```
