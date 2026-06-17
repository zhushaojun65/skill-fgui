# 关联关系 (Relation)

用于定义元素之间的自适应关系。当目标元素的位置或大小改变时，源元素自动跟随调整。

```xml
<relation target="目标元素ID或空" sidePair="关系对1,关系对2,..."/>
```

> `target=""` 表示关联到父组件。

## 关系对列表

### 基本关系

| 关系对 | 说明 |
|--------|------|
| `left-left` | 左边跟随目标左边 |
| `left-center` | 左边跟随目标中心 |
| `left-right` | 左边跟随目标右边 |
| `center-center` | 水平居中对齐 |
| `right-left` | 右边跟随目标左边 |
| `right-center` | 右边跟随目标中心 |
| `right-right` | 右边跟随目标右边 |
| `top-top` | 顶部跟随目标顶部 |
| `top-middle` | 顶部跟随目标中间 |
| `top-bottom` | 顶部跟随目标底部 |
| `middle-middle` | 垂直居中对齐 |
| `bottom-top` | 底部跟随目标顶部 |
| `bottom-middle` | 底部跟随目标中间 |
| `bottom-bottom` | 底部跟随目标底部 |
| `width-width` | 宽度跟随 |
| `height-height` | 高度跟随 |
| `width` | 宽度跟随的快捷写法（源 XML 中可见） |
| `height` | 高度跟随的快捷写法（源 XML 中可见） |

### 扩展关系（保持间距）

| 关系对 | 说明 |
|--------|------|
| `leftext-left` | 左边延伸跟随目标左边（改变自身宽度） |
| `leftext-right` | 左边延伸跟随目标右边 |
| `rightext-left` | 右边延伸跟随目标左边 |
| `rightext-right` | 右边延伸跟随目标右边 |
| `topext-top` | 顶部延伸跟随目标顶部（改变自身高度） |
| `topext-bottom` | 顶部延伸跟随目标底部 |
| `bottomext-top` | 底部延伸跟随目标顶部 |
| `bottomext-bottom` | 底部延伸跟随目标底部 |

### 整体关系

| 关系对 | 说明 |
|--------|------|
| `size-size` | 尺寸跟随（宽+高同时跟随） |
| `width,height` | 宽高同时跟随的快捷组合（源 XML 中常见于按钮/窗口组件） |

### 百分比关系

在关系对后加 `%` 表示按百分比跟随：

```xml
<relation target="" sidePair="width-width%,height-height%"/>
```

## 示例

```text
<!-- 跟随父组件宽高 -->
<image>
  <relation target="" sidePair="width-width,height-height"/>
</image>

<!-- 跟随父组件宽高的快捷写法，SDK 示例 XML 中常见 -->
<image>
  <relation target="" sidePair="width,height"/>
</image>

<!-- 相对于元素n1右对齐 -->
<component>
  <relation target="n1" sidePair="right-right"/>
</component>

<!-- 始终保持在父组件右下角 -->
<group>
  <relation target="" sidePair="right-right,bottom-bottom"/>
</group>

<!-- 左边延伸跟随父组件宽度（自身宽度随父组件变化） -->
<image>
  <relation target="" sidePair="leftext-left,rightext-right"/>
</image>

<!-- 按百分比跟随父组件尺寸 -->
<graph>
  <relation target="" sidePair="width-width%,height-height%"/>
</graph>
```
