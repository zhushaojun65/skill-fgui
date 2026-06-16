# 控制器 (Controller)

## 定义语法

```xml
<controller name="控制器名" pages="页索引0,页名0,页索引1,页名1,..." selected="默认页索引"/>
```

### 属性

| 属性 | 说明 | 默认值 |
|------|------|--------|
| `name` | 控制器名称 | 必填 |
| `pages` | 页面列表 `"索引,名称,索引,名称,..."` | 必填 |
| `selected` | 默认选中页索引 | `"0"` |
| `autoRadioGroupDepth` | 自动管理 Radio 组深度 | `false` |
| `homePageCapture` | 记忆首页状态 | `false` |

### 控制器动作

```xml
<controller name="c1" pages="0,off,1,on" selected="0">
  <action type="PlayTransition" fromPage="0" toPage="1"
          transitionName="showAni" playTimes="1" delay="0" stopOnExit="true"/>
  <action type="ChangePage" toPage="0"
          objectId="n5" controllerName="state" targetPage="0"/>
</controller>
```

| action 属性 | 说明 |
|-------------|------|
| `fromPage` | 触发来源页条件，省略表示不限 |
| `toPage` | 触发目标页条件，省略表示不限 |
| `type="PlayTransition"` | 播放本组件上的 transition |
| `transitionName` | 要播放的 transition 名称 |
| `playTimes` | 播放次数；`-1` 通常表示循环 |
| `delay` | 延迟秒数 |
| `stopOnExit` | 离开匹配页时停止该 transition |
| `type="ChangePage"` | 修改指定对象上的控制器页 |
| `objectId` | 目标对象 ID |
| `controllerName` | 目标对象上的控制器名 |
| `targetPage` | 目标页；`~1` 表示来源控制器当前页，`~2` 表示来源控制器上一页 |

## 示例

```xml
<!-- 按钮状态控制器（标准4态） -->
<controller name="button" pages="0,up,1,down,2,over,3,selectedOver" selected="0"/>

<!-- 自定义控制器 -->
<controller name="IsRead" pages="0,未读,1,已读" selected="0"/>
<controller name="tab" pages="0,,1,,2," selected="0"/>
```

## 按钮关联控制器

在组件引用中关联控制器：

```xml
<component src="radioBtn">
  <Button controller="tab" page="0" checked="true" title="Tab1"/>
</component>
```

---

# 齿轮 (Gear) 机制

齿轮用于将元素属性与控制器页面绑定，实现状态切换效果。

## 齿轮类型一览

| 齿轮标签 | 索引 | 控制属性 | values 格式 |
|----------|------|----------|-------------|
| `gearDisplay` | 0 | 显示/隐藏 | — (使用 pages) |
| `gearXY` | 1 | 位置 | `"x,y\|x,y"` |
| `gearSize` | 2 | 尺寸+缩放 | `"w,h,scaleX,scaleY\|..."` |
| `gearLook` | 3 | 外观 | `"alpha,rotation,grayed,touchable\|..."` |
| `gearColor` | 4 | 颜色 | `"#颜色,#描边颜色\|..."` |
| `gearAni` | 5 | 动画 | `"帧,状态(p/s)\|..."` |
| `gearText` | 6 | 文本 | `"文本1\|文本2"` |
| `gearIcon` | 7 | 图标 | `"url1\|url2"` |
| `gearDisplay2` | 8 | 多控制器显示 | — (使用 pages + condition) |
| `gearFontSize` | 9 | 字号 | `"字号1\|字号2"` |

> `\|` 分隔不同页面的值，`-` 或空值表示该页面不设置。

## 齿轮通用属性

| 属性 | 说明 | 适用齿轮 |
|------|------|----------|
| `controller` | 关联的控制器名称 | 所有 |
| `pages` | 生效的页面列表 `"0,2,3"` | 所有 |
| `values` | 各页面对应值（`\|` 分隔） | gearXY/Size/Look/Color/Ani/Text/Icon/FontSize |
| `default` | 未指定页面的默认值 | gearXY/Size/Look/Color/Ani/Text/Icon/FontSize |
| `tween` | 是否缓动切换 | gearXY/Size/Look |
| `ease` | 缓动函数（SDK 也支持 `Custom` 曲线） | gearXY/Size/Look |
| `duration` | 缓动时长（秒） | gearXY/Size/Look |

## gearDisplay - 显示控制

```xml
<image src="icon">
  <!-- 仅在控制器c1的页面0和2时显示 -->
  <gearDisplay controller="c1" pages="0,2"/>
</image>
```

## gearDisplay2 - 多控制器显示控制

```xml
<image src="icon">
  <!-- 同时满足多个控制器条件时显示 -->
  <gearDisplay2 controller="c1" pages="0,1" condition="0"/>
  <!-- condition: 0=全部满足(AND), 1=任一满足(OR) -->
</image>
```

## gearXY - 位置控制

```xml
<graph>
  <!-- 页面0时位置(181,271)，页面1时位置(60,328)，带缓动 -->
  <gearXY controller="c1" pages="0,1" values="181,271|60,328" tween="true"/>
</graph>
```

## gearSize - 尺寸控制

```xml
<graph>
  <!-- 页面1时尺寸196x174，默认100x100，带缓动 -->
  <gearSize controller="c1" pages="1" values="196,174,1,1" default="100,100,1,1" tween="true"/>
</graph>
```

## gearLook - 外观控制

```xml
<image>
  <!-- 格式: alpha,rotation,grayed[,touchable]；第4项可省略 -->
  <gearLook controller="button" pages="0,2"
            values="1.00,0,0,0|0.00,0,0,0"
            default="1.00,0,0,0"
            tween="true" ease="Quad.Out" duration="0.5"/>
</image>
```

## gearColor - 颜色控制

```xml
<text>
  <!-- 格式: 主颜色,描边颜色 -->
  <gearColor controller="c1" pages="1" values="#000000,#000000" default="#ffffff,#000000"/>
</text>
```

## gearAni - 动画控制

```xml
<movieclip>
  <!-- 格式: 帧,播放状态(p=play/s=stop)；loader3D 还可附带 animationName、skinName -->
  <gearAni controller="c1" pages="1" values="0,s" default="0,p"/>
</movieclip>
```

## gearText / gearIcon

```xml
<text>
  <gearText controller="lang" pages="0,1" values="Hello|你好" default="Hello"/>
</text>

<loader>
  <gearIcon controller="state" pages="0,1" values="ui://pkg/iconA|ui://pkg/iconB"/>
</loader>
```
