# 向上飞出 (Fly Up)

## 效果描述

元素从当前位置快速向上飞出屏幕，同时伴随缩小和淡出效果。使用 `Back.In` 缓动函数，先微微下沉蓄力再快速向上弹出，赋予元素一种"被发射出去"的动感。

**适用场景**：消息发送、元素移除、气泡飞出、收藏动画、点赞飞出

## XML 代码

```xml
<transition name="__exit__" frameRate="30">
  <!-- 向上飞出：XY 为绝对坐标（相对父组件）。Y 从当前位置动到屏幕外 -->
  <item time="0" type="XY" target="TARGET_ID" tween="true" startValue="-,0" endValue="-,-500" duration="12" ease="Back.In"/>
  <!-- 同步缩小 -->
  <item time="0" type="Scale" target="TARGET_ID" tween="true" startValue="1,1" endValue="0.5,0.5" duration="12" ease="Sine.In"/>
  <!-- 淡出 -->
  <item time="3" type="Alpha" target="TARGET_ID" tween="true" startValue="1" endValue="0" duration="9" ease="Sine.In"/>
</transition>
```

> **重要（坐标系）**：XY 动效的 `startValue`/`endValue` 是**绝对坐标**（相对父组件），`-` 表示该轴保持当前值不变。即 `endValue="-,-500"` 表示 X 保持、Y 设为绝对坐标 -500，**不是**「在当前位置 -500」。退场时 `startValue` 填目标元素当前最终 Y，`endValue` 填「最终 Y - 飞出偏移」（如当前 Y=0 → 终点 Y=-500）。`Back.In` 缓动会让元素先向反方向微移再加速飞出，产生蓄力效果。

## 参数说明

| 参数 | 当前值 | 说明 |
|------|--------|------|
| `frameRate` | `30` | 动画帧率 |
| `name` | `__exit__` | 自定义名称（本库退场命名约定）；引擎不会按名称自动播放，需在代码中 `GetTransition("__exit__").Play()` 触发，用于 Window 时置于 `DoHideAnimation` 内 |
| `TARGET_ID` | 需替换 | 目标元素的 ID |
| Y轴起止 | `0`→`-500` | 绝对Y坐标（相对父组件）；`0`=当前最终Y，`-500`=飞出终点Y |
| 飞出帧数 | `12` | 12帧/30fps = 0.40秒 |
| 缩小终值 | `0.5,0.5` | 飞出时缩小到50% |
| 淡出延迟 | `3` | 延迟3帧再淡出，先看到飞出动作 |

### 偏移量调节建议

| 场景 | Y偏移 | duration | 缩小终值 | 体感 |
|------|-------|----------|---------|------|
| 轻巧飞出 | `-300` | `10` | `0.6,0.6` | 快而含蓄 |
| 标准飞出 | `-500` | `12` | `0.5,0.5` | 适中（默认） |
| 远距飞出 | `-800` | `15` | `0.3,0.3` | 大幅远飞 |

## 使用示例

将 `TARGET_ID` 替换为实际元素 ID，放置在 `</displayList>` 之后：

```xml
<?xml version="1.0" encoding="utf-8"?>
<component size="400,400">
  <displayList>
    <image id="n6_bubble" name="bubble" src="chat_bubble" xy="200,300" size="80,80"/>
  </displayList>
  <transition name="__exit__" frameRate="30">
    <item time="0" type="XY" target="n6_bubble" tween="true" startValue="-,300" endValue="-,-200" duration="12" ease="Back.In"/>
    <item time="0" type="Scale" target="n6_bubble" tween="true" startValue="1,1" endValue="0.5,0.5" duration="12" ease="Sine.In"/>
    <item time="3" type="Alpha" target="n6_bubble" tween="true" startValue="1" endValue="0" duration="9" ease="Sine.In"/>
  </transition>
</component>
```

> 缩放基点（轴心）由业务在元件上配置，动效不假设也不修改。

## 变体：纯淡出飞出（无缩放）

去掉缩放，更加简洁：

```xml
<transition name="__exit__" frameRate="30">
  <item time="0" type="XY" target="TARGET_ID" tween="true" startValue="-,0" endValue="-,-500" duration="12" ease="Back.In"/>
  <item time="3" type="Alpha" target="TARGET_ID" tween="true" startValue="1" endValue="0" duration="9" ease="Sine.In"/>
</transition>
```
