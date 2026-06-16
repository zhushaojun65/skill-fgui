# 徽记心跳提示 (Badge Beat)

## 效果描述

面向红点、未读徽记、新消息角标的轻量循环动效。只使用 `Scale` 做两段短促心跳，不改变透明度，不移动位置，保证徽记始终可见。

**适用场景**：红点提示、未读徽记、新消息角标、活动入口提示、小型状态标记

## XML 代码

```xml
<transition name="badgeBeat" frameRate="30" autoPlay="true" autoPlayRepeat="-1">
  <item time="0" type="Scale" target="TARGET_ID" tween="true" startValue="1,1" endValue="1.18,1.18" duration="5" ease="Sine.Out"/>
  <item time="5" type="Scale" target="TARGET_ID" tween="true" startValue="1.18,1.18" endValue="1,1" duration="7" ease="Sine.InOut"/>
  <item time="18" type="Scale" target="TARGET_ID" tween="true" startValue="1,1" endValue="1.12,1.12" duration="4" ease="Sine.Out"/>
  <item time="22" type="Scale" target="TARGET_ID" tween="true" startValue="1.12,1.12" endValue="1,1" duration="6" ease="Sine.InOut"/>
</transition>
```

> 红点、未读提示、新消息徽记默认不要使用 `Alpha` 淡隐。需要更强提示时优先提高缩放幅度或缩短间隔，而不是把元素变透明。

## 参数说明

| 参数 | 当前值 | 说明 |
|------|--------|------|
| `frameRate` | `30` | 动画帧率 |
| `name` | `badgeBeat` | 自定义动效名称 |
| `autoPlay` | `true` | 自动播放 |
| `autoPlayRepeat` | `-1` | 无限循环 |
| `TARGET_ID` | 需替换 | 徽记、红点或角标 ID |
| 第一次放大 | `1.18,1.18` | 主心跳 |
| 第二次放大 | `1.12,1.12` | 次心跳 |
| 单轮长度 | 28帧/30fps ≈ 0.93秒 | 中等节奏 |

### 频率调节建议

| 场景 | 最大缩放 | 第二次缩放 | 体感 |
|------|----------|------------|------|
| 轻提示 | `1.10` | `1.06` | 克制 |
| 标准徽记 | `1.18` | `1.12` | 明显（默认） |
| 强提醒 | `1.28` | `1.18` | 更醒目 |

## 使用示例

```xml
<?xml version="1.0" encoding="utf-8"?>
<component size="100,100">
  <displayList>
    <image id="n8_badge" name="messageBadge" src="badgeRed" xy="74,22" size="24,24"/>
  </displayList>
  <transition name="badgeBeat" frameRate="30" autoPlay="true" autoPlayRepeat="-1">
    <item time="0" type="Scale" target="n8_badge" tween="true" startValue="1,1" endValue="1.18,1.18" duration="5" ease="Sine.Out"/>
    <item time="5" type="Scale" target="n8_badge" tween="true" startValue="1.18,1.18" endValue="1,1" duration="7" ease="Sine.InOut"/>
    <item time="18" type="Scale" target="n8_badge" tween="true" startValue="1,1" endValue="1.12,1.12" duration="4" ease="Sine.Out"/>
    <item time="22" type="Scale" target="n8_badge" tween="true" startValue="1.12,1.12" endValue="1,1" duration="6" ease="Sine.InOut"/>
  </transition>
</component>
```
