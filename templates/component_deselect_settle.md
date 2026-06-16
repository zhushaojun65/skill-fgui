# 单组件取消选中回落 (Component Deselect Settle)

## 效果描述

单个组件从选中态恢复普通态时，先轻微缩回，再平滑回到原始尺寸。通常与 `componentSelectPop` 配对使用，避免取消选中时突然跳回。

**适用场景**：Tab 取消选中、卡片失焦、列表项取消高亮、道具格恢复普通态

## XML 代码

```xml
<transition name="componentDeselectSettle" frameRate="30">
  <item time="0" type="Scale" target="TARGET_ID" tween="true" startValue="1.06,1.06" endValue="0.98,0.98" duration="4" ease="Sine.Out"/>
  <item time="4" type="Scale" target="TARGET_ID" tween="true" startValue="0.98,0.98" endValue="1,1" duration="6" ease="Sine.InOut"/>
</transition>
```

> 此动效不设 `autoPlay`，应在组件离开选中态时触发。若选中态稳定尺寸不是 `1.06`，同步调整第一段 `startValue`。

## 参数说明

| 参数 | 当前值 | 说明 |
|------|--------|------|
| `frameRate` | `30` | 动画帧率 |
| `name` | `componentDeselectSettle` | 自定义动效名称 |
| `TARGET_ID` | 需替换 | 目标组件或显示对象 ID |
| 起始尺寸 | `1.06,1.06` | 与选中态稳定尺寸保持一致 |
| 回落尺寸 | `0.98,0.98` | 轻微缩回，制造收束感 |
| 结束尺寸 | `1,1` | 普通态原始尺寸 |

## 使用示例

```xml
<?xml version="1.0" encoding="utf-8"?>
<component size="160,80">
  <displayList>
    <component id="n1_tab" name="tabItem" src="tab01" fileName="组件/TabItem.xml" xy="80,40" size="140,64"/>
  </displayList>
  <transition name="componentDeselectSettle" frameRate="30">
    <item time="0" type="Scale" target="n1_tab" tween="true" startValue="1.06,1.06" endValue="0.98,0.98" duration="4" ease="Sine.Out"/>
    <item time="4" type="Scale" target="n1_tab" tween="true" startValue="0.98,0.98" endValue="1,1" duration="6" ease="Sine.InOut"/>
  </transition>
</component>
```

### 配对建议

与 `templates/component_select_pop.md` 配对：选中播放 `componentSelectPop`，取消选中播放 `componentDeselectSettle`。
