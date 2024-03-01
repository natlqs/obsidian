 什么是样式？样式有什么作用？首先，WPF框架提供了许许多多的控件，而这些控件拥有不同的属性，例如一个button控件的长宽、背景颜色、字体字号、内外边距、边框等，我们可以设置这些属性的值，从而让控件呈现出不同的显示效果。如果有多个button，我们该怎么办呢，每个按钮都去设置一遍属性？显示这是不科学的。于是，我们可以将一系列的属性的设置“集中”起来，将它们定义成一个样式，最后将这个样式再设置到控件上，从而达到“一处定义多处引用”的偷懒行为。

样式——就是一种将一组属性值应用到多个元素的便捷方法。

**一、样式的定义**

```cs
public class Style : DispatcherObject, INameScope, IAddChild, ISealable, IHaveResources, IQueryAmbient
{
    public Style();
    public Style(Type targetType);
    public Style(Type targetType, Style basedOn);
 
    public bool IsSealed { get; }
    public Type TargetType { get; set; }
    public Style BasedOn { get; set; }
    public TriggerCollection Triggers { get; }
    public SetterBaseCollection Setters { get; }
    public ResourceDictionary Resources { get; set; }
 
    public override int GetHashCode();
    public void RegisterName(string name, object scopedElement);
    public void Seal();
    public void UnregisterName(string name);
 
}
```

样式的类型叫Style，它继承于DispatcherObject，它最重要的几个属性如下：

TargetType属性：这是一个类类型，也就是一个反射，这个属性指明了当前样式要作用于哪种类型的控件上。因为WPF中有许多的控件，我们定义一个样式时，必须要指明这个样式的“适用范围”。

BasedOn属性：样式也有继承的概念，所以，BasedOn指明了当前样式继承于哪个样式

Triggers属性：这是一个集合属性，表示触发器的定义，当满足某些条件时，触发哪些行为，以使控件达到一定的“节目效果”。比如当鼠标移上去时，控件的背景颜色变成红色。这些的效果就可以通过定义控件的触发器来设置。

Setters属性：这也是一个集合属性，样式是控件属性的值的“提前设置”，所以，我们对控件属性的值的设置都是以Setter条目的形式，一条一条的放到这个Setters集合中。

Resources属性：这个属性叫资源字典。在正式讲解样式之前，我们要先介绍一下资源字典的概念及其用途。它表示一些资源，以字典的形式进行维护，方便程序引用。下一节，我们先介绍ResourceDictionary。

**二、Style样式如何使用**

当我们定义好一个Style之后，如此去使用它？首先，我们在《[FrameworkElement基类](http://www.wpfsoft.com/2023/08/16/756.html)》那一节中也曾了解过Style的概念，并且我们会发现FrameworkElement基类就有一个Style属性。而所有的控件都继承于FrameworkElement基类，所以，我们只需要将定义好的样式赋值到控件的Style属性即可。

2.1 在Application.Resources中定义样式

```cs
<Application x:Class="HelloWorld.App"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:local="clr-namespace:HelloWorld"
             StartupUri="MainWindow.xaml">
    <Application.Resources>
        <Style x:Key="ButtonStyle" TargetType="Button">
            <Setter Property="Width" Value="100"/>
            <Setter Property="Height" Value="30"/>
            <Setter Property="Background" Value="Red"/>
            <Setter Property="Foreground" Value="White"/>
        </Style>
    </Application.Resources>
</Application>
```

2.2 在XAML中引用样式

```cs
<Grid>
    <Button Content="文字块" Style="{StaticResource ButtonStyle}"/>
</Grid>
```

![[assets/WPF从小白到大佬：样式：：WPF样式概述/2ea3a0203fa3bdceb441f52eb2da5808_MD5.jpg]]


注意：在引用样式时，我们有两种方式，分别是DynamicResource和StaticResource，后面再写上样式的名称。DynamicResource表示动态资源，StaticResource表示静态资源。这两者的区别是：静态资源在第一次编译后即确定其对象或值，之后不能对其进行修改。动态资源则是在运行时决定，当运行过程中真正需要时，才到资源目标中查找其值。因此，我们可以动态地修改它。由于动态资源的运行时才能确定其值，因此效率比静态资源要低。

另外，我们在定义样式的条目时，实际上是实例化了一个Setter对象。它有两个关键是属性，分别是Property和Value。Property表示要设置的属性名，Value表示要设置的属性值。有点像键值对的感觉。

```cs
public class Setter : SetterBase, ISupportInitialize
{
    public Setter();
    public Setter(DependencyProperty property, object value);
    public Setter(DependencyProperty property, object value, string targetName);
 
    public DependencyProperty Property { get; set; }
    public object Value { get; set; }
    public string TargetName { get; set; }
 
    public static void ReceiveMarkupExtension(object targetObject, XamlSetMarkupExtensionEventArgs eventArgs);
    public static void ReceiveTypeConverter(object targetObject, XamlSetTypeConverterEventArgs eventArgs);
 
}
```

另外，Setter从定义上看，它还有一个属性叫TargetName，顾名思义，目标名称，也就是这一对属性名和值是哪个控件的属性名和值。通常它在定义触发器时使用，我们将在后面的场景中去学习它。

这一节，我们只是简单的介绍了样式的来龙去脉，待我们了解了资源字典之后，会进一步的学习样式。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：047-《WPF样式概述》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年9月11日

[数据绑定](https://www.wpfsoft.com/binding)







