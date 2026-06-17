# 向下滑出 (Slide Down)

## 效果描述

元素从当前位置向下方滑出，同时伴随淡出效果。使用 `Sine.In` 缓动函数，起始缓慢、末端加速，产生自然的"下沉离去"感觉。与 `slide_up`（底部滑入）配对使用效果最佳。

**适用场景**：弹窗退场、底部面板关闭、操作菜单收起、通知消失

## XML 代码

```xml
<transition name="__exit__" frameRate="30">
  <!-- 向下滑出：XY 为绝对坐标（相对父组件）。Y 从当前位置动到屏幕外 -->
  <item time="0" type="XY" target="TARGET_ID" tween="true" startValue="-,0" endValue="-,300" duration="10" ease="Sine.In"/>
  <!-- 同步淡出 -->
  <item time="0" type="Alpha" target="TARGET_ID" tween="true" startValue="1" endValue="0" duration="10" ease="Sine.In"/>
</transition>
```

> **重要（坐标系）**：XY 动效的 `startValue`/`endValue` 是**绝对坐标**（相对父组件），`-` 表示该轴保持当前值不变。即 `endValue="-,300"` 表示 X 保持、Y 设为绝对坐标 300，**不是**「在当前位置 +300」。退场动效中 `startValue` 通常填目标元素当前最终 Y，`endValue` 填「最终 Y + 滑出偏移」（如当前 Y=0 → 终点 Y=300）。建议与 `slide_up` 的位移幅度保持一致。

## 参数说明

| 参数 | 当前值 | 说明 |
|------|--------|------|
| `frameRate` | `30` | 动画帧率 |
| `name` | `__exit__` | 自定义名称（本库退场命名约定）；引擎不会按名称自动播放，需在代码中 `GetTransition("__exit__").Play()` 触发，用于 Window 时置于 `DoHideAnimation` 内 |
| `TARGET_ID` | 需替换 | 目标元素的 ID |
| Y轴起止 | `0`→`300` | 绝对Y坐标（相对父组件）；`0`=当前最终Y，`300`=滑出终点Y |
| 滑动帧数 | `10` | 10帧/30fps ≈ 0.33秒 |
| 淡出帧数 | `10` | 与滑动同步 |

### 偏移量调节建议

| 场景 | Y偏移 | duration | 体感 |
|------|-------|----------|------|
| 小元素/通知条 | `150` | `8` | 轻巧快速 |
| 标准弹窗 | `300` | `10` | 适中（默认） |
| 全屏面板 | `500` | `12` | 大气沉稳 |

## 使用示例

将 `TARGET_ID` 替换为实际元素 ID，放置在 `</displayList>` 之后：

```xml
<?xml version="1.0" encoding="utf-8"?>
<component size="1080,2160" overflow="scroll">
  <displayList>
    <component id="n13_mosk" name="loaderBg" src="fz3thc" fileName="组件/BasicBackground.xml" pkg="gmtl205q" xy="0,0">
      <relation target="" sidePair="width-width,height-height"/>
    </component>
    <component id="n14_mosk" name="panel" src="moskqfd" fileName="组件/BottomPanel.xml" xy="60,1600" size="960,500">
      <relation target="" sidePair="center-center"/>
    </component>
  </displayList>
  <transition name="__exit__" frameRate="30">
    <item time="0" type="XY" target="n14_mosk" tween="true" startValue="-,1600" endValue="-,1900" duration="10" ease="Sine.In"/>
    <item time="0" type="Alpha" target="n14_mosk" tween="true" startValue="1" endValue="0" duration="10" ease="Sine.In"/>
  </transition>
</component>
```

## 变体：向右滑出

修改 XY 方向即可改变滑出方向：

```xml
<transition name="__exit__" frameRate="30">
  <!-- 向右滑出 -->
  <item time="0" type="XY" target="TARGET_ID" tween="true" startValue="0,-" endValue="400,-" duration="10" ease="Sine.In"/>
  <item time="0" type="Alpha" target="TARGET_ID" tween="true" startValue="1" endValue="0" duration="10" ease="Sine.In"/>
</transition>
```
