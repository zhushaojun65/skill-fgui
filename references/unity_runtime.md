# Unity 运行时接入

> 本文件只在用户询问 Unity SDK 运行时代码、C# 接入、窗口、虚拟列表、扩展类、手势、TypingEffect、TMP/Spine/DragonBones/LuaSupport 时加载。只写 XML 时不要加载本文件。

## 源 XML 与运行时的边界

FairyGUI 编辑器源 XML 最终会被发布为包数据，Unity 运行时通过 `ByteBuffer` 读取。写 XML 时以 `UIProject/assets/**/*.xml` 的源 XML 为准；写 C# 接入时以 `Assets/Scripts` 与 `Assets/Examples` 的运行时代码为准。不要把运行时 enum 数值直接倒写成源 XML 字符串。

## 常见入口

```csharp
UIPackage.AddPackage("UI/Basics");
GComponent view = UIPackage.CreateObject("Basics", "Main").asCom;
GRoot.inst.AddChild(view);
```

| API | 用途 |
|-----|------|
| `UIPackage.AddPackage(...)` | 加载 FairyGUI 包；示例工程同时有路径包和 AssetBundle 包用法 |
| `UIPackage.CreateObject(...)` | 创建包内对象 |
| `GRoot.inst.AddChild(...)` | 把 UI 加到根节点 |
| `GComponent.GetChild(...)` | 获取子对象 |
| `GComponent.GetController(...)` | 获取控制器 |
| `GComponent.GetTransition(...)` | 获取挂在该组件上的 transition |

## 扩展类注册

当包内组件需要绑定自定义 C# 类型时，先注册扩展类，再创建对象：

```csharp
UIPackage.AddPackage("UI/VirtualList");
UIObjectFactory.SetPackageItemExtension("ui://VirtualList/mailItem", typeof(MailItem));
```

注册 URL 必须指向包内组件资源。组件类通常继承 `GComponent` 或对应扩展类型，并在 `ConstructFromXML` / 构造后流程中缓存子节点、控制器和 transition。

## Transition 触发

Transition 挂在拥有它的 `GComponent` 上，`target` 只是被动画驱动的子对象。代码触发时从承载 transition 的组件调用：

```csharp
ownerComponent.GetTransition("t0").Play();
ownerComponent.GetTransition("t0").SetHook("label_name", OnHook);
```

Window 动画通常放在 `DoShowAnimation()` / `DoHideAnimation()`：

```csharp
override protected void DoShowAnimation()
{
    contentPane.GetTransition("t0").Play();
}
```

## 列表、虚拟列表和树

```csharp
list.itemRenderer = RenderListItem;
list.SetVirtual();
list.numItems = data.Count;
```

| 场景 | 运行时写法 |
|------|------------|
| 普通虚拟列表 | `GList.SetVirtual()` + `itemRenderer` |
| 循环列表 | `GList.SetVirtualAndLoop()` + `itemRenderer` |
| 树组件 | 源 XML 通常是 `<list treeView="true">`；运行时通过 `asTree` / `GTree` 使用 |
| 下拉框 | `ComboBox` 的 `dropdown` 组件内部必须有名为 `list` 的列表 |

## UIPanel 与 UIPainter

`UIPanel` 用于把 FairyGUI 作为 Unity 场景组件挂载；`UIPainter` 用于渲染到纹理或特殊场景。示例工程常见写法：

```csharp
GComponent view = GetComponent<UIPanel>().ui;
```

如果只是写源 XML，不需要引入 `UIPanel` / `UIPainter` 细节；只有处理 Unity 场景挂载、World Space、RenderTexture、3D UI 时再查本节。

## 弹窗、菜单和 Window

| API | 用途 |
|-----|------|
| `GRoot.inst.ShowPopup(...)` | 显示弹出层 |
| `GRoot.inst.TogglePopup(...)` | 切换弹出层 |
| `Window` | 标准窗口基类，适合封装显示、隐藏、模态等待和窗口动效 |
| `PopupMenu` | 菜单类弹出结构 |

## 手势与文本效果

示例工程使用以下运行时能力：

| 能力 | 类 |
|------|----|
| 滑动 | `SwipeGesture` |
| 长按 | `LongPressGesture` |
| 缩放 | `PinchGesture` |
| 旋转 | `RotationGesture` |
| 打字机效果 | `TypingEffect` |
| 拖拽 | `DragDropManager` |

## 可选扩展与宏

| 能力 | 宏 / 条件 |
|------|-----------|
| TextMeshPro 字体 | `FAIRYGUI_TMPRO` |
| Spine | `FAIRYGUI_SPINE` |
| DragonBones | `FAIRYGUI_DRAGONBONES` |
| Lua 支持 | `LuaSupport` 下的 ToLua / xLua 绑定 |

若用户只是要求 XML 属性，不要主动展开这些运行时扩展；只有用户涉及 Unity 工程接入、宏启用或相关报错时再加载。
