# 闪烁高亮 (Flash)

## 效果描述

元素快速进行明暗闪烁，透明度在正常和低值之间急促切换数次后恢复，形成"闪光"视觉效果。闪烁频率逐渐降低（衰减），从急促过渡到平稳，最终恢复原状。

**适用场景**：获得道具、解锁提示、数值变化强调、新元素出现高亮、限时提示

## XML 代码

```xml
<transition name="flash" frameRate="30">
  <!-- 第1次闪烁：急促 -->
  <item time="0" type="Alpha" target="TARGET_ID" tween="true" startValue="1" endValue="0.2" duration="3" ease="Sine.Out"/>
  <item time="3" type="Alpha" target="TARGET_ID" tween="true" startValue="0.2" endValue="1" duration="3" ease="Sine.In"/>
  <!-- 第2次闪烁 -->
  <item time="6" type="Alpha" target="TARGET_ID" tween="true" startValue="1" endValue="0.2" duration="3" ease="Sine.Out"/>
  <item time="9" type="Alpha" target="TARGET_ID" tween="true" startValue="0.2" endValue="1" duration="3" ease="Sine.In"/>
  <!-- 第3次闪烁：渐缓 -->
  <item time="12" type="Alpha" target="TARGET_ID" tween="true" startValue="1" endValue="0.4" duration="4" ease="Sine.Out"/>
  <item time="16" type="Alpha" target="TARGET_ID" tween="true" startValue="0.4" endValue="1" duration="4" ease="Sine.In"/>
</transition>
```

> **注意**：此动效不设 `autoPlay`，应通过代码在需要强调时触发。闪烁幅度逐次衰减（0.2 → 0.2 → 0.4），避免视觉疲劳。

## 参数说明

| 参数 | 当前值 | 说明 |
|------|--------|------|
| `frameRate` | `30` | 动画帧率 |
| `name` | `flash` | 自定义动效名称，通过代码触发 |
| `TARGET_ID` | 需替换 | 目标元素的 ID |
| 最低透明度 | `0.2` → `0.4` | 逐次衰减，从强到弱 |
| 闪烁次数 | 3次 | 注意力引导而不刺眼 |
| 单次帧数 | `6` → `8` | 逐次变慢，产生衰减感 |
| 总时长 | 20帧/30fps ≈ 0.67秒 | 短促有冲击力 |

### 强度调节建议

| 场景 | 闪烁次数 | 最低透明度 | 体感 |
|------|---------|-----------|------|
| 微妙提示 | 2次 | `0.5` | 低调闪烁 |
| 标准高亮 | 3次 | `0.2` | 明显可见（默认） |
| 强烈闪光 | 4次 | `0.05` | 非常醒目 |

## 使用示例

将 `TARGET_ID` 替换为实际元素 ID，放置在 `</displayList>` 之后：

```xml
<?xml version="1.0" encoding="utf-8"?>
<component size="200,200">
  <displayList>
    <image id="n5_item" name="itemIcon" src="item_gold" xy="100,100" size="120,120"/>
  </displayList>
  <transition name="flash" frameRate="30">
    <item time="0" type="Alpha" target="n5_item" tween="true" startValue="1" endValue="0.2" duration="3" ease="Sine.Out"/>
    <item time="3" type="Alpha" target="n5_item" tween="true" startValue="0.2" endValue="1" duration="3" ease="Sine.In"/>
    <item time="6" type="Alpha" target="n5_item" tween="true" startValue="1" endValue="0.2" duration="3" ease="Sine.Out"/>
    <item time="9" type="Alpha" target="n5_item" tween="true" startValue="0.2" endValue="1" duration="3" ease="Sine.In"/>
    <item time="12" type="Alpha" target="n5_item" tween="true" startValue="1" endValue="0.4" duration="4" ease="Sine.Out"/>
    <item time="16" type="Alpha" target="n5_item" tween="true" startValue="0.4" endValue="1" duration="4" ease="Sine.In"/>
  </transition>
</component>
```

### 代码触发示例（C#）

```csharp
// 获得道具时闪烁高亮
void OnItemObtained(GComponent ownerComponent)
{
    ownerComponent.GetTransition("flash").Play();
}
```

## 变体：闪烁 + 缩放强调

闪烁时叠加轻微缩放，增强冲击力（避免使用 ColorFilter，效果不稳定）：

```xml
<transition name="flashGlow" frameRate="30">
  <!-- 透明度闪烁 -->
  <item time="0" type="Alpha" target="TARGET_ID" tween="true" startValue="1" endValue="0.2" duration="3" ease="Sine.Out"/>
  <item time="3" type="Alpha" target="TARGET_ID" tween="true" startValue="0.2" endValue="1" duration="3" ease="Sine.In"/>
  <item time="6" type="Alpha" target="TARGET_ID" tween="true" startValue="1" endValue="0.2" duration="3" ease="Sine.Out"/>
  <item time="9" type="Alpha" target="TARGET_ID" tween="true" startValue="0.2" endValue="1" duration="3" ease="Sine.In"/>
  <!-- 缩放强调 -->
  <item time="0" type="Scale" target="TARGET_ID" tween="true" startValue="1,1" endValue="1.08,1.08" duration="3" ease="Sine.Out"/>
  <item time="3" type="Scale" target="TARGET_ID" tween="true" startValue="1.08,1.08" endValue="1,1" duration="3" ease="Sine.In"/>
</transition>
```
