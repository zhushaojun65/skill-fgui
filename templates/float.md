# 浮动效果 (Float)

## 效果描述

元素在 Y 轴方向上缓慢地上下浮动，营造悬浮在空中的视觉效果。使用 `Sine.InOut` 缓动配合 `yoyo` 往返播放，上下运动的两端速度趋缓，视觉上平滑自然。

**适用场景**：悬浮按钮、角色立绘、装饰元素、引导箭头、空状态图标

## XML 代码

```xml
<transition name="float" frameRate="30" autoPlay="true" autoPlayRepeat="-1">
  <!-- Y轴浮动：XY 为绝对坐标（相对父组件）。Y 在原位与上浮点之间往返 -->
  <item time="0" type="XY" target="TARGET_ID" tween="true" startValue="-,0" endValue="-,-15" duration="36" ease="Sine.InOut" repeat="-1" yoyo="true"/>
</transition>
```

> **重要（坐标系）**：XY 动效的 `startValue`/`endValue` 是**绝对坐标**（相对父组件），`-` 表示该轴保持当前值不变，**不是**「相对当前位置的偏移」。即 `endValue="-,-15"` 表示 X 保持、Y 设为绝对坐标 -15。使用时需填入目标元素的绝对 Y 坐标：`startValue` 填元素最终 Y，`endValue` 填「最终 Y - 浮动幅度」（如最终 Y=0 → 上浮点 Y=-15）。

## 参数说明

| 参数 | 当前值 | 说明 |
|------|--------|------|
| `frameRate` | `30` | 动画帧率 |
| `name` | `float` | 自定义动效名称 |
| `autoPlay` | `true` | 自动播放 |
| `autoPlayRepeat` | `-1` | 无限循环 |
| `TARGET_ID` | 需替换 | 目标元素的 ID |
| Y轴浮动幅度 | `-15` | 起止点Y坐标之差（位移幅度）；填入时需换算为绝对坐标 |
| 单次帧数 | `36` | 36帧/30fps = 1.2秒（单程上浮或下落） |
| `yoyo` | `true` | 往返播放 |
| `repeat` | `-1` | 无限重复 |

### 幅度调节建议

| 场景 | 浮动幅度 | duration | 体感 |
|------|---------|----------|------|
| 微妙悬浮 | `-8` | `45` | 几乎不可察觉，精致 |
| 标准浮动 | `-15` | `36` | 明显可见（默认） |
| 大幅漂浮 | `-30` | `48` | 显著浮动，适合大元素 |

## 使用示例

将 `TARGET_ID` 替换为实际元素 ID，放置在 `</displayList>` 之后：

```xml
<?xml version="1.0" encoding="utf-8"?>
<component size="200,250">
  <displayList>
    <image id="n7_arrow" name="arrow" src="guide_arrow" xy="100,50" size="64,64"/>
  </displayList>
  <transition name="float" frameRate="30" autoPlay="true" autoPlayRepeat="-1">
    <item time="0" type="XY" target="n7_arrow" tween="true" startValue="-,0" endValue="-,-15" duration="36" ease="Sine.InOut" repeat="-1" yoyo="true"/>
  </transition>
</component>
```

## 变体：浮动 + 透明度呼吸

浮动的同时配合轻微透明度变化，增强梦幻感：

```xml
<transition name="float" frameRate="30" autoPlay="true" autoPlayRepeat="-1">
  <item time="0" type="XY" target="TARGET_ID" tween="true" startValue="-,0" endValue="-,-15" duration="36" ease="Sine.InOut" repeat="-1" yoyo="true"/>
  <item time="0" type="Alpha" target="TARGET_ID" tween="true" startValue="1" endValue="0.7" duration="36" ease="Sine.InOut" repeat="-1" yoyo="true"/>
</transition>
```

## 变体：水平浮动

修改 XY 方向即可做水平漂浮：

```xml
<transition name="floatH" frameRate="30" autoPlay="true" autoPlayRepeat="-1">
  <item time="0" type="XY" target="TARGET_ID" tween="true" startValue="0,-" endValue="10,-" duration="36" ease="Sine.InOut" repeat="-1" yoyo="true"/>
</transition>
```
