# 单组件解锁弹出 (Component Unlock Pop)

## 效果描述

单个组件解锁或出现新状态时，从略小状态快速弹出，并带一个很轻的旋转回正。适合锁图标消失后，让功能入口、卡片或奖励格子自己做一次“可用了”的提示。

**适用场景**：功能解锁、章节解锁、道具解锁、新卡片出现、奖励格子激活

## XML 代码

```xml
<transition name="componentUnlockPop" frameRate="30">
  <item time="0" type="Scale" target="TARGET_ID" tween="true" startValue="0.70,0.70" endValue="1.12,1.12" duration="8" ease="Back.Out"/>
  <item time="8" type="Scale" target="TARGET_ID" tween="true" startValue="1.12,1.12" endValue="1,1" duration="6" ease="Sine.InOut"/>
  <item time="0" type="Rotation" target="TARGET_ID" tween="true" startValue="-6" endValue="3" duration="8" ease="Sine.Out"/>
  <item time="8" type="Rotation" target="TARGET_ID" tween="true" startValue="3" endValue="0" duration="6" ease="Sine.InOut"/>
</transition>
```

> 若目标组件是严格对齐的图标或文字，可删除 `Rotation` 两段，只保留缩放。

## 参数说明

| 参数 | 当前值 | 说明 |
|------|--------|------|
| `frameRate` | `30` | 动画帧率 |
| `name` | `componentUnlockPop` | 自定义动效名称 |
| `TARGET_ID` | 需替换 | 目标组件或显示对象 ID |
| 起始尺寸 | `0.70,0.70` | 从较小状态弹出 |
| 旋转范围 | `-6` → `3` → `0` | 轻微摆正，不影响读图 |

## 使用示例

```xml
<?xml version="1.0" encoding="utf-8"?>
<component size="180,180">
  <displayList>
    <component id="n7_card" name="featureCard" src="featureCard" fileName="组件/FeatureCard.xml" xy="90,90" size="150,150"/>
  </displayList>
  <transition name="componentUnlockPop" frameRate="30">
    <item time="0" type="Scale" target="n7_card" tween="true" startValue="0.70,0.70" endValue="1.12,1.12" duration="8" ease="Back.Out"/>
    <item time="8" type="Scale" target="n7_card" tween="true" startValue="1.12,1.12" endValue="1,1" duration="6" ease="Sine.InOut"/>
    <item time="0" type="Rotation" target="n7_card" tween="true" startValue="-6" endValue="3" duration="8" ease="Sine.Out"/>
    <item time="8" type="Rotation" target="n7_card" tween="true" startValue="3" endValue="0" duration="6" ease="Sine.InOut"/>
  </transition>
</component>
```
