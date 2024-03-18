首先我们介绍一下WPF的依赖属性系统，它是指WPF提供的一组服务，专门用来扩展WPF的属性功能，而受到这些服务支持的属性就称为依赖属性。

WPF的依赖属性系统对于开发者而言，几乎是感知不到的，它通过DependencyProperty类型的一些静态方法成员，提供一系列注册依赖属性或附加属性的功能，让我们可以向依赖属性系统注册属于我们自己写的依赖属性。

为了对比CLR普通属性与WPF的依赖属性的区别，直观的认知两者的概念，我们先来看看普通属性的定义

一、普通属性的定义

```cs
private int length = 0;public int Length{    get { return length; }    set { length = value; }}
```

CLR普通属性的本质是在内部定义了一个私有字段，然后通过属性包装器将内部私有定段公开出来，get表示读出私有字段的值，set表示写入值到私有字段。假如WPF控件上的某个属性就是这类的普通属性，那么我们要更新这个属性的值，就只能赋值，用不能采用WPF的绑定方式了，因为只有依赖属性才支持绑定。

二、依赖属性的定义

在C#后端的类型中，输入：propdp，然后按下tab键，VS会自动帮我们输入以下代码：

```cs
public int MyProperty
{
    get { return (int)GetValue(MyPropertyProperty); }
    set { SetValue(MyPropertyProperty, value); }
}
 
// Using a DependencyProperty as the backing store for MyProperty.
// This enables animation, styling, binding, etc...
public static readonly DependencyProperty MyPropertyProperty =
    DependencyProperty.Register("MyProperty", typeof(int), typeof(ownerclass), new PropertyMetadata(0));
```

我们来一一分析一下上述代码。

首先是MyPropertyProperty成员，它被声明为DependencyProperty 类型，且用DependencyProperty的Register方法注册，而在注册的时候，传入了4个参数。

第一个参数“MyProperty”：这个MyProperty其实是一个类似普通属性包装器的名字。经过依赖属性系统注册后，将来MyProperty就代表了MyPropertyProperty依赖属性。

第二个参数typeof(int)：表示这个MyPropertyProperty的数据类型，也就是我们在使用MyProperty时的数据类型，这里被声明成int型。注意这里要求传入数据类型的反射。

第三个参数typeof(ownerclass)：表示当前这个MyPropertyProperty依赖属性是属于哪个类的，一般填写当前这个类型。

第四个参数new PropertyMetadata(0)：表示传入一个PropertyMetadata属性元数据。这个PropertyMetadata定义了MyPropertyProperty依赖属性的默认值和回调函数。回调函数就是当属性值发生改变时要执行的逻辑代码。

其次是MyProperty成员，它由CLR属性包装器实现get和set，并且使用了GetValue 和 SetValue成员去读出和写入MyPropertyProperty依赖属性。

咦？哪儿来的GetValue和SetValue？

在讲解WPF的基类时，我们曾经分享过[DependencyObject](http://www.wpfsoft.com/2023/08/13/571.html)类。这个类就定义了GetValue和SetValue，分别表示获取某个依赖属性的值和写入一个值到某个依赖属性。结论，我们要在某个类中自定义一个依赖属性，那么这个类一定要继承DependencyObject基类。

在了解依赖属性的概念和定义之后，我们就可以正式地去定义并使用它。下一节，我们将以一个实例来说明WPF的依赖属性用法。

——重庆教主 2023年10月25日