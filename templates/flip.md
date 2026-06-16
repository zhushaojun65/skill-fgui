# 翻转效果 (Flip)

## 效果描述

通过 X 轴缩放模拟卡片水平翻转效果：元素先沿 X 轴缩放到 0（"侧面"不可见状态），然后从 0 恢复到正常宽度，视觉上如同卡片翻转了一面。翻转中点可配合内容切换，实现"翻牌"效果。

**适用场景**：卡片翻转、抽卡揭示、状态切换（正面/反面）、答案揭晓

## XML 代码

```xml
<transition name="flip" frameRate="30">
  <!-- 前半段：正面缩小到消失 -->
  <item time="0" type="Scale" target="TARGET_ID" tween="true" startValue="1,1" endValue="0,1" duration="8" ease="Sine.In"/>
  <!-- 后半段：反面从消失恢复 -->
  <item time="8" type="Scale" target="TARGET_ID" tween="true" startValue="0,1" endValue="1,1" duration="8" ease="Sine.Out" label="flip_mid"/>
</transition>
```

> **注意**：翻转在第8帧到达 `0,1` 状态（元素宽度为0），此时在代码中切换显示内容即可实现真实的翻牌效果。翻转基点（轴心）由业务在元件上配置，动效不假设也不修改。

## 参数说明

| 参数 | 当前值 | 说明 |
|------|--------|------|
| `frameRate` | `30` | 动画帧率 |
| `name` | `flip` | 自定义动效名称，通过代码触发 |
| `TARGET_ID` | 需替换 | 目标元素的 ID |
| 翻转中点 | 第8帧 | X缩放为0的时刻，可在此切换内容 |
| 前半段 | `1→0`，8帧 | 正面缩小消失 |
| 后半段 | `0→1`，8帧 | 反面展开显示 |
| 总时长 | 16帧/30fps ≈ 0.53秒 | 流畅自然 |

### 速度调节建议

| 场景 | 单侧duration | 总时长 | 体感 |
|------|-------------|--------|------|
| 快速翻转 | `6` | 0.40秒 | 干脆利落 |
| 标准翻转 | `8` | 0.53秒 | 流畅自然（默认） |
| 缓慢揭示 | `12` | 0.80秒 | 悬念感强 |

## 使用示例

将 `TARGET_ID` 替换为实际元素 ID，放置在 `</displayList>` 之后：

```xml
<?xml version="1.0" encoding="utf-8"?>
<component size="300,400">
  <displayList>
    <component id="n6_card" name="card" src="card01" fileName="组件/GameCard.xml" xy="150,200" size="240,340">
      <relation target="" sidePair="center-center,middle-middle"/>
    </component>
  </displayList>
  <transition name="flip" frameRate="30">
    <item time="0" type="Scale" target="n6_card" tween="true" startValue="1,1" endValue="0,1" duration="8" ease="Sine.In"/>
    <item time="8" type="Scale" target="n6_card" tween="true" startValue="0,1" endValue="1,1" duration="8" ease="Sine.Out" label="flip_mid"/>
  </transition>
</component>
```

### 代码触发示例（C#）——翻牌切换内容

```csharp
var flipTrans = card.GetTransition("flip");

// 在翻转中点切换内容
flipTrans.SetHook("flip_mid", () =>
{
    // 切换正面/反面显示
    cardFront.visible = !cardFront.visible;
    cardBack.visible = !cardBack.visible;
});

flipTrans.Play();
```

> **提示**：FairyGUI Transition 支持在特定帧设置 Hook 回调，可以精确控制翻转中点的内容切换时机。

## 变体：垂直翻转

将 X 轴缩放改为 Y 轴缩放，实现上下翻转：

```xml
<transition name="flipV" frameRate="30">
  <!-- 前半段：纵向缩小到消失 -->
  <item time="0" type="Scale" target="TARGET_ID" tween="true" startValue="1,1" endValue="1,0" duration="8" ease="Sine.In"/>
  <!-- 后半段：纵向恢复 -->
  <item time="8" type="Scale" target="TARGET_ID" tween="true" startValue="1,0" endValue="1,1" duration="8" ease="Sine.Out"/>
</transition>
```

## 变体：翻转 + 淡入淡出

在翻转中点配合透明度变化，避免 X 缩放为 0 时的视觉瑕疵：

```xml
<transition name="flip" frameRate="30">
  <!-- 缩放翻转 -->
  <item time="0" type="Scale" target="TARGET_ID" tween="true" startValue="1,1" endValue="0,1" duration="8" ease="Sine.In"/>
  <item time="8" type="Scale" target="TARGET_ID" tween="true" startValue="0,1" endValue="1,1" duration="8" ease="Sine.Out" label="flip_mid"/>
  <!-- 中点附近透明度辅助 -->
  <item time="5" type="Alpha" target="TARGET_ID" tween="true" startValue="1" endValue="0" duration="3" ease="Sine.In"/>
  <item time="8" type="Alpha" target="TARGET_ID" tween="true" startValue="0" endValue="1" duration="3" ease="Sine.Out"/>
</transition>
```
