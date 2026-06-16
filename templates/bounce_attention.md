# 弹跳提醒 (Bounce Attention)

## 效果描述

元素在 Y 轴方向快速向上弹跳数次，幅度逐次衰减，模拟弹球自由落地的物理效果。用于吸引用户注意力，提示可交互或有新内容。整个过程约0.6秒，动感十足但不过分打扰。

**适用场景**：新功能提示、引导点击、图标提醒、红点跳动、未读消息

## XML 代码

```xml
<transition name="bounce" frameRate="30">
  <!-- 第1次弹跳：大幅向上 -->
  <item time="0" type="XY" target="TARGET_ID" tween="true" startValue="-,0" endValue="-,-20" duration="4" ease="Sine.Out"/>
  <item time="4" type="XY" target="TARGET_ID" tween="true" startValue="-,-20" endValue="-,0" duration="4" ease="Sine.In"/>
  <!-- 第2次弹跳：中幅 -->
  <item time="8" type="XY" target="TARGET_ID" tween="true" startValue="-,0" endValue="-,-10" duration="3" ease="Sine.Out"/>
  <item time="11" type="XY" target="TARGET_ID" tween="true" startValue="-,-10" endValue="-,0" duration="3" ease="Sine.In"/>
  <!-- 第3次弹跳：微幅 -->
  <item time="14" type="XY" target="TARGET_ID" tween="true" startValue="-,0" endValue="-,-4" duration="2" ease="Sine.Out"/>
  <item time="16" type="XY" target="TARGET_ID" tween="true" startValue="-,-4" endValue="-,0" duration="2" ease="Sine.In"/>
</transition>
```

> **重要（坐标系）**：XY 动效的 `startValue`/`endValue` 是**绝对坐标**（相对父组件），`-` 表示该轴保持当前值不变。即 `endValue="-,-20"` 表示 X 保持、Y 设为绝对坐标 -20，**不是**「在当前位置 -20」。本模板假设目标元素最终 Y=0，弹跳在 0 与 -20/-10/-4 之间衰减；若元素实际 Y 不是 0，需把所有 Y 值替换为「实际 Y」与「实际 Y - 弹跳高度」。弹跳幅度逐次衰减（20 → 10 → 4 像素），模拟物理阻尼效果。

## 参数说明

| 参数 | 当前值 | 说明 |
|------|--------|------|
| `frameRate` | `30` | 动画帧率 |
| `name` | `bounce` | 自定义动效名称，通过代码触发 |
| `TARGET_ID` | 需替换 | 目标元素的 ID |
| 第1次弹跳 | `-20` 像素 | 最大弹跳高度 |
| 第2次弹跳 | `-10` 像素 | 衰减到50% |
| 第3次弹跳 | `-4` 像素 | 衰减到20% |
| 弹跳次数 | 3次 | 物理自然的弹跳节奏 |
| 总时长 | 18帧/30fps = 0.60秒 | 短促醒目 |

### 强度调节建议

| 场景 | 初始弹跳高度 | 弹跳次数 | 体感 |
|------|------------|---------|------|
| 微妙提示 | `-10` 像素 | 2次 | 低调含蓄 |
| 标准提醒 | `-20` 像素 | 3次 | 明显可见（默认） |
| 强烈引导 | `-35` 像素 | 4次 | 大幅跳动，非常醒目 |

## 使用示例

将 `TARGET_ID` 替换为实际元素 ID，放置在 `</displayList>` 之后：

```xml
<?xml version="1.0" encoding="utf-8"?>
<component size="120,120">
  <displayList>
    <image id="n4_icon" name="newFeature" src="icon_new" xy="60,60" size="80,80"/>
  </displayList>
  <transition name="bounce" frameRate="30">
    <item time="0" type="XY" target="n4_icon" tween="true" startValue="-,0" endValue="-,-20" duration="4" ease="Sine.Out"/>
    <item time="4" type="XY" target="n4_icon" tween="true" startValue="-,-20" endValue="-,0" duration="4" ease="Sine.In"/>
    <item time="8" type="XY" target="n4_icon" tween="true" startValue="-,0" endValue="-,-10" duration="3" ease="Sine.Out"/>
    <item time="11" type="XY" target="n4_icon" tween="true" startValue="-,-10" endValue="-,0" duration="3" ease="Sine.In"/>
    <item time="14" type="XY" target="n4_icon" tween="true" startValue="-,0" endValue="-,-4" duration="2" ease="Sine.Out"/>
    <item time="16" type="XY" target="n4_icon" tween="true" startValue="-,-4" endValue="-,0" duration="2" ease="Sine.In"/>
  </transition>
</component>
```

### 代码触发示例（C#）

```csharp
// 有新消息时弹跳提醒
void OnNewMessage()
{
    ownerComponent.GetTransition("bounce").Play();
}
```

## 变体：弹跳 + 缩放挤压

每次落地时伴随微妙的横向挤压，增强弹跳的物理质感：

```xml
<transition name="bounce" frameRate="30">
  <!-- 第1次弹跳 -->
  <item time="0" type="XY" target="TARGET_ID" tween="true" startValue="-,0" endValue="-,-20" duration="4" ease="Sine.Out"/>
  <item time="4" type="XY" target="TARGET_ID" tween="true" startValue="-,-20" endValue="-,0" duration="4" ease="Sine.In"/>
  <!-- 落地挤压 -->
  <item time="7" type="Scale" target="TARGET_ID" tween="true" startValue="1,1" endValue="1.08,0.92" duration="2" ease="Sine.Out"/>
  <item time="9" type="Scale" target="TARGET_ID" tween="true" startValue="1.08,0.92" endValue="1,1" duration="3" ease="Sine.InOut"/>
  <!-- 第2次弹跳 -->
  <item time="9" type="XY" target="TARGET_ID" tween="true" startValue="-,0" endValue="-,-10" duration="3" ease="Sine.Out"/>
  <item time="12" type="XY" target="TARGET_ID" tween="true" startValue="-,-10" endValue="-,0" duration="3" ease="Sine.In"/>
  <!-- 第3次微弹 -->
  <item time="15" type="XY" target="TARGET_ID" tween="true" startValue="-,0" endValue="-,-4" duration="2" ease="Sine.Out"/>
  <item time="17" type="XY" target="TARGET_ID" tween="true" startValue="-,-4" endValue="-,0" duration="2" ease="Sine.In"/>
</transition>
```
