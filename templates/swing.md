# 摇摆效果 (Swing)

## 效果描述

元素如钟摆般左右摇摆旋转，从中心轴交替向两侧偏转。使用 `Sine.InOut` 缓动确保摆动两端自然减速，配合多段动画实现"左→右→归位"的完整摇摆周期，持续循环。

**适用场景**：铃铛图标、通知提示、注意力引导、趣味装饰、角色待机动作

## XML 代码

```xml
<transition name="swing" frameRate="30" autoPlay="true" autoPlayRepeat="-1">
  <!-- 第1段：向左摆 -->
  <item time="0" type="Rotation" target="TARGET_ID" tween="true" startValue="0" endValue="-12" duration="10" ease="Sine.InOut"/>
  <!-- 第2段：向右摆（过中心） -->
  <item time="10" type="Rotation" target="TARGET_ID" tween="true" startValue="-12" endValue="12" duration="12" ease="Sine.InOut"/>
  <!-- 第3段：回到中心 -->
  <item time="22" type="Rotation" target="TARGET_ID" tween="true" startValue="12" endValue="0" duration="10" ease="Sine.InOut"/>
  <!-- 第4段：静止等待 -->
  <item time="32" type="Rotation" target="TARGET_ID" tween="true" startValue="0" endValue="0" duration="15"/>
</transition>
```

> **注意**：第4段静止等待（15帧 = 0.5秒）让摇摆有节奏感，避免持续不停看起来眩晕。摇摆基点（轴心）由业务在元件上配置（钟摆感常用顶部轴心），动效不假设也不修改。

## 参数说明

| 参数 | 当前值 | 说明 |
|------|--------|------|
| `frameRate` | `30` | 动画帧率 |
| `name` | `swing` | 自定义动效名称 |
| `autoPlay` | `true` | 自动播放 |
| `autoPlayRepeat` | `-1` | 无限循环 |
| `TARGET_ID` | 需替换 | 目标元素的 ID |
| 摇摆角度 | `±12°` | 左右各摆动12度 |
| 摆动周期 | 32帧 = 1.07秒 | 完整左→右→归位周期 |
| 静止等待 | 15帧 = 0.5秒 | 两次摇摆之间的间隔 |
| 总循环 | 47帧 ≈ 1.57秒 | 一次完整循环时长 |

### 幅度调节建议

| 场景 | 摇摆角度 | 静止等待 | 体感 |
|------|---------|----------|------|
| 微妙提示 | `±6°` | `20帧` | 低调含蓄 |
| 标准摇摆 | `±12°` | `15帧` | 明显可见（默认） |
| 夸张摆动 | `±25°` | `10帧` | 活泼卡通 |

> 摇摆基点（轴心）由业务在元件上自行配置，不同基点会产生钟摆、中心摆、不倒翁等不同效果，本模板不做假设。

## 使用示例

将 `TARGET_ID` 替换为实际元素 ID，放置在 `</displayList>` 之后：

```xml
<?xml version="1.0" encoding="utf-8"?>
<component size="100,100">
  <displayList>
    <image id="n4_bell" name="bell" src="icon_bell" xy="50,10" size="48,48"/>
  </displayList>
  <transition name="swing" frameRate="30" autoPlay="true" autoPlayRepeat="-1">
    <item time="0" type="Rotation" target="n4_bell" tween="true" startValue="0" endValue="-12" duration="10" ease="Sine.InOut"/>
    <item time="10" type="Rotation" target="n4_bell" tween="true" startValue="-12" endValue="12" duration="12" ease="Sine.InOut"/>
    <item time="22" type="Rotation" target="n4_bell" tween="true" startValue="12" endValue="0" duration="10" ease="Sine.InOut"/>
    <item time="32" type="Rotation" target="n4_bell" tween="true" startValue="0" endValue="0" duration="15"/>
  </transition>
</component>
```

## 变体：持续摇摆（无间歇）

去掉第4段静止等待，使用 yoyo 简化为双段循环：

```xml
<transition name="swing" frameRate="30" autoPlay="true" autoPlayRepeat="-1">
  <item time="0" type="Rotation" target="TARGET_ID" tween="true" startValue="-12" endValue="12" duration="12" ease="Sine.InOut" repeat="-1" yoyo="true"/>
</transition>
```
