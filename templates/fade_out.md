# 淡出效果 (Fade Out)

## 效果描述

最基础的退场动画：元素从完全不透明逐渐变为透明直至消失。使用 `Sine.In` 缓动函数，前期变化慢、后期加速消失，视觉上有一个柔和的"渐渐离去"感觉。

**适用场景**：通用退场、叠加层消失、页面切换、元素隐藏

## XML 代码

```xml
<transition name="__exit__" frameRate="30">
  <item time="0" type="Alpha" target="TARGET_ID" tween="true" startValue="1" endValue="0" duration="10" ease="Sine.In"/>
</transition>
```

## 参数说明

| 参数 | 当前值 | 说明 |
|------|--------|------|
| `frameRate` | `30` | 动画帧率 |
| `name` | `__exit__` | 自定义名称（本库退场命名约定）；引擎不会按名称自动播放，需在代码中 `GetTransition("__exit__").Play()` 触发，用于 Window 时置于 `DoHideAnimation` 内 |
| `TARGET_ID` | 需替换 | 目标元素的 ID |
| 持续帧数 | `10` | 10帧/30fps ≈ 0.33秒 |
| 缓动 | `Sine.In` | 慢进快出，起始柔和 |

### 速度调节建议

| 场景 | duration | 实际时长 | 体感 |
|------|----------|---------|------|
| 快速消失 | `6` | 0.20秒 | 干脆利落 |
| 标准淡出 | `10` | 0.33秒 | 适中（默认） |
| 缓慢消隐 | `18` | 0.60秒 | 优雅从容 |

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
  <transition name="__exit__" frameRate="30">
    <item time="0" type="Alpha" target="n10_panel" tween="true" startValue="1" endValue="0" duration="10" ease="Sine.In"/>
  </transition>
</component>
```

## 变体：配合缩放的淡出

缩小的同时淡出，增强"收拢消失"的感觉：

```xml
<transition name="__exit__" frameRate="30">
  <item time="0" type="Alpha" target="TARGET_ID" tween="true" startValue="1" endValue="0" duration="10" ease="Sine.In"/>
  <item time="0" type="Scale" target="TARGET_ID" tween="true" startValue="1,1" endValue="0.95,0.95" duration="10" ease="Sine.In"/>
</transition>
```
