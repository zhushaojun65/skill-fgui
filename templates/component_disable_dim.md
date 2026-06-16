# 单组件置灰禁用 (Component Disable Dim)

## 效果描述

组件进入禁用态时，轻微缩小并降低透明度，形成“暂不可用”的稳定状态。动效不做颜色滤镜变化（ColorFilter 效果不稳定），也不做完全透明消失，确保组件依然可见。

**适用场景**：按钮禁用、技能冷却、资源不足、奖励暂不可领、条件未满足

## XML 代码

```xml
<transition name="componentDisableDim" frameRate="30">
  <item time="0" type="Scale" target="TARGET_ID" tween="true" startValue="1,1" endValue="0.96,0.96" duration="6" ease="Sine.Out"/>
  <item time="0" type="Alpha" target="TARGET_ID" tween="true" startValue="1" endValue="0.6" duration="6" ease="Sine.Out"/>
</transition>
```

> 此模板只负责视觉过渡，不负责 FairyGUI 的 `enabled` / `touchable` 状态。交互禁用仍应由业务代码或控制器处理。如需真正的“置灰”视觉，建议由业务切换元件的 grayed 属性或替换图源，而非依赖 ColorFilter。

## 参数说明

| 参数 | 当前值 | 说明 |
|------|--------|------|
| `frameRate` | `30` | 动画帧率 |
| `name` | `componentDisableDim` | 自定义动效名称 |
| `TARGET_ID` | 需替换 | 目标组件或显示对象 ID |
| 缩放结束值 | `0.96,0.96` | 轻微收缩，不改变布局 |
| 透明度结束值 | `0.6` | 半透明提示不可用，保持可见 |
| 总时长 | 6帧/30fps = 0.20秒 | 快速进入禁用感 |

### 强度调节建议

| 场景 | 缩放值 | 透明度值 | 体感 |
|------|--------|----------|------|
| 轻量禁用 | `0.98` | `0.8` | 轻微变弱 |
| 标准禁用 | `0.96` | `0.6` | 明确不可用（默认） |
| 强禁用 | `0.92` | `0.4` | 明显压低 |

## 使用示例

```xml
<?xml version="1.0" encoding="utf-8"?>
<component size="220,90">
  <displayList>
    <component id="n4_btn" name="claimButton" src="btnClaim" fileName="组件/ClaimButton.xml" xy="110,45" size="200,72"/>
  </displayList>
  <transition name="componentDisableDim" frameRate="30">
    <item time="0" type="Scale" target="n4_btn" tween="true" startValue="1,1" endValue="0.96,0.96" duration="6" ease="Sine.Out"/>
    <item time="0" type="Alpha" target="n4_btn" tween="true" startValue="1" endValue="0.6" duration="6" ease="Sine.Out"/>
  </transition>
</component>
```
