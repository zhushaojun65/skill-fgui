# 弹性缩放入场 (Scale Bounce In)

## 效果描述

元素从极小尺寸快速放大到正常尺寸，配合 `Back.Out` 缓动产生过冲回弹效果——先超过目标大小再弹回。同时伴随淡入，视觉冲击感强且活泼。

**适用场景**：弹窗出现、卡片弹出、成就解锁、奖励展示

## XML 代码

```xml
<transition name="__enter__" frameRate="30">
  <!-- 缩放：从0.3放大到1.0，Back.Out产生过冲回弹 -->
  <item time="0" type="Scale" target="TARGET_ID" tween="true" startValue="0.3,0.3" endValue="1,1" duration="12" ease="Back.Out"/>
  <!-- 淡入 -->
  <item time="0" type="Alpha" target="TARGET_ID" tween="true" startValue="0" endValue="1" duration="6" ease="Sine.Out"/>
</transition>
```

## 参数说明

| 参数 | 当前值 | 说明 |
|------|--------|------|
| `frameRate` | `30` | 动画帧率 |
| `name` | `__enter__` | 自定义名称（本库入场命名约定）；引擎不会按名称自动播放，需在代码中 `GetTransition("__enter__").Play()` 触发，用于 Window 时置于 `DoShowAnimation` 内 |
| `TARGET_ID` | 需替换 | 目标元素的 ID |
| 初始缩放 | `0.3,0.3` | 起始为正常大小的30% |
| 缩放帧数 | `12` | 12帧/30fps = 0.40秒 |
| 淡入帧数 | `6` | 6帧/30fps = 0.20秒（快速显现，避免幽灵感） |
| 缓动 | `Back.Out` | 过冲回弹（先冲过目标约 10% 再弹回；Back 内部参数 s≈1.70158） |

### 风格调节建议

| 场景 | 初始缩放 | duration | 缓动 | 体感 |
|------|---------|----------|------|------|
| 含蓄弹出 | `0.6,0.6` | `10` | `Back.Out` | 温和自然 |
| 标准弹出 | `0.3,0.3` | `12` | `Back.Out` | 活泼适中（默认） |
| 夸张弹出 | `0.1,0.1` | `15` | `Elastic.Out` | 强烈弹性 |

## 使用示例

将 `TARGET_ID` 替换为实际元素 ID，放置在 `</displayList>` 之后：

```xml
<?xml version="1.0" encoding="utf-8"?>
<component size="1080,2160" overflow="scroll">
  <displayList>
    <component id="n13_mosk" name="loaderBg" src="fz3thc" fileName="组件/BasicBackground.xml" pkg="gmtl205q" xy="0,0">
      <relation target="" sidePair="width-width,height-height"/>
    </component>
    <component id="n14_mosk" name="dialog" src="moskqfd" fileName="组件/RewardDialog.xml" xy="540,1080" size="800,600">
      <relation target="" sidePair="center-center,middle-middle"/>
    </component>
  </displayList>
  <transition name="__enter__" frameRate="30">
    <item time="0" type="Scale" target="n14_mosk" tween="true" startValue="0.3,0.3" endValue="1,1" duration="12" ease="Back.Out"/>
    <item time="0" type="Alpha" target="n14_mosk" tween="true" startValue="0" endValue="1" duration="6" ease="Sine.Out"/>
  </transition>
</component>
```

> 缩放基点（轴心）由业务在元件上配置，动效不假设也不修改。

## 变体：配合旋转的弹性入场

增加轻微旋转，适合装饰元素或趣味场景：

```xml
<transition name="__enter__" frameRate="30">
  <item time="0" type="Scale" target="TARGET_ID" tween="true" startValue="0.3,0.3" endValue="1,1" duration="12" ease="Back.Out"/>
  <item time="0" type="Alpha" target="TARGET_ID" tween="true" startValue="0" endValue="1" duration="6" ease="Sine.Out"/>
  <item time="0" type="Rotation" target="TARGET_ID" tween="true" startValue="-15" endValue="0" duration="12" ease="Back.Out"/>
</transition>
```
