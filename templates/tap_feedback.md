# 点击反馈 (Tap Feedback)

## 效果描述

经典的按钮点击反馈动画：元素快速缩小到 90%，然后弹性回弹到原始大小，配合 `Back.Out` 缓动产生过冲效果。整个过程极短（约0.3秒），给用户即时的触觉反馈感。

**适用场景**：按钮点击、可交互元素反馈、图标点击、Tab切换

## XML 代码

```xml
<transition name="tap" frameRate="30">
  <!-- 快速缩小 -->
  <item time="0" type="Scale" target="TARGET_ID" tween="true" startValue="1,1" endValue="0.90,0.90" duration="3" ease="Sine.Out"/>
  <!-- 弹性回弹 -->
  <item time="3" type="Scale" target="TARGET_ID" tween="true" startValue="0.90,0.90" endValue="1,1" duration="7" ease="Back.Out"/>
</transition>
```

> **注意**：此动效不设 `autoPlay`，应通过代码在点击时触发。缩放基点（轴心）由业务在元件上配置，动效不假设也不修改。

## 参数说明

| 参数 | 当前值 | 说明 |
|------|--------|------|
| `frameRate` | `30` | 动画帧率 |
| `name` | `tap` | 自定义动效名称，通过代码触发 |
| `TARGET_ID` | 需替换 | 目标元素的 ID |
| 缩小比例 | `0.90,0.90` | 缩小到90% |
| 缩小帧数 | `3` | 3帧/30fps = 0.10秒（极速按下） |
| 回弹帧数 | `7` | 7帧/30fps ≈ 0.23秒（弹性恢复） |
| 回弹缓动 | `Back.Out` | 过冲回弹效果 |
| 总时长 | 10帧/30fps ≈ 0.33秒 | 短促有力 |

### 强度调节建议

| 场景 | 缩小比例 | 回弹duration | 体感 |
|------|---------|-------------|------|
| 微妙反馈 | `0.95,0.95` | `5` | 含蓄，高端感 |
| 标准反馈 | `0.90,0.90` | `7` | 明显可感（默认） |
| 强烈反馈 | `0.82,0.82` | `9` | 夸张，卡通感 |

## 使用示例

将 `TARGET_ID` 替换为实际元素 ID，放置在 `</displayList>` 之后：

```xml
<?xml version="1.0" encoding="utf-8"?>
<component size="300,120">
  <displayList>
    <component id="n3_btn" name="btnConfirm" src="btn01" fileName="组件/ConfirmButton.xml" xy="150,60" size="260,80">
      <relation target="" sidePair="center-center,middle-middle"/>
    </component>
  </displayList>
  <transition name="tap" frameRate="30">
    <item time="0" type="Scale" target="n3_btn" tween="true" startValue="1,1" endValue="0.90,0.90" duration="3" ease="Sine.Out"/>
    <item time="3" type="Scale" target="n3_btn" tween="true" startValue="0.90,0.90" endValue="1,1" duration="7" ease="Back.Out"/>
  </transition>
</component>
```

### 代码触发示例（C#）

```csharp
btnConfirm.onClick.Add(() =>
{
    component.GetTransition("tap").Play();
});
```

## 变体：点击反馈 + 轻微过冲放大

点击时把回弹幅度调大一些，强化反馈感（不使用 ColorFilter）：

```xml
<transition name="tap" frameRate="30">
  <!-- 缩放反馈 -->
  <item time="0" type="Scale" target="TARGET_ID" tween="true" startValue="1,1" endValue="0.88,0.88" duration="3" ease="Sine.Out"/>
  <item time="3" type="Scale" target="TARGET_ID" tween="true" startValue="0.88,0.88" endValue="1.04,1.04" duration="5" ease="Back.Out"/>
  <item time="8" type="Scale" target="TARGET_ID" tween="true" startValue="1.04,1.04" endValue="1,1" duration="3" ease="Sine.InOut"/>
</transition>
```
