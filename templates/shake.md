# 抖动效果 (Shake)

## 效果描述

元素快速左右水平抖动数次后恢复原位，用于提示操作失败或输入错误。使用多段短促位移实现"左→右→左→右→归位"的快速震颤，整体时长极短（约0.4秒），给用户即时反馈而不打扰。

**适用场景**：密码错误、输入验证失败、操作拒绝、表单错误、余额不足提示

## XML 代码

```xml
<transition name="shake" frameRate="30">
  <!-- 第1段：快速左移 -->
  <item time="0" type="XY" target="TARGET_ID" tween="true" startValue="0,-" endValue="-12,-" duration="2" ease="Sine.Out"/>
  <!-- 第2段：弹到右侧 -->
  <item time="2" type="XY" target="TARGET_ID" tween="true" startValue="-12,-" endValue="10,-" duration="2" ease="Sine.InOut"/>
  <!-- 第3段：回弹到左侧（幅度减小） -->
  <item time="4" type="XY" target="TARGET_ID" tween="true" startValue="10,-" endValue="-8,-" duration="2" ease="Sine.InOut"/>
  <!-- 第4段：弹到右侧（幅度继续减小） -->
  <item time="6" type="XY" target="TARGET_ID" tween="true" startValue="-8,-" endValue="5,-" duration="2" ease="Sine.InOut"/>
  <!-- 第5段：回归原位 -->
  <item time="8" type="XY" target="TARGET_ID" tween="true" startValue="5,-" endValue="0,-" duration="3" ease="Sine.Out"/>
</transition>
```

> **重要（坐标系）**：XY 动效的 `startValue`/`endValue` 是**绝对坐标**（相对父组件），`-` 表示该轴保持当前值不变。本模板假设目标元素最终 X=0，抖动在 0 与 ±12/±10/±8/±5 之间衰减；若元素实际 X 不是 0，需把所有 X 值替换为「实际 X ± 抖动幅度」，否则元素会被瞬移到 X=0。抖动幅度逐段衰减（12→10→8→5→0），模拟自然的阻尼振荡。此动效不设 `autoPlay`，应通过代码在需要时触发。

## 参数说明

| 参数 | 当前值 | 说明 |
|------|--------|------|
| `frameRate` | `30` | 动画帧率 |
| `name` | `shake` | 自定义动效名称，通过代码触发 |
| `TARGET_ID` | 需替换 | 目标元素的 ID |
| 初始偏移 | `12` 像素 | 第一次抖动的最大幅度 |
| 抖动次数 | 4次 | 左右交替4次后归位 |
| 总时长 | 11帧/30fps ≈ 0.37秒 | 短促有力 |

### 强度调节建议

| 场景 | 初始偏移 | 抖动轮次 | 体感 |
|------|---------|----------|------|
| 轻微提示 | `6px` | 3次 | 含蓄，仅暗示 |
| 标准抖动 | `12px` | 4次 | 明显可感（默认） |
| 强烈警告 | `20px` | 5次 | 激烈震动 |

## 使用示例

将 `TARGET_ID` 替换为实际元素 ID，放置在 `</displayList>` 之后：

```xml
<?xml version="1.0" encoding="utf-8"?>
<component size="600,120">
  <displayList>
    <component id="n9_input" name="inputField" src="input01" fileName="组件/InputField.xml" xy="300,60" size="500,80">
      <relation target="" sidePair="center-center"/>
    </component>
  </displayList>
  <transition name="shake" frameRate="30">
    <item time="0" type="XY" target="n9_input" tween="true" startValue="0,-" endValue="-12,-" duration="2" ease="Sine.Out"/>
    <item time="2" type="XY" target="n9_input" tween="true" startValue="-12,-" endValue="10,-" duration="2" ease="Sine.InOut"/>
    <item time="4" type="XY" target="n9_input" tween="true" startValue="10,-" endValue="-8,-" duration="2" ease="Sine.InOut"/>
    <item time="6" type="XY" target="n9_input" tween="true" startValue="-8,-" endValue="5,-" duration="2" ease="Sine.InOut"/>
    <item time="8" type="XY" target="n9_input" tween="true" startValue="5,-" endValue="0,-" duration="3" ease="Sine.Out"/>
  </transition>
</component>
```

### 代码触发示例（C#）

```csharp
// 密码错误时触发抖动
if (!isPasswordCorrect)
{
    inputComponent.GetTransition("shake").Play();
}
```

## 变体：垂直抖动

将 XY 的偏移方向从 X 轴改为 Y 轴：

```xml
<transition name="shakeV" frameRate="30">
  <item time="0" type="XY" target="TARGET_ID" tween="true" startValue="-,0" endValue="-,-10" duration="2" ease="Sine.Out"/>
  <item time="2" type="XY" target="TARGET_ID" tween="true" startValue="-,-10" endValue="-,8" duration="2" ease="Sine.InOut"/>
  <item time="4" type="XY" target="TARGET_ID" tween="true" startValue="-,8" endValue="-,-6" duration="2" ease="Sine.InOut"/>
  <item time="6" type="XY" target="TARGET_ID" tween="true" startValue="-,-6" endValue="-,3" duration="2" ease="Sine.InOut"/>
  <item time="8" type="XY" target="TARGET_ID" tween="true" startValue="-,3" endValue="-,0" duration="3" ease="Sine.Out"/>
</transition>
```

## 变体：抖动 + 透明度闪烁

抖动同时叠加一次透明度闪烁，强化"错误"提示（如需变色提示，请由业务在元件上切换图标/图源，不要依赖 ColorFilter）：

```xml
<transition name="shakeError" frameRate="30">
  <!-- 抖动 -->
  <item time="0" type="XY" target="TARGET_ID" tween="true" startValue="0,-" endValue="-12,-" duration="2" ease="Sine.Out"/>
  <item time="2" type="XY" target="TARGET_ID" tween="true" startValue="-12,-" endValue="10,-" duration="2" ease="Sine.InOut"/>
  <item time="4" type="XY" target="TARGET_ID" tween="true" startValue="10,-" endValue="-8,-" duration="2" ease="Sine.InOut"/>
  <item time="6" type="XY" target="TARGET_ID" tween="true" startValue="-8,-" endValue="5,-" duration="2" ease="Sine.InOut"/>
  <item time="8" type="XY" target="TARGET_ID" tween="true" startValue="5,-" endValue="0,-" duration="3" ease="Sine.Out"/>
  <!-- 透明度闪烁 -->
  <item time="0" type="Alpha" target="TARGET_ID" tween="true" startValue="1" endValue="0.4" duration="4" ease="Sine.Out"/>
  <item time="4" type="Alpha" target="TARGET_ID" tween="true" startValue="0.4" endValue="1" duration="7" ease="Sine.InOut"/>
</transition>
```
