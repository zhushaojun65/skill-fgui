# 扩展组件类型

## Button（按钮）

```xml
<component size="85,25" extention="Button">
  <controller name="button" pages="0,up,1,down,2,over,3,selectedOver" selected="0"/>
  <displayList>
    <!-- up状态背景 -->
    <image name="bg_up" src="btn_up">
      <gearDisplay controller="button" pages="0"/>
    </image>
    <!-- down状态背景 -->
    <image name="bg_down" src="btn_down">
      <gearDisplay controller="button" pages="1,3"/>
    </image>
    <!-- over状态背景 -->
    <image name="bg_over" src="btn_over">
      <gearDisplay controller="button" pages="2"/>
    </image>
    <!-- 标题文字（名称必须为title） -->
    <text name="title" xy="0,2" size="85,22" align="center"/>
    <!-- 图标（名称必须为icon） -->
    <loader name="icon" xy="5,5" size="20,20"/>
  </displayList>
  <Button mode="Common|Check|Radio"
          sound="ui://音效URL" volume="100"
          downEffect="scale" downEffectValue="0.9"/>
</component>
```

### Button 扩展标签属性

| 属性 | 说明 | 默认值 |
|------|------|--------|
| `mode` | 按钮模式 | `"Common"` |
| `sound` | 点击音效 URL | 无 |
| `volume` | 音效音量 (0-100) | `100` |
| `downEffect` | 按下效果；当前 SDK 示例源 XML 确认值为 `scale`，无效果时通常省略 | 省略 |
| `downEffectValue` | 效果参数值 | `0.9` |

> Unity 运行时二进制中 `_downEffect` 还有变暗模式（数值 1）和缩放模式（数值 2）。编写源 XML 时不要直接写 `1`/`2`，优先使用编辑器导出的字符串值。

### 作为子元素引用时 (Setup_AfterAdd)

```xml
<component src="btnRes">
  <Button title="按钮文字" icon="ui://包ID资源ID"
          selectedTitle="选中标题" selectedIcon="ui://选中图标"
          titleColor="#ffffff" titleFontSize="18"
          controller="tab" page="0"
          sound="ui://音效URL"
          checked="true"/>
</component>
```

| 属性 | 说明 |
|------|------|
| `title` | 按钮标题 |
| `selectedTitle` | 选中状态标题 |
| `icon` | 图标 URL |
| `selectedIcon` | 选中状态图标 URL |
| `titleColor` | 标题颜色 `#RRGGBB` |
| `titleFontSize` | 标题字号 |
| `controller` | 关联的控制器名称 |
| `page` | 关联的页面 ID |
| `checked` | 初始选中状态 (bool) |

**按钮模式**:
- `Common`: 普通按钮
- `Check`: 复选框（带 checked 状态）
- `Radio`: 单选按钮（同组互斥）

**约定名称**:
- `title`: 文本/标签元素，显示按钮标题
- `icon`: 加载器，显示按钮图标

---

## ProgressBar（进度条）

```xml
<component size="200,20" extention="ProgressBar">
  <displayList>
    <image name="bg" src="bar_bg"/>
    <!-- 横向进度条（名称必须为bar） -->
    <image name="bar" src="bar_fill" xy="0,0" size="200,20"/>
    <!-- 可选：纵向进度条 -->
    <image name="bar_v" src="bar_fill_v"/>
    <!-- 可选：进度文本 -->
    <text name="title" xy="0,0" size="200,20" align="center"/>
    <!-- 可选：动画进度 -->
    <movieclip name="ani" src="progressAni"/>
  </displayList>
  <ProgressBar titleType="percent|valueAndmax|value|max" reverse="false"/>
</component>
```

### ProgressBar 扩展标签属性

| 属性 | 说明 | 默认值 |
|------|------|--------|
| `titleType` | 标题显示类型: `percent`, `valueAndmax`, `value`, `max` | `"percent"` |
| `reverse` | 是否反向（从右到左/从下到上） | `false` |

### 作为子元素引用时

```xml
<component src="progressRes">
  <ProgressBar value="50" min="0" max="100"
               sound="ui://音效URL" volume="100"/>
</component>
```

| 属性 | 说明 | 默认值 |
|------|------|--------|
| `value` | 当前值 | `0` |
| `min` | 最小值 | `0` |
| `max` | 最大值 | `100` |
| `sound` | 点击音效 URL（SDK 版本字段支持） | 无 |
| `volume` | 音效音量 | `100` |

**约定名称**:
- `bar`: 横向进度条填充区域
- `bar_v`: 纵向进度条填充区域
- `title`: 进度文本（支持 text 或 label）
- `ani`: 进度动画

---

## Slider（滑动条）

```xml
<component size="200,40" extention="Slider">
  <displayList>
    <image name="track" src="slider_bg"/>
    <image name="bar" src="slider_fill"/>
    <component name="grip" src="grip_btn">
      <relation target="bar" sidePair="right-right"/>
    </component>
    <text name="title" xy="0,0"/>
  </displayList>
  <Slider titleType="percent|valueAndmax|value|max"
          reverse="false" wholeNumbers="false" changeOnClick="true"/>
</component>
```

### Slider 扩展标签属性

| 属性 | 说明 | 默认值 |
|------|------|--------|
| `titleType` | 标题显示类型: `percent`, `valueAndmax`, `value`, `max` | `"percent"` |
| `reverse` | 是否反向 | `false` |
| `wholeNumbers` | 是否只允许整数 | `false` |
| `changeOnClick` | 点击轨道是否直接改变值 | `true` |

### 作为子元素引用时

```xml
<component src="sliderRes">
  <Slider value="50" min="0" max="100"/>
</component>
```

| 属性 | 说明 | 默认值 |
|------|------|--------|
| `value` | 当前值 | `0` |
| `min` | 最小值 | `0` |
| `max` | 最大值 | `100` |

**约定名称**:
- `bar`: 已滑动区域
- `grip`: 滑块手柄
- `title`: 数值显示

---

## ComboBox（下拉框）

```xml
<component size="150,30" extention="ComboBox">
  <controller name="button" pages="0,up,1,down,2,over,3,selectedOver" selected="0"/>
  <displayList>
    <image name="bg" src="combo_bg">
      <gearDisplay controller="button" pages="0"/>
    </image>
    <text name="title" xy="5,5" size="120,20"/>
    <image name="arrow" src="dropdown_arrow" xy="130,10"/>
  </displayList>
  <ComboBox dropdown="ui://包ID下拉列表组件ID"
            visibleItemCount="10"
            direction="auto|up|down"/>
</component>
```

> `dropdown` 指向的下拉面板组件内部必须包含名为 `list` 的 `GList`，SDK 会通过 `dropdown.GetChild("list")` 获取它；没有该节点会警告并导致下拉列表无法正常绑定。

### ComboBox 扩展标签属性

| 属性 | 说明 | 默认值 |
|------|------|--------|
| `dropdown` | 下拉面板资源 URL | 必填 |
| `visibleItemCount` | 下拉可见项数 | `10` |
| `direction` | 弹出方向 | `"auto"` |

### 作为子元素引用时

```xml
<component src="comboRes">
  <ComboBox titleColor="#ffffff" selectionController="tabCtrl">
    <item title="选项1" value="opt1" icon="ui://图标URL"/>
    <item title="选项2" value="opt2"/>
    <item title="选项3" value="opt3"/>
  </ComboBox>
</component>
```

| 属性 | 说明 |
|------|------|
| `titleColor` | 标题颜色 |
| `selectionController` | 选中项关联的控制器 |

**约定名称**:
- `title`: 当前选中项文本
- `icon`: 当前选中项图标（可选）
- `list`: 下拉面板中的列表，必须在 `dropdown` 组件内部

---

## Label（标签）

```xml
<component size="200,50" extention="Label">
  <displayList>
    <image name="bg" src="label_bg"/>
    <text name="title" xy="10,5" size="180,20"/>
    <loader name="icon" xy="5,5" size="30,30"/>
  </displayList>
  <Label/>
</component>
```

### 作为子元素引用时

```xml
<component src="labelRes">
  <Label title="标签文字" icon="ui://图标URL"
         titleColor="#000000" titleFontSize="16"
         sound="ui://音效URL" volume="100"/>
</component>
```

| 属性 | 说明 |
|------|------|
| `title` | 标签标题 |
| `icon` | 标签图标 URL |
| `titleColor` | 标题颜色 |
| `titleFontSize` | 标题字号 |
| `sound` | 点击音效 URL（SDK 版本字段支持） |
| `volume` | 音效音量 |

> 如果 `title` 元素是输入框文本（`<text input="true">`），还支持:

| 属性 | 说明 |
|------|------|
| `prompt` | 输入占位提示 |
| `restrict` | 输入限制（正则） |
| `maxLength` | 最大字符数 |
| `keyboardType` | 键盘类型 |
| `password` | 是否密码模式 |

---

## ScrollBar（滚动条）

```xml
<component size="20,200" extention="ScrollBar">
  <displayList>
    <image name="arrow1" src="scrollArrowUp"/>
    <image name="arrow2" src="scrollArrowDown"/>
    <component name="grip" src="scrollGrip"/>
    <graph name="bar" xy="0,20" size="20,160" type="rect" fillColor="#00000000"/>
  </displayList>
  <ScrollBar fixedGripSize="false"/>
</component>
```

| 属性 | 说明 | 默认值 |
|------|------|--------|
| `fixedGripSize` | 固定滑块尺寸，不随内容比例伸缩 | `false` |

**约定名称**:
- `arrow1`: 向上/向左箭头
- `arrow2`: 向下/向右箭头
- `grip`: 滑块手柄
- `bar`: 滑动轨道

---

## Tree（树组件）

```xml
<list id="n_tree" name="tree" size="300,400"
      treeView="true" indent="30" clickToExpand="1">
  <item url="ui://包ID节点组件ID" title="根节点" level="0"/>
  <item url="ui://包ID节点组件ID" title="子节点" level="1"/>
</list>
```

### Tree/List 属性

| 属性 | 说明 | 默认值 |
|------|------|--------|
| `treeView` | 标记该 list 为树形列表 | `false` |
| `indent` | 子节点缩进量 (px) | `30` |
| `clickToExpand` | 点击展开模式: 0=无, 1=单击, 2=双击 | `0` |
| `item.level` | 预设树节点层级 | `0` |

> 当前 SDK 示例工程使用 `<list treeView="true">` 表达树形列表；不要把它写成独立 `<component extention="Tree">`，除非目标工具链明确会把它转换为 `ObjectType.Tree`。

---

# 资源引用规则

## URL 格式

```
ui://[包ID][资源ID]
示例: ui://9leh0eyfrpmb6
```

## 包内引用

```xml
<!-- 同包内直接使用资源ID -->
<image src="rpmb6"/>
<component src="rpmbz"/>
```

## 跨包引用

```xml
<!-- 跨包使用完整URL -->
<Button icon="ui://9leh0eyfrpmbu"/>
<loader url="ui://otherPackageId/resourceId"/>
```

---

# 完整组件示例

## 带动效的按钮

```xml
<?xml version="1.0" encoding="utf-8"?>
<component size="160,52" extention="Button">
  <controller name="button" pages="0,up,1,down,2,over,3,selectedOver" selected="0"/>
  <displayList>
    <image id="n1" name="n1" src="btn_pressed" xy="0,0">
      <gearDisplay controller="button" pages="1,3"/>
      <relation target="" sidePair="width-width,height-height"/>
    </image>
    <image id="n2" name="n2" src="btn_normal" xy="0,0">
      <gearDisplay controller="button" pages="0,2"/>
      <relation target="" sidePair="width-width,height-height"/>
    </image>
    <text id="n3" name="title" xy="0,7" size="160,45"
          fontSize="18" color="#ffffff"
          align="center" vAlign="middle"
          autoSize="none" text="">
      <relation target="" sidePair="width-width,height-height"/>
    </text>
  </displayList>
  <Button sound="ui://9leh0eyfo4lt7w"/>
  <transition name="t0">
    <item time="0" type="Scale" tween="true"
          startValue="1,1" endValue="0.95,0.95"
          duration="3"/>
    <item time="3" type="Scale" tween="true"
          startValue="0.95,0.95" endValue="1,1"
          duration="6" ease="Back.Out"/>
  </transition>
</component>
```

## 带入场动画的列表项

```xml
<?xml version="1.0" encoding="utf-8"?>
<component size="380,125" extention="Button">
  <controller name="IsRead" pages="0,未读,1,已读" selected="0"/>
  <controller name="button" pages="0,up,1,down,2,over,3,selectedOver" selected="0"/>
  <displayList>
    <image id="n10" name="bg" src="item_bg" xy="0,0" size="380,125"/>
    <image id="n13" name="selected" src="item_selected" xy="-2,-3" size="386,131">
      <gearDisplay controller="button" pages="1,3"/>
    </image>
    <image id="n16" name="unread" src="unread_icon" xy="40,42">
      <gearDisplay controller="IsRead" pages="0"/>
    </image>
    <image id="n15" name="read" src="read_icon" xy="40,29">
      <gearDisplay controller="IsRead" pages="1"/>
    </image>
    <text id="n9" name="title" xy="114,28" size="240,28"
          fontSize="22" color="#3333ff" vAlign="middle" autoSize="none"/>
    <text id="n4" name="timeText" xy="113,67" size="239,39"
          fontSize="20" color="#075747" vAlign="middle"/>
  </displayList>
  <Button mode="Radio"/>
  <transition name="t0">
    <item time="0" type="Visible" value="true"/>
    <item time="0" type="XY" tween="true"
          startValue="391,-" endValue="0,0"
          duration="9"/>
  </transition>
</component>
```
