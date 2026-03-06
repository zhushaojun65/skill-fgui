# FairyGUI 组件开发指南 (fgui)

> Claude Code Skill - 用于创建和修改 FairyGUI 组件 XML 文件的开发指南

## 简介

本 Skill 提供了 FairyGUI 组件 XML 的完整开发参考，帮助开发者快速创建 UI 组件、动效、控制器等。

## 功能覆盖

| 模块 | 说明 |
|------|------|
| 动效 (Transition) | 入场/退场动画、缓动效果、音效播放 |
| 控制器 (Controller) | 状态管理、页面切换 |
| 齿轮 (Gear) | 元素属性与控制器绑定 |
| 显示元素 | image/text/loader/graph/list/group 等 |
| 扩展组件 | Button/ProgressBar/Slider/ComboBox/Label 等 |
| 关联关系 | 元素自适应布局 |

## 文件结构

```
fgui/
├── SKILL.md                    # Skill 入口文件
├── references/
│   ├── notes.md               # 通用规则、GObject 属性
│   ├── transition.md          # 动效定义
│   ├── gear.md                # 控制器、齿轮机制
│   ├── elements.md            # 显示元素类型
│   ├── components.md          # 扩展组件类型
│   └── relation.md            # 关联关系
└── README.md                  # 本文件
```

## 使用方式

在 Claude Code 中，当需要处理以下任务时会自动加载此 Skill：

- 创建/修改 FairyGUI 组件 XML 文件
- 添加动画效果 (Transition)
- 设置状态切换 (Controller/Gear)
- 添加显示元素 (image/text/loader 等)
- 创建扩展组件 (Button/ProgressBar 等)

### 触发关键词

- 动画、动效、transition、缓动、ease
- 控制器、状态切换、gear、页面切换
- 图片、文本、列表、loader、image、text
- Button、进度条、滑块、下拉框

## 示例

### 带动效的按钮

```xml
<?xml version="1.0" encoding="utf-8"?>
<component size="160,52" extention="Button">
  <controller name="button" pages="0,up,1,down,2,over,3,selectedOver" selected="0"/>
  <displayList>
    <image id="n1" src="btn_pressed">
      <gearDisplay controller="button" pages="1,3"/>
    </image>
    <image id="n2" src="btn_normal">
      <gearDisplay controller="button" pages="0,2"/>
    </image>
    <text id="n3" name="title" xy="0,7" size="160,45"
          fontSize="18" color="#ffffff" align="center"/>
  </displayList>
  <Button sound="ui://9leh0eyfo4lt7w"/>
  <transition name="t0">
    <item time="0" type="Pivot" value="0.5,0.5"/>
    <item time="0" type="Scale" tween="true"
          startValue="1,1" endValue="0.95,0.95" duration="3"/>
    <item time="3" type="Scale" tween="true"
          startValue="0.95,0.95" endValue="1,1" duration="6" ease="Back.Out"/>
  </transition>
</component>
```

## 参考资料

- [FairyGUI 官方文档](https://www.fairygui.com/docs/)
- [FairyGUI 编辑器下载](https://www.fairygui.com/download)

## 版本

- Init Version - 基础组件开发指南
