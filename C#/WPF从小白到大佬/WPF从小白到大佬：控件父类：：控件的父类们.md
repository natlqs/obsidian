在C#的世界里，除了object，没有人敢称终极父类，而在WPF的世界里，DispatcherObject坐上了头把交椅。这个类位于程序集:WindowsBase.dll中，根据微软官网资料显示，DispatcherObject继承于object，虽然它在WPF框架算是终极父类，但在整个.NET中来看，仍然只不过是千年老二。

微软在设计WPF框架时，做了一些非常经典且合理的代码架构。我们在开发程序时可能会用到各种各样的控件，这些控件的功能可能各不相同，甚至包括它们的属性、事件和方法，于是微软的工程师们将相同的方法成员或属性成员进行了层层抽象，并写入到一个又一个的父类中，最后让这些控件去继承父类即可。

我们以最常用的Button控件为例。首先看看它的父类们：`Button->ButtonBase->ContentControl->Control->FrameworkElement->UIElement->Visual->DependencyObject->DispatcherObject`。

然后再看一个最常用的StackPanel控件的继承路线：`StackPanel->Panel->FrameworkElement->UIElement->Visual->DependencyObject->DispatcherObject`。

最后再看一个Rectangle矩形图形的继承路线：`Rectangle->Shape->FrameworkElement->UIElement->Visual->DependencyObject->DispatcherObject`。

我们会发现它们的继承路线最终都在FrameworkElement这一层汇合，换句话说，这三种控件的身上都流着FrameworkElement的血，那自然也流淌着`UIElement->Visual->DependencyObject->DispatcherObject`这四个父类的血了。

由此我们可以得出结论，控件的父类们(准确的说，应该叫父类的父类的父类)，至少有如下几个类型：

- DispatcherObject
- DependencyObject
- Visual
- UIElement
- FrameworkElement

WPF几乎所有的控件都从上面这五个父类继承，它们的相互继承关系，形成了一棵树。

![](http://www.wpfsoft.com/wp-content/uploads/2023/08/2023081203432433.png)

那么，这五个父类分别拥有哪些属性、哪些方法、哪些事件？它们为什么而存在呢？下一节，我们将从千年老二DispatcherObject抽象父类说起。

——重庆教主 2023年8月11日