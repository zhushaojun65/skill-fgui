# 单组件选中弹起 (Component Select Pop)

## 效果描述

单个组件进入选中态时快速放大、轻微过冲，再回到稳定选中尺寸。动效只改变目标组件自身的 `Scale`，适合 Tab、卡片、道具格、列表项等局部选中反馈。

**适用场景**：Tab 选中、卡片选中、背包格子选中、技能按钮选中、列表项聚焦

## XML 代码

```xml
<transition name="componentSelectPop" frameRate="30">
  <item time="0" type="Scale" target="TARGET_ID" tween="true" startValue="1,1" endValue="1.12,1.12" duration="4" ease="Sine.Out"/>
  <item time="4" type="Scale" target="TARGET_ID" tween="true" startValue="1.12,1.12" endValue="1.06,1.06" duration="6" ease="Back.Out"/>
</transition>
```

> 此动效不设 `autoPlay`，应在组件切到选中态时触发。缩放基点（轴心）由业务在元件上配置，动效不假设也不修改。

## 参数说明

| 参数 | 当前值 | 说明 |
|------|--------|------|
| `frameRate` | `30` | 动画帧率 |
| `name` | `componentSelectPop` | 自定义动效名称，通过代码或控制器触发 |
| `TARGET_ID` | 需替换 | 目标组件或显示对象 ID |
| 过冲尺寸 | `1.12,1.12` | 选中瞬间的最大放大值 |
| 稳定尺寸 | `1.06,1.06` | 选中态停留尺寸 |
| 总时长 | 10帧/30fps ≈ 0.33秒 | 短促明确 |

### 强度调节建议

| 场景 | 过冲尺寸 | 稳定尺寸 | 体感 |
|------|----------|----------|------|
| 轻量 UI | `1.06` | `1.03` | 克制 |
| 标准组件 | `1.12` | `1.06` | 明显（默认） |
| 游戏按钮 | `1.18` | `1.10` | 强反馈 |

## 使用示例

```xml
<?xml version="1.0" encoding="utf-8"?>
<component size="160,80">
  <displayList>
    <component id="n1_tab" name="tabItem" src="tab01" fileName="组件/TabItem.xml" xy="80,40" size="140,64"/>
  </displayList>
  <transition name="componentSelectPop" frameRate="30">
    <item time="0" type="Scale" target="n1_tab" tween="true" startValue="1,1" endValue="1.12,1.12" duration="4" ease="Sine.Out"/>
    <item time="4" type="Scale" target="n1_tab" tween="true" startValue="1.12,1.12" endValue="1.06,1.06" duration="6" ease="Back.Out"/>
  </transition>
</component>
```

### 代码触发示例（C#）

```csharp
component.GetTransition("componentSelectPop").Play();
```
