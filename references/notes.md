# 注意事项

## 适用范围

本知识库面向 FairyGUI 编辑器导出的**源 XML**（如 `UIProject/assets/**/*.xml`）。Unity SDK 运行时会把包数据读成二进制 `ByteBuffer`，部分字段在运行时是数字枚举，但源 XML 中是字符串；生成或修改 XML 时必须优先采用源 XML 写法。

1.  **ID规则**: `id` 属性在包内必须唯一，通常由编辑器自动生成
2.  **名称约定**: 扩展组件有特定的名称约定（如 `title`, `bar`, `grip`）
3.  **坐标系**: 左上角为原点，向右向下为正
4.  **帧率**: 动效内部的 `time`/`duration` 按动效自身的 `frameRate` 换算；`frameRate="24"` 时 24 帧 = 1 秒，`frameRate="30"` 时 30 帧 = 1 秒
5.  **颜色格式**: 
    *   普通颜色: `#RRGGBB`
    *   带透明度: `#AARRGGBB`
6.  **值分隔符**: 
    *   坐标用逗号: `"100,200"`
    *   多值用竖线: `"值1|值2|值3"`
7.  **保留值**: `-` 表示保持原值不变（如 `"-,100"` 表示X不变，Y=100）

## GObject 通用属性

以下属性适用于所有显示元素（image/text/graph/loader/component 等）：

| 属性 | 说明 | 默认值 |
|------|------|--------|
| `id` | 元素唯一ID | 必填 |
| `name` | 元素名称（代码引用用） | 可选 |
| `src` | 资源ID（包内引用） | 与类型相关 |
| `pkg` | 跨包引用时的包ID | 可选 |
| `fileName` | 原始文件名（仅编辑器用） | 可选 |
| `xy` | 位置坐标 `"x,y"` | `"0,0"` |
| `size` | 尺寸 `"宽,高"` | 原始尺寸 |
| `aspect` | 保持资源宽高比（源 XML 常见于 image/component） | `false` |
| `restrictSize` | 尺寸限制 `"minW,maxW,minH,maxH"` | 无限制 |
| `pivot` | 轴心 (0-1) `"px,py"` | `"0,0"` |
| `anchor` | 是否启用锚点 | `false` |
| `scale` | 缩放 `"sx,sy"` | `"1,1"` |
| `skew` | 倾斜 `"sx,sy"` | `"0,0"` |
| `alpha` | 透明度 (0-1) | `1` |
| `rotation` | 旋转角度 | `0` |
| `visible` | 是否可见 | `true` |
| `touchable` | 是否可交互 | `true` |
| `grayed` | 是否灰化 | `false` |
| `blend` | 混合模式 | `"none"` |
| `filter` | 滤镜类型（如 `"color"`） | 无 |
| `filterData` | 滤镜数据 `"亮度,对比度,饱和度,色相"` | 无 |
| `customData` | 自定义数据（传递给代码） | 无 |
| `tooltips` | 悬停提示文字 | 无 |
| `group` | 所属组ID | 无 |
| `locked` | 编辑器锁定标记，源 XML 可出现，通常不作为运行时逻辑依赖 | `false` |
| `sortingOrder` | 自定义显示深度，非 0 时会影响同级显示排序 | `0` |

## 混合模式 (blend)

| 值 | 说明 |
|----|------|
| `none` | 无混合（默认） |
| `add` | 加法混合 |
| `multiply` | 乘法混合 |
| `screen` | 滤色混合 |
| `erase` | 擦除 |
| `mask` | 遮罩 |
| `below` | 置于下方 |
| `off` | 关闭 |
| `normal` | 普通混合（SDK enum 支持） |
| `one_oneMinusSrcAlpha` | 预乘透明相关混合（SDK enum 支持） |
| `custom1`/`custom2`/`custom3` | 自定义混合模式（SDK enum 支持） |

## 资源引用规则

```
ui://[包ID][资源ID]      — 完整 URL（跨包引用）
ui://包名/资源名           — 通过名称引用
直接使用资源ID            — 同包内引用（如 src="rpmb6"）
```
