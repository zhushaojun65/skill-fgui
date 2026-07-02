# 资源包与 package.xml

> 本文件用于处理 FairyGUI 编辑器源工程里的 `package.xml` 资源声明，以及由资源声明派生的显示对象引用。只写普通组件布局时优先看 `elements.md`；涉及新增/修改包资源、九宫格、tile、JTA、字体或音效时加载本文件。

## 源 XML 分层

FairyGUI 源工程里同名标签可能出现在不同层级，语义不同：

| 层级 | 常见文件 | 示例 | 语义 |
|------|----------|------|------|
| 资源声明 | `UIProject/assets/<包名>/package.xml` | `<image id="rpmbd" name="b5.png" path="/images/" scale="9grid" scale9grid="12,7,61,14"/>` | 声明包内资源、资源 ID、导出与九宫格等资源级设置 |
| 显示对象 | `UIProject/assets/<包名>/*.xml` 组件文件 | `<image id="n1" src="rpmbd" xy="0,0" size="100,40"/>` | 在组件显示列表中实例化资源或对象 |

不要把资源声明的属性直接套到显示对象上。最容易混淆的是 `image scale`：

- `package.xml` 里的 `<image scale="9grid|tile">` 是图片资源缩放模式。
- 组件显示对象上的 `scale="sx,sy"` 才是对象变换缩放，SDK 示例中主要见于 `loader`、`component` 等显示对象。

## package.xml 基本结构

```xml
<?xml version="1.0" encoding="utf-8"?>
<packageDescription id="包ID">
  <resources>
    <image id="资源ID" name="图片.png" path="/images/" exported="true"/>
    <movieclip id="动画ID" name="pet.jta" path="/"/>
    <font id="字体ID" name="number.fnt" path="/" exported="true"/>
    <sound id="音效ID" name="click.wav" path="/"/>
  </resources>
  <publish name="包名">
    <atlas name="Default" index="0"/>
  </publish>
</packageDescription>
```

| 标签 | 说明 | SDK 示例出现情况 |
|------|------|------------------|
| `packageDescription` | 包描述根节点，常带 `id`、`jpegQuality`、`compressPNG` | 常见 |
| `resources` | 包内资源声明集合 | 常见 |
| `publish` | 发布包名称；图集声明位于该节点内部 | 常见 |
| `image` | 图片资源声明 | 高频 |
| `movieclip` | JTA/影片剪辑资源声明 | 常见 |
| `font` | 位图字体资源声明 | 常见 |
| `sound` | 音效资源声明 | 少量 |
| `atlas` | 图集声明，位于 `<publish>` 内，由编辑器维护 | 常见 |
| `swf` | SWF 资源声明 | 少量 |

## image 资源声明

```xml
<image id="rpmbd" name="b5.png" path="/images/"
       scale="9grid" scale9grid="12,7,61,14"
       duplicatePadding="true" exported="true"/>
<image id="tileBg" name="repeat.png" path="/images/" scale="tile"/>
<image id="rawBg" name="Paper.jpg" path="/" scale="" duplicatePadding="true"/>
```

| 属性 | 说明 |
|------|------|
| `id` | 包内资源 ID，组件显示对象通过 `src` 引用它 |
| `name` | 源文件名 |
| `path` | 包内目录，SDK 示例使用 `/`、`/images/`、`/Icons/` 等 |
| `exported` | 是否导出给外部按 URL 引用 |
| `scale` | 图片资源缩放模式；SDK 示例确认 `9grid`、`tile` |
| `scale9grid` | 九宫格区域 `"x,y,w,h"`，仅在 `scale="9grid"` 时使用 |
| `duplicatePadding` | 图集边缘复制，常用于避免采样漏边 |

`scale=""` 在 SDK 示例源工程中可见，通常表示编辑器保留的“无特殊缩放模式”。修改已有资源时保留它；新增资源时若不需要九宫格或平铺，优先省略 `scale`，不要把空字符串当作第三种可设计的缩放模式。

写组件显示对象时引用资源 ID：

```xml
<image id="n1" name="bg" src="rpmbd" xy="0,0" size="160,52"/>
```

## movieclip / jta

`package.xml` 中的 JTA 动画资源声明使用 `<movieclip>`：

```xml
<movieclip id="srctk" name="pet.jta" path="/"/>
```

组件显示列表中 SDK 示例有两种写法：

```xml
<movieclip id="n13" name="anim" src="srctk" fileName="pet.jta" xy="781,233"/>
<jta id="n5" name="effect" src="chmf8" xy="-11,33" playing="false"/>
```

`jta` 是编辑器源 XML 中可见的影片剪辑显示对象写法，运行时仍对应 MovieClip 语义。常用属性包括 `src`、`xy`、`size`、`frame`、`playing`、`color`、`pivot`、`alpha`、`rotation`、`visible`、`grayed`、`group`。

## font / sound / atlas / swf

```xml
<font id="p3yav" name="cdtime.fnt" path="/" exported="true"/>
<font id="wa8u2r" name="BMFontTest.fnt" path="/font/" exported="true" texture="jb800"/>
<font id="v040e" name="LiberationSans SDF.ttf" path="/font/" renderMode="sdfaa" samplePointSize="60"/>
<sound id="gkq03" name="BUZZ4.wav" path="/images/"/>
<swf id="swfId" name="asset.swf" path="/"/>
<publish name="Basics">
  <atlas name="Default" index="0"/>
</publish>
```

资源使用规则：

- `font` 资源可被文本 `font="ui://包ID字体ID"` 引用。
- BMFont `.fnt` 可带 `texture="图片资源ID"` 指向字体贴图；TextMeshPro `.ttf` 示例可带 `renderMode="sdfaa"`、`samplePointSize="60"`。
- `sound` 资源可被 Button/Label/ProgressBar/ComboBox 子元素的 `sound` 或 Transition 的 `Sound` item 引用。
- `atlas` 通常由编辑器发布维护，位置在 `<publish>` 内，手写组件 XML 时不要主动改。
- `swf` 仅在源工程已有对应资源时保留；新写 UI 优先使用 SDK 当前支持的 image/movieclip/component 资源。

## 自检规则

- 修改 `package.xml` 时保持资源 `id` 在包内唯一，避免破坏已有 `src` / `ui://` 引用。
- 不要把 `package.xml` 的 `image scale="9grid|tile"` 当作显示对象缩放；显示对象缩放应写 `scale="sx,sy"`。
- 九宫格资源需要同时保留 `scale="9grid"` 与 `scale9grid`；只写 `scale9grid` 语义不完整。
- `movieclip` 资源声明和显示列表里的 `movieclip`/`jta` 实例要区分：前者带 `path`，后者带 `src`、`xy` 等显示对象属性。
- 不要把 `<atlas>` 写进 `<resources>`；当前 SDK 示例工程把它放在 `<publish>` 内。
- 新增或维护字体资源时保留 `.fnt` 的 `texture`、TMP `.ttf` 的 `renderMode` / `samplePointSize`，否则 Unity 端字体解析可能缺少必要元数据。
