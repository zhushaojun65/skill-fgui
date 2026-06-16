# 缩小消失 (Scale Out)

## 效果描述

元素从正常尺寸缩小到极小并消失，同时伴随淡出效果。使用 `Sine.In` 缓动函数，起始缓慢、末端加速收缩，产生被"吸走"的视觉感受。与 `scale_bounce_in`（弹性缩放入场）配对使用效果最佳。

**适用场景**：弹窗关闭、卡片消失、元素移除、对话框退场

## XML 代码

```xml
<transition name="__exit__" frameRate="30">
  <!-- 缩小到0.3 -->
  <item time="0" type="Scale" target="TARGET_ID" tween="true" startValue="1,1" endValue="0.3,0.3" duration="10" ease="Sine.In"/>
  <!-- 同步淡出 -->
  <item time="0" type="Alpha" target="TARGET_ID" tween="true" startValue="1" endValue="0" duration="10" ease="Sine.In"/>
</transition>
```

## 参数说明

| 参数 | 当前值 | 说明 |
|------|--------|------|
| `frameRate` | `30` | 动画帧率 |
| `name` | `__exit__` | 自定义名称（本库退场命名约定）；引擎不会按名称自动播放，需在代码中 `GetTransition("__exit__").Play()` 触发，用于 Window 时置于 `DoHideAnimation` 内 |
| `TARGET_ID` | 需替换 | 目标元素的 ID |
| 目标缩放 | `0.3,0.3` | 最终缩小到正常大小的30% |
| 缩放帧数 | `10` | 10帧/30fps ≈ 0.33秒 |
| 缓动 | `Sine.In` | 慢进快出，末端加速 |

### 风格调节建议

| 场景 | 目标缩放 | duration | 体感 |
|------|---------|----------|------|
| 温和收缩 | `0.6,0.6` | `8` | 含蓄自然 |
| 标准缩小 | `0.3,0.3` | `10` | 适中（默认） |
| 完全消失 | `0,0` | `12` | 彻底吸走 |

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
  <transition name="__exit__" frameRate="30">
    <item time="0" type="Scale" target="n14_mosk" tween="true" startValue="1,1" endValue="0.3,0.3" duration="10" ease="Sine.In"/>
    <item time="0" type="Alpha" target="n14_mosk" tween="true" startValue="1" endValue="0" duration="10" ease="Sine.In"/>
  </transition>
</component>
```

> 缩放基点（轴心）由业务在元件上配置，动效不假设也不修改。

## 变体：配合 Back.In 的吸入效果

使用 `Back.In` 缓动，退场前会先略微膨胀再缩小，增加戏剧性：

```xml
<transition name="__exit__" frameRate="30">
  <item time="0" type="Scale" target="TARGET_ID" tween="true" startValue="1,1" endValue="0.1,0.1" duration="12" ease="Back.In"/>
  <item time="0" type="Alpha" target="TARGET_ID" tween="true" startValue="1" endValue="0" duration="12" ease="Sine.In"/>
</transition>
```
