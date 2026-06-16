# 果冻效果 (Jelly Bounce)

## 效果描述

经典的果冻弹跳入场动画：元素出现时先横向拉伸并纵向压缩（压扁），然后弹性回弹过度，再平滑恢复到原始尺寸，最后以一个微妙的呼吸感收尾。整个过程使用 Sine 系列缓动函数，视觉上柔和丝滑。

**适用场景**：弹窗入场、消息框出现、卡片弹出、按钮点击反馈

## XML 代码

```xml
<transition name="__enter__" frameRate="30">
  <!-- 第1段：快速压扁 -->
  <item time="0" type="Scale" target="TARGET_ID" tween="true" startValue="1,1" endValue="1.05,0.90" duration="6" ease="Sine.Out"/>
  <!-- 第2段：弹性回弹（过冲） -->
  <item time="6" type="Scale" target="TARGET_ID" tween="true" startValue="1.05,0.90" endValue="0.97,1.03" duration="5" ease="Sine.InOut"/>
  <!-- 第3段：平滑恢复 -->
  <item time="11" type="Scale" target="TARGET_ID" tween="true" startValue="0.97,1.03" endValue="1,1" duration="5" ease="Sine.InOut"/>
  <!-- 第4段：微妙膨胀 -->
  <item time="16" type="Scale" target="TARGET_ID" tween="true" startValue="1,1" endValue="1.01,0.99" duration="4" ease="Sine.InOut"/>
  <!-- 第5段：精准归位 -->
  <item time="20" type="Scale" target="TARGET_ID" tween="true" startValue="1.01,0.99" endValue="1,1" duration="4" ease="Sine.Out"/>
</transition>
```

## 参数说明

| 参数 | 当前值 | 说明 |
|------|--------|------|
| `frameRate` | `30` | 动画帧率，可按项目需求改为 24 或 60 |
| `name` | `__enter__` | 自定义名称（本库入场命名约定）；引擎不会按名称自动播放，需在代码中 `GetTransition("__enter__").Play()` 触发，用于 Window 时置于 `DoShowAnimation` 内 |
| `TARGET_ID` | 需替换 | 目标元素的 ID，如 `n14_mosk` |
| 压扁幅度 | `1.05,0.90` | 横向拉伸5%，纵向压缩10% |
| 回弹幅度 | `0.97,1.03` | 横向收缩3%，纵向拉伸3% |
| 总时长 | 24帧/30fps = 0.80秒 | 可通过调整 duration 控制 |

### 幅度调节建议

| 场景 | 压扁幅度 | 回弹幅度 | 体感 |
|------|---------|---------|------|
| 轻柔弹窗 | `1.03,0.95` | `0.98,1.02` | 含蓄优雅 |
| 标准弹窗 | `1.05,0.90` | `0.97,1.03` | 适中（默认） |
| 活泼卡通 | `1.10,0.85` | `0.93,1.06` | 夸张有趣 |

## 使用示例

将 `TARGET_ID` 替换为实际元素 ID，放置在 `</displayList>` 之后：

```xml
<?xml version="1.0" encoding="utf-8"?>
<component size="1080,2160" overflow="scroll">
  <displayList>
    <component id="n13_mosk" name="loaderBg" src="fz3thc" fileName="组件/BasicBackground.xml" pkg="gmtl205q" xy="0,0">
      <relation target="" sidePair="width-width,height-height"/>
    </component>
    <component id="n14_mosk" name="frame" src="moskqfd" fileName="组件/MessageFrame.xml" xy="540,1000" size="960,452">
      <relation target="" sidePair="center-center,middle-middle"/>
    </component>
  </displayList>
  <transition name="__enter__" frameRate="30">
    <item time="0" type="Scale" target="n14_mosk" tween="true" startValue="1,1" endValue="1.05,0.90" duration="6" ease="Sine.Out"/>
    <item time="6" type="Scale" target="n14_mosk" tween="true" startValue="1.05,0.90" endValue="0.97,1.03" duration="5" ease="Sine.InOut"/>
    <item time="11" type="Scale" target="n14_mosk" tween="true" startValue="0.97,1.03" endValue="1,1" duration="5" ease="Sine.InOut"/>
    <item time="16" type="Scale" target="n14_mosk" tween="true" startValue="1,1" endValue="1.01,0.99" duration="4" ease="Sine.InOut"/>
    <item time="20" type="Scale" target="n14_mosk" tween="true" startValue="1.01,0.99" endValue="1,1" duration="4" ease="Sine.Out"/>
  </transition>
</component>
```

## 变体：配合透明度淡入

如果希望弹跳的同时有淡入效果，可在 transition 中追加 Alpha 项：

```xml
<transition name="__enter__" frameRate="30">
  <!-- 淡入 -->
  <item time="0" type="Alpha" target="TARGET_ID" tween="true" startValue="0" endValue="1" duration="8" ease="Sine.Out"/>
  <!-- 果冻缩放（同上） -->
  <item time="0" type="Scale" target="TARGET_ID" tween="true" startValue="1,1" endValue="1.05,0.90" duration="6" ease="Sine.Out"/>
  <item time="6" type="Scale" target="TARGET_ID" tween="true" startValue="1.05,0.90" endValue="0.97,1.03" duration="5" ease="Sine.InOut"/>
  <item time="11" type="Scale" target="TARGET_ID" tween="true" startValue="0.97,1.03" endValue="1,1" duration="5" ease="Sine.InOut"/>
  <item time="16" type="Scale" target="TARGET_ID" tween="true" startValue="1,1" endValue="1.01,0.99" duration="4" ease="Sine.InOut"/>
  <item time="20" type="Scale" target="TARGET_ID" tween="true" startValue="1.01,0.99" endValue="1,1" duration="4" ease="Sine.Out"/>
</transition>
```
