# 组件结构

## 适用范围

当任务涉及 component 根节点、组件尺寸、扩展类型、滚动容器、遮罩、软裁剪、点击测试或组件级背景色时读取本文件。显示元素通用属性仍以 references/notes.md 为准，Button/ProgressBar 等扩展组件细节见 references/components.md。

## 组件基本结构

    <?xml version="1.0" encoding="utf-8"?>
    <component size="宽度,高度" [extention="扩展类型"]
               [overflow="visible|hidden|scroll"] [opaque="true"]
               [margin="上,下,左,右"] [clipSoftness="x,y"]
               [mask="遮罩元素ID"] [reversedMask="false"]
               [hitTest="resourceId,x,y"]>
      <controller ... />       <!-- 控制器定义 -->
      <displayList>            <!-- 显示列表 -->
        ...                    <!-- 显示元素 -->
      </displayList>
      <transition ... />       <!-- 动效定义 -->
      <ExtensionTag/>          <!-- 扩展类型标记 -->
    </component>

完整按钮与列表项示例见 references/components.md 的“完整组件示例”；动效写法见 references/transition.md 和对应模板。

## 组件属性

| 属性 | 说明 | 默认值 |
|------|------|--------|
| size | 组件尺寸，格式为 "宽,高" | 必填 |
| extention | 扩展类型 | 无 |
| overflow | 溢出处理: visible, hidden, scroll | "visible" |
| opaque | 是否不透明，点击空白区域是否穿透 | true |
| margin | 内边距 "上,下,左,右"，与 SDK 二进制读取顺序一致 | 无 |
| clipSoftness | 裁剪软边 "x,y" | 无 |
| mask | 遮罩元素 ID | 无 |
| reversedMask | 是否反向遮罩 | false |
| hitTest | 像素级点击测试 "resourceId,x,y" | 无 |
| bgColor / bgColorEnabled | 组件背景色与是否启用背景色 | 无 / false |
| initName | 初始化名称，源 XML 可出现 | 无 |
| idnum | 编辑器生成 ID 计数，保留即可，手写通常不需要 | 无 |

## 滚动属性（overflow="scroll" 时）

| 属性 | 说明 | 默认值 |
|------|------|--------|
| scroll | 滚动方向: horizontal, vertical, both | "vertical" |
| scrollBar | 滚动条策略: default, visible, auto, hidden | "default" |
| scrollBarFlags | 滚动条标志位 | 0 |
| scrollBarMargin | 滚动条边距 "上,下,左,右"，与 SDK 二进制读取顺序一致 | 无 |
| scrollBarRes | 滚动条资源 "垂直资源ID,水平资源ID" | 无 |
| ptrRes | 下拉刷新资源 "headerResId,footerResId" | 无 |
| pageController | 滚动页同步到控制器 | 无 |
| scrollItemToViewOnClick | 点击子项时滚动到可见区域 | false |
| foldInvisibleItems | 折叠不可见子项占位 | false |

常用 scrollBarFlags 位含义：1=垂直滚动条显示在左侧，2=snapToItem，4=滚动条按需显示，8=pageMode，16/32=强制开启/关闭 touchEffect，64/128=强制开启/关闭 bounceback，256=禁用惯性，512=禁用遮罩裁剪，1024=滚动条浮动，2048=不裁剪 margin。
