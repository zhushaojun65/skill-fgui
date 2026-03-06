# 动效 (Transition)

## 动效定义结构

```xml
<transition name="动效名称" [frameRate="24"] [options="0"]
            [autoPlay="true" autoPlayRepeat="1" autoPlayDelay="0"]>
  <item time="开始帧" type="动画类型" [target="目标元素ID"]
        [tween="true"] [startValue="起始值"] [endValue="结束值"]
        [duration="持续帧数"] [ease="缓动函数"]
        [repeat="重复次数"] [yoyo="true"]
        [label="标签"] [label2="结束标签"]
        [value="直接设置值"]/>
</transition>
```

## transition 属性

| 属性 | 说明 | 默认值 |
|------|------|--------|
| `name` | 动效名称 | 必填 |
| `frameRate` | 帧率（每秒帧数） | `24` |
| `options` | 位掩码选项 | `0` |
| `autoPlay` | 是否自动播放 | `false` |
| `autoPlayRepeat` | 自动播放重复次数 | `1` |
| `autoPlayDelay` | 自动播放延迟（秒） | `0` |

### options 位掩码

| 值 | 说明 |
|----|------|
| `1` | 忽略控制器（切换控制器时不停止动效） |
| `2` | 禁用自动停止 |
| `4` | 结束时自动停止 |

## 动画类型一览

| type | 说明 | value/startValue-endValue 格式 |
|------|------|-------------------------------|
| `XY` | 位置 | `"x,y"` 或 `"-,y"` (保持某轴不变) |
| `Size` | 尺寸 | `"width,height"` |
| `Scale` | 缩放 | `"scaleX,scaleY"` |
| `Rotation` | 旋转 | `"角度"` |
| `Alpha` | 透明度 | `"0.00"` ~ `"1.00"` |
| `Pivot` | 锚点 | `"x,y"` (0-1) |
| `Skew` | 倾斜 | `"skewX,skewY"` |
| `Color` | 颜色 | `"#RRGGBB"` |
| `ColorFilter` | 颜色滤镜 | `"亮度,对比度,饱和度,色相"` (-1~1) |
| `Visible` | 可见性 | `"true"` 或 `"false"` |
| `Animation` | 动画控制 | `"帧,p/s"` (p=播放, s=停止) |
| `Transition` | 嵌套动效 | `"动效名,重复次数"` |
| `Sound` | 播放音效 | `"ui://包ID资源ID,音量"` (音量0-100) |
| `Shake` | 震动 | `"振幅,持续秒数"` |
| `Text` | 改变文本 | `"文本内容"` |
| `Icon` | 改变图标 | `"ui://包ID资源ID"` |

## 缓动函数 (ease)

| 类别 | 缓动函数 |
|------|----------|
| 线性 | `Linear` |
| 二次 | `Quad.In`, `Quad.Out`, `Quad.InOut` |
| 三次 | `Cubic.In`, `Cubic.Out`, `Cubic.InOut` |
| 四次 | `Quart.In`, `Quart.Out`, `Quart.InOut` |
| 五次 | `Quint.In`, `Quint.Out`, `Quint.InOut` |
| 正弦 | `Sine.In`, `Sine.Out`, `Sine.InOut` |
| 指数 | `Expo.In`, `Expo.Out`, `Expo.InOut` |
| 圆形 | `Circ.In`, `Circ.Out`, `Circ.InOut` |
| 弹性 | `Elastic.In`, `Elastic.Out`, `Elastic.InOut` |
| 回弹 | `Back.In`, `Back.Out`, `Back.InOut` |
| 弹跳 | `Bounce.In`, `Bounce.Out`, `Bounce.InOut` |

## item 属性说明

| 属性 | 说明 |
|------|------|
| `time` | 开始帧（基于 24fps） |
| `target` | 目标元素 ID（省略则为组件自身） |
| `tween` | 是否缓动（`true`/`false`） |
| `duration` | 持续帧数 |
| `ease` | 缓动函数 |
| `repeat` | 重复次数（-1=无限） |
| `yoyo` | 往返播放 |
| `label` | 起始标签（代码定位用） |
| `label2` | 结束标签 |

## 路径动画 (path)

```xml
<item type="XY" target="n2" tween="true"
      startValue="166,577.5" endValue="220,205.5"
      duration="120" ease="Linear" repeat="-1"
      path="路径数据..."/>
```

> `path` 为贝塞尔曲线控制点数据，由编辑器生成。

## 动效示例

### 入场动画（从左滑入）

```xml
<transition name="入场">
  <item time="0" type="XY" tween="true"
        startValue="-400,0" endValue="0,0"
        duration="18" ease="Expo.Out"/>
</transition>
```

### 按钮点击效果（缩放+回弹）

```xml
<transition name="点击">
  <item time="0" type="Pivot" value="0.5,0.5"/>
  <item time="0" type="Scale" tween="true"
        startValue="1,1" endValue="0.9,0.9"
        duration="3"/>
  <item time="3" type="Scale" tween="true"
        startValue="0.9,0.9" endValue="1,1"
        duration="6" ease="Back.Out"/>
</transition>
```

### 自动播放的循环呼吸效果

```xml
<transition name="呼吸" autoPlay="true" autoPlayRepeat="-1">
  <item time="0" type="Scale" tween="true"
        startValue="0.8,0.8" endValue="1.1,1.1"
        duration="30" repeat="-1" yoyo="true"/>
  <item time="0" type="ColorFilter" tween="true"
        startValue="0.3,0,0,0" endValue="0,0,0,0"
        duration="30" repeat="-1" yoyo="true"/>
</transition>
```

### 复合入场动效

```xml
<transition name="t0">
  <!-- 播放音效 -->
  <item time="0" type="Sound" value="ui://zgmoraj4gkq03,100"/>

  <!-- 左侧元素从左入场 -->
  <item time="0" type="XY" target="n4" tween="true"
        startValue="-275,-" endValue="279,-"
        duration="12" ease="Expo.Out"/>

  <!-- 右侧元素从右入场 -->
  <item time="0" type="XY" target="n5" tween="true"
        startValue="1183,-" endValue="533,-"
        duration="12" ease="Expo.Out"/>

  <!-- 嵌套调用另一个动效 -->
  <item time="12" type="Transition" value="shakeAni,1"/>

  <!-- 改变文本 -->
  <item time="12" type="Text" target="n3" value="BOSS 出现!"/>
</transition>
```
