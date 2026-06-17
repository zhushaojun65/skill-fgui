# 动效 (Transition)

## 动效定义结构

```text
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
| `frameRate` | **该动效自己的帧率**（每秒帧数），决定内部 `time`/`duration` 的换算单位 | `24` |
| `options` | 位掩码选项 | `0` |
| `autoPlay` | 是否自动播放 | `false` |
| `autoPlayRepeat` | 自动播放重复次数 | `1` |
| `autoPlayDelay` | 自动播放延迟（秒） | `0` |

> **关于 frameRate**：`frameRate` 是动效自身的播放帧率，**不是项目全局帧率**。transition 内所有 `time` 和 `duration` 数值都按这个帧率换算成秒：例如 `frameRate="30"` 时 `duration="30"` = 1 秒；`frameRate="24"` 时 `duration="24"` = 1 秒。修改 `frameRate` 会等比例改变所有时长。

当前 FairyGUI Unity SDK 示例源 XML 通常省略 `frameRate`，直接写 `time` / `duration` 帧数；模板库为了便于统一动效手感显式写 `frameRate="30"`。从 SDK 示例迁移或回写项目 XML 时，先确认项目编辑器帧率约定，再决定是否保留模板帧率。

### options 位掩码

| 值 | 说明 |
|----|------|
| `1` | 忽略控制器（切换控制器时不停止动效） |
| `2` | 禁用自动停止 |
| `4` | 结束时自动停止 |

## 动画类型一览

| type | 说明 | value/startValue-endValue 格式 |
|------|------|-------------------------------|
| `XY` | 位置 | `"x,y"` 或 `"-,y"` (保持某轴不变)；SDK 还支持百分比位置标记 |
| `Size` | 尺寸 | `"width,height"` |
| `Scale` | 缩放 | `"scaleX,scaleY"` |
| `Rotation` | 旋转 | `"角度"` |
| `Alpha` | 透明度 | `"0.00"` ~ `"1.00"` |
| `Pivot` | 锚点 | `"x,y"` (0-1) — 会运行时改写元件轴心，**通常不建议在动效中使用**，轴心应由元件自身属性配置 |
| `Skew` | 倾斜 | `"skewX,skewY"` |
| `Color` | 颜色 | `"#RRGGBB"` |
| `ColorFilter` | 颜色滤镜 | `"亮度,对比度,饱和度,色相"` (-1~1) — 引擎支持但**视觉效果常不理想，不建议在动效模板中使用** |
| `Visible` | 可见性 | `"true"` 或 `"false"` |
| `Animation` | 动画控制 | `"帧,p/s"` (p=播放, s=停止)；目标是 `GLoader3D` 时还可带 animationName、skinName |
| `Transition` | 嵌套动效 | `"动效名,重复次数"`；重复次数为 `0` 时停止该嵌套动效 |
| `Sound` | 播放音效 | `"ui://包ID资源ID"` 或 `"ui://包ID资源ID,音量"` (音量0-100) |
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
| 自定义 | `Custom`（由编辑器生成自定义曲线数据） |

## item 属性说明

| 属性 | 说明 |
|------|------|
| `time` | 开始帧（按动效自身的 `frameRate` 换算） |
| `target` | 目标元素 ID（省略则为组件自身） |
| `tween` | 是否缓动（`true`/`false`） |
| `duration` | 持续帧数（按动效自身的 `frameRate` 换算） |
| `ease` | 缓动函数 |
| `repeat` | 重复次数（-1=无限） |
| `yoyo` | 往返播放 |
| `label` | 起始标签（代码定位用） |
| `label2` | 结束标签 |

## ⚠️ 关键规则：动效起始帧必须为每条时间轴显式赋初始值

**这是最常踩、且会反复犯的错误，必须严格遵守。**

> **总原则**：一个 `<transition>` 从 time=0 开始播放时，它涉及的**每一条时间轴**（即每一个被动画的属性：Alpha、XY、Scale、Rotation、Size……）在起始帧都应当有一个**明确、可预期**的初始值。**不要默认"元件现在显示的状态就是动效的起点"**。每写一条 `<item>`，都要主动判断"这条时间轴在动效开始的那一帧，值到底是什么"，而不是单纯依赖元件的初始状态。

### 为什么不能依赖元件的初始状态

FairyGUI 动效**不会自动继承或恢复元件的设计静止态**，原因有三：

1. **`startValue` 只在补间真正开始的那一帧才被写入**，它不会"提前接管"延迟期间（`0 → time`）的属性。这段时间属性值 = 元素的**当前实际值**，不一定是设计静止态。
2. **元素"当前实际值"经常不是设计静止态**——它可能被上一次入场动效改过、被控制器/齿轮切换过、或被上一次没播完的动效残留过。
3. 一旦"实际起点"和你"以为的起点"不一致，动效第一帧就会**跳变 / 闪烁**。

最典型的表现：入场时若 `startValue="0"`（Alpha=0）但 `time>0`，延迟期间元素会以 Alpha=1 满不透明**闪现**几十毫秒，再突然跳回 0 才开始淡入。Scale 从 0.3 开始、XY 从屏外开始等所有"从非静止态起始"的情况都会出现"先闪现再跳回起点"的 bug。

### 规则

**凡是被动画的属性，其起始值若可能与"元件当前实际值"不一致，就必须在起始帧用一条 `tween="false"` 的关键帧显式锁定。** 按场景判定：

| 场景 | 起始帧是否需要显式赋值？ | 锁成什么值 |
|------|--------------------------|-----------|
| 补间 `time=0`，元素状态干净（从未被其他动效/控制器改动） | ⚠️ 一般可省略 | 但要确认 `startValue` 与设计静止态一致，否则第一帧仍会跳变 |
| 补间 `time>0`（延迟入场/退场） | ✅ **必须** | 锁成 `startValue`（如 Alpha=0、Scale=0.3、屏外坐标），杜绝延迟期闪现 |
| 退场动效 / 动效重播 / 元素曾被入场动效或控制器改变过 | ✅ **强烈建议** | 即使 `time=0` 也补 `tween="false"` 锁帧，把属性归一到已知起点，避免被残留状态污染 |

### 修复前后对比（延迟补间场景）

❌ **错误**（延迟期间元素会先以静止态闪现）：
```xml
<transition name="__enter__" frameRate="30">
  <item time="0"  type="Alpha" target="n_bg" tween="true" startValue="0" endValue="1" duration="6" ease="Sine.Out"/>
  <!-- 标题延迟 5 帧才淡入，但这 5 帧里 title 会以 Alpha=1 闪现 -->
  <item time="5"  type="Alpha" target="n_title" tween="true" startValue="0" endValue="1" duration="6" ease="Sine.Out"/>
  <item time="5"  type="XY"    target="n_title" tween="true" startValue="100,-50" endValue="100,0" duration="10" ease="Back.Out"/>
</transition>
```

✅ **正确**（在 time=0 锁起始态）：
```xml
<transition name="__enter__" frameRate="30">
  <!-- 起始关键帧锁：把延迟元素的属性在 t=0 就设成 startValue 值，杜绝闪现 -->
  <item time="0" type="Alpha" target="n_title" tween="false" value="0"/>
  <item time="0" type="XY"    target="n_title" tween="false" value="100,-50"/>
  <!-- 然后才是正常补间 -->
  <item time="0"  type="Alpha" target="n_bg"    tween="true" startValue="0" endValue="1" duration="6" ease="Sine.Out"/>
  <item time="5"  type="Alpha" target="n_title" tween="true" startValue="0" endValue="1" duration="6" ease="Sine.Out"/>
  <item time="5"  type="XY"    target="n_title" tween="true" startValue="100,-50" endValue="100,0" duration="10" ease="Back.Out"/>
</transition>
```

### 退场/重播场景：归一化起始态

入场动效把 `n_title` 移动/缩放后，退场动效若直接从 `time=0` 开始，其"起点"其实是入场结束后的残留态——一旦入场被中断或状态被改动，退场第一帧就会跳变。稳妥写法是先归一化：

```xml
<transition name="__exit__" frameRate="30">
  <!-- 归一化起始态：把可能被入场动效改过的属性，在 t=0 锁成退场确知起点 -->
  <item time="0" type="Alpha" target="n_title" tween="false" value="1"/>
  <item time="0" type="Scale" target="n_title" tween="false" value="1,1"/>
  <item time="0" type="XY"    target="n_title" tween="false" value="100,0"/>
  <!-- 退场补间（time>0 的 XY 仍须先在 t=0 锁值，规则同上） -->
  <item time="0" type="Alpha" target="n_title" tween="true" startValue="1" endValue="0" duration="6" ease="Sine.In"/>
  <item time="2" type="XY"    target="n_title" tween="true" startValue="100,0" endValue="100,-50" duration="8" ease="Sine.In"/>
</transition>
```

### 写入清单（每写完一个 transition 逐条自检）

对照下表检查**每一条补间 item**：

- [ ] 这条补间的 `startValue`，与元件在 time=0 的实际状态一致吗？不一致是否会跳变？
- [ ] 这条补间是 `time>0` 吗？是 → 必须在 `time=0` 为该属性补 `tween="false"` 锁帧。
- [ ] 元素是否被其他动效 / 控制器改过状态？是 → 即使 `time=0` 也建议先锁帧归一化。

> **核心心法**：`startValue` 只负责"补间开始的那一瞬间"，它**不负责**"动效开始前的整段时间"，**也不会帮你清理**元件被其他动效改过的状态。所以每条时间轴的起点都要靠 `tween="false"` 关键帧**强制显式赋值**，而不能依赖元件的初始状态。

## 路径动画 (path)

```xml
<item type="XY" target="n2" tween="true"
      startValue="166,577.5" endValue="220,205.5"
      duration="120" ease="Linear" repeat="-1"
      path="路径数据..."/>
```

> `path` 为贝塞尔曲线控制点数据，由编辑器生成。

## 位置百分比

`XY` 动效在 SDK 中支持百分比位置标记，通常由编辑器生成。手写 XML 时优先使用普通像素坐标；只有在明确需要相对父级尺寸变化的位置动画时，才保留编辑器导出的百分比写法。

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
  <item time="0" type="Scale" tween="true"
        startValue="1,1" endValue="0.9,0.9"
        duration="3"/>
  <item time="3" type="Scale" tween="true"
        startValue="0.9,0.9" endValue="1,1"
        duration="6" ease="Back.Out"/>
</transition>
```

> 提示：`Scale`/`Rotation` 的视觉基点由元件自身的 `pivot`/`anchor` 属性决定，**应在元件上配置**，不要用动效去改。

### 自动播放的循环呼吸效果

```xml
<transition name="呼吸" autoPlay="true" autoPlayRepeat="-1">
  <item time="0" type="Scale" tween="true"
        startValue="0.8,0.8" endValue="1.1,1.1"
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
