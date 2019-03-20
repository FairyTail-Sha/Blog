---
title: Windows 窗体中的事件顺序
date: 2019-03-14 21:47:08
tags: winforms C#
---

# 应用程序启动和关闭事件

Form 和 Control 类公开一组与应用程序启动和关闭相关的事件。 Windows 窗体应用程序启动时，主窗体的启动事件将按照以下顺序引发：

```cs
Control.HandleCreated 在为控件创建句柄时发生。

Control.BindingContextChanged 当 BindingContext 属性的值更改时发生。

Form.Load 在第一次显示窗体前发生。

Control.VisibleChanged 在 Visible 属性值更改时发生。

Form.Activated  当 使用代码激活或用户激活窗体时发生。

Form.Shown  只要窗体是首次显示就发生。
```

应用程序关闭时，主窗体的关闭事件将按照以下顺序引发：

```cs
Form.Closing 在关闭窗体时发生。

Form.FormClosing 关闭窗体前发生。

Form.Closed 关闭窗体后发生。

Form.FormClosed 关闭窗体后发生。

Form.Deactivate 当窗体失去焦点并不再是活动窗体时发生。
```

在主窗体关闭事件后，将引发 Application 类的 ApplicationExit 事件。

# 焦点和验证事件

当通过使用键盘（TAB、SHIFT+TAB 等），通过调用 Select 或 SelectNextControl 方法，或通过将 ActiveControl 属性设置为当前窗体来更改焦点时，Control 类的焦点事件将按以下顺序发生：

```cs
Enter 进入控件时发生。

GotFocus 在控件接收焦点时发生。

Leave 在输入焦点离开控件时发生。

Validating 在控件验证时发生。 如验证邮箱

Validated 在控件完成验证时发生。

LostFocus 在控件失去焦点时发生。
```

当通过使用鼠标或调用 Focus 方法更改焦点时，Control 类的焦点事件将按以下顺序发生：

```cs
Enter

GotFocus

LostFocus

Leave

Validating

Validated
```