# 淡入效果 (Fade In)

## 效果描述

最基础的入场动画：元素从完全透明逐渐显现为完全不透明。使用 `Sine.Out` 缓动函数，前期变化快、后期趋于平缓，视觉上自然舒适。

**适用场景**：通用入场、页面切换、元素渐显、叠加层出现

## XML 代码

```xml
<transition name="__enter__" frameRate="30">
  <item time="0" type="Alpha" target="TARGET_ID" tween="true" startValue="0" endValue="1" duration="10" ease="Sine.Out"/>
</transition>
```

## 参数说明

| 参数 | 当前值 | 说明 |
|------|--------|------|
| `frameRate` | `30` | 动画帧率 |
| `name` | `__enter__` | 自定义名称（本库入场命名约定）；引擎不会按名称自动播放，需在代码中 `GetTransition("__enter__").Play()` 触发，用于 Window 时置于 `DoShowAnimation` 内 |
| `TARGET_ID` | 需替换 | 目标元素的 ID |
| 持续帧数 | `10` | 10帧/30fps ≈ 0.33秒 |
| 缓动 | `Sine.Out` | 快进慢出，末端柔和 |

### 速度调节建议

| 场景 | duration | 实际时长 | 体感 |
|------|----------|---------|------|
| 快速闪现 | `6` | 0.20秒 | 干脆利落 |
| 标准淡入 | `10` | 0.33秒 | 适中（默认） |
| 缓慢渐显 | `18` | 0.60秒 | 优雅从容 |

## 使用示例

将 `TARGET_ID` 替换为实际元素 ID，放置在 `</displayList>` 之后：

```xml
<?xml version="1.0" encoding="utf-8"?>
<component size="1080,2160">
  <displayList>
    <component id="n10_panel" name="panel" src="abc123" xy="0,0" size="1080,2160">
      <relation target="" sidePair="width-width,height-height"/>
    </component>
  </displayList>
  <transition name="__enter__" frameRate="30">
    <item time="0" type="Alpha" target="n10_panel" tween="true" startValue="0" endValue="1" duration="10" ease="Sine.Out"/>
  </transition>
</component>
```

## 变体：配合缩放的淡入

从略小尺寸渐显放大到正常尺寸，增添层次感：

```xml
<transition name="__enter__" frameRate="30">
  <item time="0" type="Alpha" target="TARGET_ID" tween="true" startValue="0" endValue="1" duration="10" ease="Sine.Out"/>
  <item time="0" type="Scale" target="TARGET_ID" tween="true" startValue="0.95,0.95" endValue="1,1" duration="10" ease="Sine.Out"/>
</transition>
```
