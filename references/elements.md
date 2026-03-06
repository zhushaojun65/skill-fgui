# 显示元素类型

> 通用属性（id/name/xy/size/pivot/alpha/rotation/visible 等）见 `notes.md`。本文仅列出各元素的**专有属性**。

## 图片 (image)

```xml
<image id="n1" name="bg" src="资源ID"
       xy="x,y" size="宽,高"
       color="#ffffff"
       flip="none|horizontal|vertical|both"
       fillMethod="none|horizontal|vertical|radial90|radial180|radial360"
       fillOrigin="0" fillClockwise="true" fillAmount="1">
  <relation ... />
  <gearDisplay ... />
</image>
```

| 属性 | 说明 | 默认值 |
|------|------|--------|
| `src` | 图片资源ID | 必填 |
| `color` | 颜色叠加 `#RRGGBB` | `"#ffffff"` |
| `flip` | 翻转方式 | `"none"` |
| `fillMethod` | 填充方式 | `"none"` |
| `fillOrigin` | 填充起点 (int，含义随 fillMethod 变化) | `0` |
| `fillClockwise` | 是否顺时针填充 | `true` |
| `fillAmount` | 填充比例 (0-1) | `1` |

### fillOrigin 取值

| fillMethod | fillOrigin 值 |
|------------|---------------|
| `horizontal` | 0=Left, 1=Right |
| `vertical` | 0=Top, 1=Bottom |
| `radial90` | 0=TopLeft, 1=TopRight, 2=BottomLeft, 3=BottomRight |
| `radial180` | 0=Top, 1=Bottom, 2=Left, 3=Right |
| `radial360` | 0=Top, 1=Bottom, 2=Left, 3=Right |

---

## 文本 (text)

```xml
<text id="n2" name="title"
      xy="0,2" size="100,30"
      font="字体名或ui://包ID字体ID" fontSize="18"
      color="#ffffff" align="center" vAlign="middle"
      autoSize="none|both|height|shrink|ellipsis"
      singleLine="true" leading="6" letterSpacing="0"
      underline="false" italic="false" bold="false"
      strokeColor="#000000" strokeSize="1"
      shadowColor="#000000" shadowOffset="1,1"
      ubb="false" vars="true"
      strikethrough="false"
      text="显示文本"/>
```

| 属性 | 说明 | 默认值 |
|------|------|--------|
| `font` | 字体（系统字体名 或 `ui://包ID字体ID`） | 默认字体 |
| `fontSize` | 字号 | 12 |
| `color` | 文字颜色 `#RRGGBB` | `"#000000"` |
| `align` | 水平对齐: `left`, `center`, `right` | `"left"` |
| `vAlign` | 垂直对齐: `top`, `middle`, `bottom` | `"top"` |
| `autoSize` | 自动尺寸 | `"none"` |
| `singleLine` | 单行显示 | `false` |
| `leading` | 行间距 | `3` |
| `letterSpacing` | 字距 | `0` |
| `underline` | 下划线 | `false` |
| `italic` | 斜体 | `false` |
| `bold` | 粗体 | `false` |
| `strikethrough` | 删除线 | `false` |
| `strokeColor` | 描边颜色 `#RRGGBB` | 无 |
| `strokeSize` | 描边粗细 (float) | `1` |
| `shadowColor` | 阴影颜色 `#RRGGBB` | 无 |
| `shadowOffset` | 阴影偏移 `"x,y"` | `"1,1"` |
| `ubb` | 是否支持 UBB 标签 | `false` |
| `vars` | 是否启用模板变量（`{变量名}` 替换） | `false` |
| `text` | 显示文本（支持 `&#xA;` 换行） | 空 |

---

## 富文本 (richtext)

```xml
<richtext id="n2b" name="desc"
          xy="0,0" size="200,100"
          font="字体名" fontSize="16" color="#ffffff"
          align="left" vAlign="top"
          ubb="true"
          text="普通文本[color=#ff0000]红色文本[/color]"/>
```

> 属性与 `text` 一致，但默认启用 UBB 和 HTML 标签解析。

---

## 文本输入 (textinput)

```xml
<textinput id="n2c" name="input"
           xy="0,0" size="200,30"
           font="字体名" fontSize="16" color="#000000"
           align="left" vAlign="middle"
           singleLine="true" maxLength="20"
           password="false" restrict="[0-9]"
           prompt="请输入..."
           keyboardType="0"/>
```

| 属性 | 说明 | 默认值 |
|------|------|--------|
| `prompt` | 占位提示文字 | 无 |
| `password` | 是否密码模式 | `false` |
| `restrict` | 输入限制（正则表达式） | 无 |
| `maxLength` | 最大字符数 | `0`（无限） |
| `keyboardType` | 键盘类型 (int) | `0` |

> 继承 `text` 的所有属性。

---

## 图形 (graph)

```xml
<graph id="n3" name="bg"
       xy="0,0" size="100,100"
       type="rect" lineSize="1"
       lineColor="#ffffff" fillColor="#ff333333"
       corner="10,10,10,10"/>
```

| 属性 | 说明 | 默认值 |
|------|------|--------|
| `type` | 图形类型 | 必填 |
| `lineSize` | 线宽 | `1` |
| `lineColor` | 线条颜色 `#RRGGBB` | `"#000000"` |
| `fillColor` | 填充颜色 `#AARRGGBB` | 无 |
| `corner` | 圆角半径（仅 rect） | 无 |

### 图形类型

| type | 专有属性 |
|------|----------|
| `empty` | 无（仅占位，用于点击区域） |
| `rect` | `corner="r1,r2,r3,r4"` (四角圆角，可简写为单值) |
| `ellipse` | 无 |
| `polygon` | `points="x1,y1,x2,y2,..."` (顶点坐标列表) |
| `regular_polygon` | `sides="6"` (边数), `startAngle="0"`, `distances="0.5,0.8,..."` (顶点距离) |

---

## 加载器 (loader)

```xml
<loader id="n5" name="icon"
        xy="0,0" size="100,100"
        url="ui://包ID资源ID"
        align="center" vAlign="middle"
        fill="none|scale|scaleMatchHeight|scaleMatchWidth|scaleFree|scaleNoBorder"
        shrinkOnly="true" autoSize="false"
        errorSign="true"
        playing="true" frame="0"
        color="#ffffff"
        fillMethod="none" fillOrigin="0"
        fillClockwise="true" fillAmount="1"/>
```

| 属性 | 说明 | 默认值 |
|------|------|--------|
| `url` | 资源地址（`ui://` 格式） | 无 |
| `align` | 水平对齐: `left`, `center`, `right` | `"left"` |
| `vAlign` | 垂直对齐: `top`, `middle`, `bottom` | `"top"` |
| `fill` | 填充缩放模式 | `"none"` |
| `shrinkOnly` | 仅缩小不放大 | `false` |
| `autoSize` | 自动调节大小 | `false` |
| `errorSign` | 加载失败时显示错误标志 | `true` |
| `playing` | 是否播放动画 | `true` |
| `frame` | 动画帧 | `0` |
| `color` | 颜色叠加 `#RRGGBB` | `"#ffffff"` |
| `fillMethod` 系列 | 同 image 的填充属性 | — |

---

## 3D加载器 (loader3D)

```xml
<loader3D id="n5b" name="spine"
          xy="0,0" size="200,200"
          url="ui://包ID资源ID"
          align="center" vAlign="middle"
          fill="none|scale|scaleMatchHeight|scaleMatchWidth|scaleFree|scaleNoBorder"
          animation="idle" skin="default"
          playing="true" loop="true" frame="0"
          color="#ffffff"/>
```

| 属性 | 说明 | 默认值 |
|------|------|--------|
| `animation` | Spine/DragonBones 动画名 | 无 |
| `skin` | 皮肤名 | 无 |
| `loop` | 是否循环播放 | `true` |

> 其余属性与 `loader` 类似。

---

## 影片剪辑 (movieclip)

```xml
<movieclip id="n7" name="anim" src="资源ID"
           xy="0,0" playing="true" frame="0"
           color="#ffffff"
           flip="none|horizontal|vertical|both"/>
```

| 属性 | 说明 | 默认值 |
|------|------|--------|
| `src` | 动画资源ID | 必填 |
| `playing` | 是否播放 | `true` |
| `frame` | 起始帧 | `0` |
| `color` | 颜色叠加 `#RRGGBB` | `"#ffffff"` |
| `flip` | 翻转方式 | `"none"` |

---

## 组件引用 (component)

```xml
<component id="n4" name="btn" src="组件资源ID"
           xy="30,26" size="100,40"
           fileName="components/Button.xml">
  <Button title="按钮文字" icon="ui://包ID资源ID" checked="true"/>
  <!-- 或其他扩展类型的属性设置 -->
</component>
```

> 组件引用作为子元素时，可通过内嵌扩展标签设置属性（见 `components.md`）。

---

## 列表 (list)

```xml
<list id="n6" name="itemList"
      xy="0,0" size="400,500"
      layout="column|row|flow_hz|flow_vt|pagination"
      selectionMode="none|single|multiple|multiple_singleClick"
      overflow="visible|hidden|scroll"
      scroll="horizontal|vertical|both"
      scrollBar="default|visible|auto|hidden"
      scrollBarFlags="0"
      scrollBarMargin="上,右,下,左"
      margin="上,右,下,左"
      clipSoftness="x,y"
      lineGap="6" colGap="6"
      align="left" vAlign="top"
      defaultItem="ui://包ID组件ID"
      autoItemSize="true"
      renderOrder="ascent|descent|arch"
      apexIndex="0">
  <!-- 预设项 -->
  <item url="ui://组件URL" title="标题" icon="图标URL" name="项名称"
        selectedTitle="选中标题" selectedIcon="选中图标"/>
</list>
```

| 属性 | 说明 | 默认值 |
|------|------|--------|
| `layout` | 布局: `column`(单列), `row`(单行), `flow_hz`(水平流), `flow_vt`(垂直流), `pagination`(分页) | `"column"` |
| `selectionMode` | 选择模式 | `"single"` |
| `overflow` | 溢出处理 | `"visible"` |
| `scroll` | 滚动方向（overflow=scroll 时） | `"vertical"` |
| `scrollBar` | 滚动条显示策略 | `"default"` |
| `scrollBarFlags` | 滚动条标志位 (int) | `0` |
| `margin` | 内边距 `"上,右,下,左"` | `"0,0,0,0"` |
| `clipSoftness` | 裁剪软边 `"x,y"` | 无 |
| `lineGap` | 行间距 | `0` |
| `colGap` | 列间距 | `0` |
| `defaultItem` | 默认项组件 URL | 无 |
| `autoItemSize` | 自动调整项大小 | `true` |
| `renderOrder` | 渲染顺序 | `"ascent"` |
| `apexIndex` | arch 模式顶点索引 | `0` |

---

## 组 (group)

```xml
<group id="n9" name="grp"
       xy="0,0" size="300,200"
       layout="none|horizontal|vertical"
       lineGap="0" colGap="0"
       excludeInvisibles="false"
       autoSizeDisabled="false"
       mainGridIndex="-1"/>
```

| 属性 | 说明 | 默认值 |
|------|------|--------|
| `layout` | 布局方式 | `"none"` |
| `lineGap` | 行间距 | `0` |
| `colGap` | 列间距 | `0` |
| `excludeInvisibles` | 排除不可见元素 | `false` |
| `autoSizeDisabled` | 禁用自动尺寸 | `false` |
| `mainGridIndex` | 主网格索引 (-1=无) | `-1` |
