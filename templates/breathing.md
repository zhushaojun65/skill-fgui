# 呼吸效果 (Breathing)

## 效果描述

持续的缓慢缩放循环：元素在 0.96 和 1.04 之间微妙地放大缩小，如同呼吸般有节奏感。使用 `Sine.InOut` 缓动确保过渡两端柔和自然，配合 `yoyo` 实现往返播放。

**适用场景**：关注引导、按钮提示、可交互元素高亮、等待状态

## XML 代码

```xml
<transition name="breathing" frameRate="30" autoPlay="true" autoPlayRepeat="-1">
  <item time="0" type="Scale" target="TARGET_ID" tween="true" startValue="0.96,0.96" endValue="1.04,1.04" duration="30" ease="Sine.InOut" repeat="-1" yoyo="true"/>
</transition>
```

## 参数说明

| 参数 | 当前值 | 说明 |
|------|--------|------|
| `frameRate` | `30` | 动画帧率 |
| `name` | `breathing` | 自定义动效名称 |
| `autoPlay` | `true` | 自动播放 |
| `autoPlayRepeat` | `-1` | 无限循环 |
| `TARGET_ID` | 需替换 | 目标元素的 ID |
| 缩放范围 | `0.96` ↔ `1.04` | ±4% 的缩放幅度 |
| 单次帧数 | `30` | 30帧/30fps = 1.0秒（单程） |
| `yoyo` | `true` | 往返播放 |
| `repeat` | `-1` | 无限重复 |

### 幅度调节建议

| 场景 | 缩放范围 | duration | 体感 |
|------|---------|----------|------|
| 微妙呼吸 | `0.98` ↔ `1.02` | `36` | 几乎不可察觉，高端感 |
| 标准呼吸 | `0.96` ↔ `1.04` | `30` | 明显可见（默认） |
| 醒目脉动 | `0.90` ↔ `1.10` | `24` | 强烈引导注意力 |

## 使用示例

将 `TARGET_ID` 替换为实际元素 ID，放置在 `</displayList>` 之后：

```xml
<?xml version="1.0" encoding="utf-8"?>
<component size="200,200">
  <displayList>
    <image id="n5_icon" name="icon" src="rpmb6" xy="100,100" size="120,120"/>
  </displayList>
  <transition name="breathing" frameRate="30" autoPlay="true" autoPlayRepeat="-1">
    <item time="0" type="Scale" target="n5_icon" tween="true" startValue="0.96,0.96" endValue="1.04,1.04" duration="30" ease="Sine.InOut" repeat="-1" yoyo="true"/>
  </transition>
</component>
```

> 轴心、锚点等基点属性由业务在元件上自行配置，本模板不做假设。

## 变体：呼吸 + 透明度变化

配合 Alpha，呼吸时同步产生轻微的明暗（透明度）变化：

```xml
<transition name="breathing" frameRate="30" autoPlay="true" autoPlayRepeat="-1">
  <item time="0" type="Scale" target="TARGET_ID" tween="true" startValue="0.96,0.96" endValue="1.04,1.04" duration="30" ease="Sine.InOut" repeat="-1" yoyo="true"/>
  <item time="0" type="Alpha" target="TARGET_ID" tween="true" startValue="1" endValue="0.85" duration="30" ease="Sine.InOut" repeat="-1" yoyo="true"/>
</transition>
```
