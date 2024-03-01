触发器是指当满足预设的条件时去执行一些事务的工具，比如我们希望鼠标移到某个按钮上方时，这个按钮的颜色、大小发生一些改变。这个时候，条件是鼠标移到按钮上，执行的事务是改变按钮的颜色和大小。

WPF提供了5种触发器，以满足不同场合下的使用要求。触发器主要运用的场景在Style、ControlTemplate、DataTemplate三个地方。我们先介绍Style样式中的用法，待学习模板知识时，再进一步学习触发器在模板中的使用。

|   |   |
|---|---|
|触发器名称|说明|
|Trigger|监测依赖属性的变化、触发器生效|
|MultiTrigger|通过多个条件的设置、达到满足条件、触发器生效|
|DataTrigger|通过数据的变化、触发器生效|
|MultiDataTrigger|多个数据条件的触发器|
|EventTrigger|事件触发器, 触发了某类事件时, 触发器生效。|

一、**Trigger触发器的定义**

```cs
public class Trigger : TriggerBase, IAddChild, ISupportInitialize
{
    public Trigger();
 
    public DependencyProperty Property { get; set; }
    public object Value { get; set; }
    public string SourceName { get; set; }
    public SetterBaseCollection Setters { get; }
 
    public static void ReceiveTypeConverter(object targetObject, XamlSetTypeConverterEventArgs eventArgs);
 
}
```

从定义上看，它有4个属性，我们一一分析

Property属性：表示定义触发器所指向的属性名称，需要注意的是，它的类似是DependencyProperty（依赖属性），也就是说，触发器所生效的属性必须是WPF中的依赖属性。

Value属性：表示定义触发器所指向的属性的值，这两个属性要连起来理解，即某个属性的值等于预设的值时，此时将该触发器将被触发，至于要执行哪些具体的事务，就要看Setters集合中定义了哪些项。

SourceName属性：获取或设置与导致关联的 setter 要应用的属性对象的名称。

Setters属性：这是一个集合，类似于Style样式的Setters，且用途是一致的，就是指当前触发器被触发时，将执行Setters里面的内容项。

二、**Trigger**示例

前端代码

```cs
<Window x:Class="HelloWorld.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:HelloWorld" 
        xmlns:forms="clr-namespace:System.Windows.Forms;assembly=System.Windows.Forms"
        mc:Ignorable="d" FontSize="14"
        Title="WPF中文网之控件课程 - www.wpfsoft.com" Height="350" Width="500">
    <Window.Resources>
        <Style x:Key="GreenButtonStyle" TargetType="Button">
            <Setter Property="Width" Value="100"/>
            <Setter Property="Height" Value="30"/>
            <Setter Property="Background" Value="Green"/>
            <Setter Property="Foreground" Value="White"/>
            <Setter Property="Margin" Value="3"/>
            <Style.Triggers>
                <Trigger Property="IsMouseOver" Value="True">
                    <Setter Property="Foreground" Value="Red"/>
                    <Setter Property="Width" Value="150"/>
                    <Setter Property="Height" Value="50"/>
                    <Setter Property="Content" Value="鼠标移入"/>
                </Trigger>                
            </Style.Triggers>
        </Style>
    </Window.Resources>
    <StackPanel VerticalAlignment="Center">
        <Button Content="绿色按钮" Style="{StaticResource GreenButtonStyle}"/>
        <CheckBox Content="CheckBox">
            <CheckBox.Style>
                <Style TargetType="CheckBox">
                    <Setter Property="Width" Value="100"/>
                    <Setter Property="Height" Value="30"/>
                    <Setter Property="Background" Value="Orange"/>
                    <Setter Property="Foreground" Value="Green"/>
                    <Setter Property="Margin" Value="3"/>
                    <Style.Triggers>
                        <Trigger Property="IsChecked"  Value="True">
                            <Setter Property="Foreground" Value="Red"/>
                        </Trigger>
                        <Trigger Property="IsChecked"  Value="False">
                            <Setter Property="Foreground" Value="Orange"/>
                        </Trigger>
                        <Trigger Property="IsMouseOver" Value="True">
                            <Setter Property="FontWeight" Value="Bold"/>
                        </Trigger>                       
                        <Trigger Property="IsMouseOver" Value="False">
                            <Setter Property="FontWeight" Value="Normal"/>
                        </Trigger>
                    </Style.Triggers>
                </Style>
            </CheckBox.Style>
        </CheckBox> 
    </StackPanel>
</Window>
```

![[assets/WPF从小白到大佬：样式：：Trigger触发器/6a72363327c6195d62fbcfbe43b53329_MD5.jpg]]

在按钮的样式中，我们定义了一个触发器，条件是当按钮的IsMouseOver属性等于True时，执行的事务是将按钮的前景色改成红色，同时改变按钮的尺寸。

在CheckBox复选项的样式中，我们定义的触发器条件是当CheckBox的IsChecked属性等于True时，执行的事务是将复选框的前景色改成红色。

需要注意的是：**Trigger触发器的条件应该是当前控件拥有的属性名称**。比如Button按钮没有IsChecked属性，就不可设置与CheckBox相同的触发器。

大多数控件都有IsMouseOver属性，包括CheckBox,所以，我们在鼠标移到CheckBox上方时，将其字体变成了粗体。另外，完整的触发器写法应如CheckBox一样，如果是bool型的属性，Value的值就有两个，分别是True和False，所以，条件也就有了两个，如果只写了True的情况，WPF也会默认一个False条件的触发器。

下一节，我将介绍多条件的触发器。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：050-《Trigger触发器》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年9月11日