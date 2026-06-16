# 脉冲提示 (Pulse)

## 效果描述

元素在原始尺寸和轻微放大之间循环切换，形成有节奏的脉冲提示。使用 `Sine.InOut` 缓动使缩放过渡柔和流畅，避免生硬跳变。

**适用场景**：新消息提醒、未读标记、限时提示、引导高亮、可点击提示

## XML 代码

```xml
<transition name="pulse" frameRate="30" autoPlay="true" autoPlayRepeat="-1">
  <item time="0" type="Scale" target="TARGET_ID" tween="true" startValue="1,1" endValue="1.15,1.15" duration="20" ease="Sine.InOut" repeat="-1" yoyo="true"/>
</transition>
```

## 参数说明

| 参数 | 当前值 | 说明 |
|------|--------|------|
| `frameRate` | `30` | 动画帧率 |
| `name` | `pulse` | 自定义动效名称 |
| `autoPlay` | `true` | 自动播放 |
| `autoPlayRepeat` | `-1` | 无限循环 |
| `TARGET_ID` | 需替换 | 目标元素的 ID |
| 缩放范围 | `1` ↔ `1.15` | 原始大小到 115% |
| 单次帧数 | `20` | 20帧/30fps ≈ 0.67秒（单程） |
| `yoyo` | `true` | 往返播放 |
| `repeat` | `-1` | 无限重复 |

> 对红点、未读提示、新消息提示等必须持续可见的元素，默认使用缩放脉冲，不要用 Alpha 淡到半透明或不可见。

### 频率调节建议

| 场景 | 缩放范围 | duration | 体感 |
|------|----------|----------|------|
| 柔和提示 | `1` ↔ `1.08` | `30` | 缓慢温和，不打扰 |
| 标准脉冲 | `1` ↔ `1.15` | `20` | 明显可见（默认） |
| 紧急提醒 | `1` ↔ `1.25` | `12` | 快速脉冲，强烈提示 |

## 使用示例

将 `TARGET_ID` 替换为实际元素 ID，放置在 `</displayList>` 之后：

```xml
<?xml version="1.0" encoding="utf-8"?>
<component size="80,80">
  <displayList>
    <image id="n3_dot" name="redDot" src="reddot01" xy="40,40" size="24,24"/>
  </displayList>
  <transition name="pulse" frameRate="30" autoPlay="true" autoPlayRepeat="-1">
    <item time="0" type="Scale" target="n3_dot" tween="true" startValue="1,1" endValue="1.15,1.15" duration="20" ease="Sine.InOut" repeat="-1" yoyo="true"/>
  </transition>
</component>
```

## 变体：缩放 + 透明度

仅当元素允许短暂变淡时，可叠加轻微透明度变化：

```xml
<transition name="pulse" frameRate="30" autoPlay="true" autoPlayRepeat="-1">
  <item time="0" type="Scale" target="TARGET_ID" tween="true" startValue="1,1" endValue="1.15,1.15" duration="20" ease="Sine.InOut" repeat="-1" yoyo="true"/>
  <item time="0" type="Alpha" target="TARGET_ID" tween="true" startValue="1" endValue="0.75" duration="20" ease="Sine.InOut" repeat="-1" yoyo="true"/>
</transition>
```
