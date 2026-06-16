# 旋转加载 (Spin Loop)

## 效果描述

元素持续匀速旋转，形成经典的加载指示器效果。使用 `Linear` 缓动确保旋转速度恒定无变速，配合 `repeat="-1"` 实现无限循环。简洁高效，适用于所有 Loading 场景。

**适用场景**：加载指示器、刷新图标、处理中状态、同步图标

## XML 代码

```xml
<transition name="spin" frameRate="30" autoPlay="true" autoPlayRepeat="-1">
  <item time="0" type="Rotation" target="TARGET_ID" tween="true" startValue="0" endValue="360" duration="30" ease="Linear" repeat="-1"/>
</transition>
```

> **注意**：`endValue="360"` 表示顺时针旋转一整圈。旋转基点（轴心）由业务在元件上自行配置，本模板不做假设。

## 参数说明

| 参数 | 当前值 | 说明 |
|------|--------|------|
| `frameRate` | `30` | 动画帧率 |
| `name` | `spin` | 自定义动效名称 |
| `autoPlay` | `true` | 自动播放 |
| `autoPlayRepeat` | `-1` | 无限循环 |
| `TARGET_ID` | 需替换 | 目标元素的 ID |
| 旋转角度 | `0` → `360` | 顺时针旋转360度 |
| 单圈帧数 | `30` | 30帧/30fps = 1.0秒/圈 |
| `repeat` | `-1` | 无限重复 |
| `ease` | `Linear` | 匀速旋转 |

### 速度调节建议

| 场景 | duration | 体感 |
|------|----------|------|
| 快速加载 | `20` | 0.67秒/圈，紧迫感 |
| 标准加载 | `30` | 1.0秒/圈（默认） |
| 缓慢旋转 | `60` | 2.0秒/圈，沉稳 |

## 使用示例

将 `TARGET_ID` 替换为实际元素 ID，放置在 `</displayList>` 之后：

```xml
<?xml version="1.0" encoding="utf-8"?>
<component size="120,120">
  <displayList>
    <image id="n2_loader" name="loader" src="loading_icon" xy="60,60" size="80,80"/>
  </displayList>
  <transition name="spin" frameRate="30" autoPlay="true" autoPlayRepeat="-1">
    <item time="0" type="Rotation" target="n2_loader" tween="true" startValue="0" endValue="360" duration="30" ease="Linear" repeat="-1"/>
  </transition>
</component>
```

> **重要**：旋转基点（轴心）由业务在元件上配置，动效不假设也不修改。

## 变体：逆时针旋转

将 endValue 改为负值即可：

```xml
<transition name="spin" frameRate="30" autoPlay="true" autoPlayRepeat="-1">
  <item time="0" type="Rotation" target="TARGET_ID" tween="true" startValue="0" endValue="-360" duration="30" ease="Linear" repeat="-1"/>
</transition>
```

## 变体：变速旋转（呼吸感）

使用 `Sine.InOut` 缓动，旋转时会有快慢变化，更有韵律：

```xml
<transition name="spin" frameRate="30" autoPlay="true" autoPlayRepeat="-1">
  <item time="0" type="Rotation" target="TARGET_ID" tween="true" startValue="0" endValue="360" duration="36" ease="Sine.InOut" repeat="-1"/>
</transition>
```
