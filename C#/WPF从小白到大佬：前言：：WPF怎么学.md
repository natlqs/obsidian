# WPF怎么学
许多小伙伴如果没有接触过Web开发，特别是对React、Vue等前端框架没有开发经验，习惯了WinForm的拖拽布局方式，在首次接触WPF时，一时间会感觉有些扑朔迷离、力不从心。笔者以前也是从WinForm转过来的，对于WPF的数据绑定、触发器、模板、样式、命令、依赖属性、路由事件等概念和实际应用，也曾走过不少弯路。鉴于此，我希望借助自己搭建的WPF中文网平台，将过去踩过的坑、形成的经验分享出来，以资后来人。

WPF作为一款桌面程序的UI框架，其实在计算机软件领域中，算是很小很小的一个知识分支，通常学校是不会排课的，所以对于大部分程序员而言，WPF框架都是自学的。那么要怎么样学呢？

**学习前提**

首先，如果将来以C#+WPF开发桌面应用程序，那么C#的基础知识和高级知识是必须掌握的。通常我们对于C#初学者而言，可能刚好掌握了C#的基础知识，例如C#的常量、变量、控制流程、函数、类型，也了解C#的抽象、继承、多态、封装、委托、事件、回调函数等，熟悉WinForm控件的常规使用，这些知识在学习和开发简单的WinForm应用程序可能足够写出一些Demo，但是，一旦开始学习WPF，不但前端XAML要手动编写，而且在XAML的写法中，又要理解绑定、触发器、模板、样式、命令这些类型的作用，以及它们在前后端的实例化和业务上的相互关系，属实有些蒙圈。所以接下来就学习前提简要分析一下。

```c#
public int Width
    {
        get { return (int)GetValue(WidthProperty); }
        set { SetValue(WidthProperty, value); }
    }
 
// Using a DependencyProperty as the backing store for Width.  This enables animation, styling, binding, etc...
public static readonly DependencyProperty WidthProperty =
    DependencyProperty.Register("Width", typeof(int), typeof(ownerclass), new PropertyMetadata(0));
```

就拿上面的代码举例，我们定义了一个WPF的依赖属性WidProperty，然后通过属性包装器以普通属性Width对外暴露。真正定义依赖属性的关键词是DependencyProperty，以及它的静态方法Register,从Register所需要参数来看，第二个参数和第三个参数要求传入两个反射对象实例，可见WPF在这里使用了C#反射技术。

另外，再看一个微软官方提供的关于路由事件的自定义代码。

```C#
// Register a custom routed event using the Bubble routing strategy.
public static readonly RoutedEvent TapEvent = EventManager.RegisterRoutedEvent(
    name: "Tap",
    routingStrategy: RoutingStrategy.Bubble,
    handlerType: typeof(RoutedEventHandler),
    ownerType: typeof(CustomButton));
 
// Provide CLR accessors for adding and removing an event handler.
public event RoutedEventHandler Tap
{
    add { AddHandler(TapEvent, value); }
    remove { RemoveHandler(TapEvent, value); }
}
```

观察上面的代码，不难看出，RouteEvent代表一个路由事件，并使用EventManager.RegisterRoutedEvent方法成员向WPF事件系统注册了一个叫Tap的事件，这里不单使用了反射，还使用了C#的事件（event）关键词。而要掌握event关键词的概念及用法，委托知识也是必须要理解透彻的。所以要想学会WPF框架，下面这些关于C#语言的编程知识是必须掌握的。

- 熟悉OOP面向对象编程思维，掌握抽象、继承、多态、封装的概念和应用；
- 掌握C#的基本语法、数据类型、程序结构、类型定义、方法成员、常量、变量、数组等基础知识；
- 掌握C#的特性（Attribute）、 反射（Reflection）、 属性（Property）、 索引器（Indexer）；
- 掌握C#的委托（Delegate）、 事件（Event）、 集合（Collection）、 泛型（Generic）；
- 了解XML或HTML语义结构；

**学习路线**

要论学习路线，无外呼两种，一是先学整体知识框架，再逐一学习各项细节知识；二是反过来，先从细节入手，一点一滴的啃，最终学完所有细节，从而形成对整体知识框架的认知。在这里我推荐第一种方法，所以我将WPF的学习路线安排如下：

![[assets/WPF从小白到大佬：前言：：WPF怎么学/75c91c92bd35c0f7e05f2b800e632be9_MD5.jpg]]

**最后建议**

山不让尘乃成其高，水不辞盈方有其阔。学习WPF并坚持学习WPF，很快您将从WPF小白蜕变成WPF大佬，加油！

——重庆教主 2023年8月8日