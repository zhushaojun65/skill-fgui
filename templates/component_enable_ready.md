# 单组件恢复可用 (Component Enable Ready)

## 效果描述

组件从禁用、锁定或冷却结束状态恢复可用时，先从略小状态弹回并轻微过冲，再回到正常尺寸，提示玩家现在可以操作。

**适用场景**：按钮冷却结束、技能可释放、奖励可领取、功能恢复可点击、列表项解除禁用

## XML 代码

```xml
<transition name="componentEnableReady" frameRate="30">
  <item time="0" type="Scale" target="TARGET_ID" tween="true" startValue="0.94,0.94" endValue="1.08,1.08" duration="5" ease="Back.Out"/>
  <item time="5" type="Scale" target="TARGET_ID" tween="true" startValue="1.08,1.08" endValue="1,1" duration="6" ease="Sine.InOut"/>
</transition>
```

> 此动效适合“恢复可用”的瞬间提示。不建议长期自动循环，避免和按钮本身的可点击状态混淆。

## 参数说明

| 参数 | 当前值 | 说明 |
|------|--------|------|
| `frameRate` | `30` | 动画帧率 |
| `name` | `componentEnableReady` | 自定义动效名称 |
| `TARGET_ID` | 需替换 | 目标组件或显示对象 ID |
| 缩放范围 | `0.94` → `1.08` → `1` | 从不可用感回弹到正常 |
| 总时长 | 11帧/30fps ≈ 0.37秒 | 快速提示 |

## 使用示例

```xml
<?xml version="1.0" encoding="utf-8"?>
<component size="120,120">
  <displayList>
    <component id="n2_skill" name="skillButton" src="skillBtn" fileName="组件/SkillButton.xml" xy="60,60" size="96,96"/>
  </displayList>
  <transition name="componentEnableReady" frameRate="30">
    <item time="0" type="Scale" target="n2_skill" tween="true" startValue="0.94,0.94" endValue="1.08,1.08" duration="5" ease="Back.Out"/>
    <item time="5" type="Scale" target="n2_skill" tween="true" startValue="1.08,1.08" endValue="1,1" duration="6" ease="Sine.InOut"/>
  </transition>
</component>
```
