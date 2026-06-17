# 单组件数值跳动 (Component Value Bump)

## 效果描述

单个文本、图标或资源条数值变化时，先轻微上跳并放大，再回到原位。用于强调“数值刚刚发生变化”，不改变组件最终布局。

**适用场景**：金币数量变化、战力增加、等级提升、进度条数值刷新、购买数量变化

## XML 代码

```xml
<transition name="componentValueBump" frameRate="30">
  <item time="0" type="Scale" target="TARGET_ID" tween="true" startValue="1,1" endValue="1.18,1.18" duration="4" ease="Sine.Out"/>
  <item time="4" type="Scale" target="TARGET_ID" tween="true" startValue="1.18,1.18" endValue="1,1" duration="8" ease="Back.Out"/>
  <item time="0" type="XY" target="TARGET_ID" tween="true" startValue="-,0" endValue="-,-8" duration="4" ease="Sine.Out"/>
  <item time="4" type="XY" target="TARGET_ID" tween="true" startValue="-,-8" endValue="-,0" duration="8" ease="Back.Out"/>
</transition>
```

> **重要（坐标系）**：XY 动效的 `startValue`/`endValue` 是**绝对坐标**（相对父组件），`-` 表示该轴保持当前值不变，**不是**相对偏移。本模板假设目标元素最终 Y=0，跳动在 0 与 -8 之间；若元素实际 Y 不是 0，需把 Y 值替换为「实际 Y」与「实际 Y - 跳动高度」，否则元素会被瞬移到 Y=0。若不便换算坐标，可改用下方「仅缩放」版本。

## 参数说明

| 参数 | 当前值 | 说明 |
|------|--------|------|
| `frameRate` | `30` | 动画帧率 |
| `name` | `componentValueBump` | 自定义动效名称 |
| `TARGET_ID` | 需替换 | 目标文本、图标或组件 ID |
| 放大值 | `1.18,1.18` | 数值变化瞬间放大 |
| 跳动高度 | `8px` | Y 轴向上跳动 |
| 总时长 | 12帧/30fps = 0.40秒 | 清晰但不拖沓 |

### 变体：仅缩放数值跳动

如果组件所在项目不适合使用 `XY` 相对位移，可只保留缩放段：

```xml
<transition name="componentValueBump" frameRate="30">
  <item time="0" type="Scale" target="TARGET_ID" tween="true" startValue="1,1" endValue="1.18,1.18" duration="4" ease="Sine.Out"/>
  <item time="4" type="Scale" target="TARGET_ID" tween="true" startValue="1.18,1.18" endValue="1,1" duration="8" ease="Back.Out"/>
</transition>
```

## 使用示例

```xml
<?xml version="1.0" encoding="utf-8"?>
<component size="180,60">
  <displayList>
    <text id="n6_value" name="coinValue" xy="90,30" size="160,40" text="12345" fontSize="28" align="center"/>
  </displayList>
  <transition name="componentValueBump" frameRate="30">
    <item time="0" type="Scale" target="n6_value" tween="true" startValue="1,1" endValue="1.18,1.18" duration="4" ease="Sine.Out"/>
    <item time="4" type="Scale" target="n6_value" tween="true" startValue="1.18,1.18" endValue="1,1" duration="8" ease="Back.Out"/>
    <item time="0" type="XY" target="n6_value" tween="true" startValue="-,30" endValue="-,22" duration="4" ease="Sine.Out"/>
    <item time="4" type="XY" target="n6_value" tween="true" startValue="-,22" endValue="-,30" duration="8" ease="Back.Out"/>
  </transition>
</component>
```
