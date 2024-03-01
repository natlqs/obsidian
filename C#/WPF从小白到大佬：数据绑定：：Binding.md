Binding类架起了控件和ViewModel之间的桥梁，它就像一个媒婆，指示了哪个控件的哪个属性与哪个ViewModel类的哪个属性建立绑定关系。提供对绑定定义的高级访问，该绑定连接绑定目标对象（通常为 WPF 元素）的属性和任何数据源（例如数据库、XML 文件，或包含数据的任何对象）。

**一、Binding类的定义**

```cs
public class Binding : BindingBase
{
    public const string IndexerName = "Item[]";
    public static readonly RoutedEvent SourceUpdatedEvent;
    public static readonly RoutedEvent TargetUpdatedEvent;
    public static readonly DependencyProperty XmlNamespaceManagerProperty;
    public static readonly object DoNothing;
 
    public Binding();
    public Binding(string path);
 
    public UpdateSourceTrigger UpdateSourceTrigger { get; set; }
    public bool NotifyOnSourceUpdated { get; set; }
    public bool NotifyOnTargetUpdated { get; set; }
    public bool NotifyOnValidationError { get; set; }
    public IValueConverter Converter { get; set; }
    public object ConverterParameter { get; set; }
    public CultureInfo ConverterCulture { get; set; }
    public object Source { get; set; }
    public RelativeSource RelativeSource { get; set; }
    public string ElementName { get; set; }
    public bool IsAsync { get; set; }
    public object AsyncState { get; set; }
    public BindingMode Mode { get; set; }
    public string XPath { get; set; }
    public bool ValidatesOnDataErrors { get; set; }
    public bool ValidatesOnNotifyDataErrors { get; set; }
    public bool BindsDirectlyToSource { get; set; }
    public bool ValidatesOnExceptions { get; set; }
    public Collection<ValidationRule> ValidationRules { get; }
    public PropertyPath Path { get; set; }
    public UpdateSourceExceptionFilterCallback UpdateSourceExceptionFilter { get; set; }
 
    public static void AddSourceUpdatedHandler(DependencyObject element, EventHandler<DataTransferEventArgs> handler);
    public static void AddTargetUpdatedHandler(DependencyObject element, EventHandler<DataTransferEventArgs> handler);
    public static XmlNamespaceManager GetXmlNamespaceManager(DependencyObject target);
    public static void RemoveSourceUpdatedHandler(DependencyObject element, EventHandler<DataTransferEventArgs> handler);
    public static void RemoveTargetUpdatedHandler(DependencyObject element, EventHandler<DataTransferEventArgs> handler);
    public static void SetXmlNamespaceManager(DependencyObject target, XmlNamespaceManager value);
    public bool ShouldSerializePath();
    public bool ShouldSerializeSource();
    public bool ShouldSerializeValidationRules();
 
}
```

接下来，我们重点分析一下Binding类提供了哪些重要的属性，这些属性在实际使用过程中，代表什么意思。

**二、Binding的数据源**

首先，控件的属性与Model的属性建立绑定，我们将Model及其属性称为数据源，而数据源大致可以分为以下4种方式进行绑定。

**第一种数据源**，也就是ViewModel中的Model。在写法上直接如下所示：

```cs
Text="{Binding Person.Name}"
```

这里实例化了一个Binding对象，后面紧跟的Person.Name表示一个Path路径，指的是当前的DataContext中那个ViewModel对象的Person.Name，注意看，Binding的有一个带参数的构造函数：public Binding(string path);实际上，就是将Person.Name路径传给了path形参。

**第二种数据源**，指明某个具体的数据源对象及对象的属性。这种绑定方式要用了Binding类的Source属性和Path属性。通常写法如下：

```cs
Text="{Binding Source={StaticResource RedBrush},Path=Color}"
```

在这里，Source属性表示数据源对象，它是一个静态资源对象，Path=Color表示要绑定这个静态资源对象的Color属性。我们已经提前在资源里定义好了这个资源对象。资源对象的实例名叫RedBrush，它确实也有一个叫Color的属性。

```cs
<Window.Resources>
    <SolidColorBrush x:Key="RedBrush" Color="Red"/>
</Window.Resources>
```

这样绑定的效果如下图所示。

![[assets/WPF从小白到大佬：数据绑定：：Binding/22dc0afe00ec5ce28a0ea6a3ea331254_MD5.jpg]]

看起来Color=Red,实则是一个代表颜色的16进制字符串。

**第三种数据源**，利用ElementName属性指明另一个控件作为数据源，这里仍然要用到Path来指定另一个控件的某个属性路径。

```cs
<StackPanel x:Name="panel" VerticalAlignment="Center" Margin="100,0">
    <TextBlock Margin="5">
        <Run Text="Source示例:"/>
        <Run Text="{Binding Source={StaticResource RedBrush},Path=Color}"/>
    </TextBlock>
    <TextBlock Margin="5">
        <Run Text="ElementName示例:"/>
        <Run Text="{Binding ElementName=panel,Path=Margin}"/>
    </TextBlock>
</StackPanel>
```

![[assets/WPF从小白到大佬：数据绑定：：Binding/de6e0f37cb61f1906c7448c59e1a1122_MD5.jpg]]

通过Binding类的ElementName去指定当前XAML中的另一个StackPanel控件，并绑定其Margin属性，这样，TextBlock就显示了StackPanel控件的Margin属性值。

**第四种数据源**，利用RelativeSource属性绑定一个相对的数据源。这种绑定方式也经常使用，且实用价值很高，作为开发者，一定要掌握它的用法。

```cs
<StackPanel x:Name="panel" VerticalAlignment="Center" Margin="80,0">
    <TextBlock Margin="5">
        <Run Text="Source示例:"/>
        <Run Text="{Binding Source={StaticResource RedBrush},Path=Color}"/>
    </TextBlock>
    <TextBlock Margin="5">
        <Run Text="ElementName示例:"/>
        <Run Text="{Binding ElementName=panel,Path=Margin}"/>
    </TextBlock>
    <TextBlock Margin="5">
        <Run Text="RelativeSource示例:"/>
        <Run Text="{Binding RelativeSource={RelativeSource Mode=Self},Path=Foreground}"/>
        <Run Text="{Binding RelativeSource={RelativeSource Mode=FindAncestor,AncestorType=StackPanel},Path=Margin}"/>
    </TextBlock>    
</StackPanel>
```

![[assets/WPF从小白到大佬：数据绑定：：Binding/c01cf29e46a6b7b1a5fe54e5d76ac8f4_MD5.jpg]]

RelativeSource类有3个重要的属性，它们分别是Mode、AncestorType和AncestorLevel。

Mode：表示寻找相对数据源的模式，一共有4种模式

|   |   |
|---|---|
|模式|说明|
|PreviousData|允许在当前显示的数据项列表中绑定上一个数据项（不是包含数据项的控件）。|
|TemplatedParent|引用应用了模板的元素，其中此模板中存在数据绑定元素。|
|Self|引用正在其上设置绑定的元素，并允许你将该元素的一个属性绑定到同一元素的其他属性上。|
|FindAncestor|引用数据绑定元素的父链中的上级。 这可用于绑定到特定类型的上级或其子类。|

在上面的例子中，我们演示了Self（自身控件）和FindAncestor（找上级控件）两种模式。

AncestorType：当Mode=FindAncestor时，这时就要指示要找的这个上级是什么类型，AncestorType用来表示上级的类型。

AncestorLevel：获取或设置要查找的上级级别。 使用 1 指示最靠近绑定目标元素的项。

```cs
Text="{Binding RelativeSource={RelativeSource Mode=Self},Path=Foreground}"
```

表示将自己的前景色显示到Text属性上。

```
Text="{Binding RelativeSource={RelativeSource Mode=FindAncestor,AncestorType=StackPanel},Path=Margin}"
```

表示从当前控件开始找上级，一个类型为StackPanel的控件，并把这个StackPanel控件的Margin显示到当前控件的Text属性上。

**三、Binding的绑定模式**

当一个实体的属性绑定到控件的属性之后，还需要指明这两者之间的绑定关系。这个就是Binding类的Mode属性，Mode属性是一个枚举类型。它有如下几个情况。

|   |   |
|---|---|
|枚举值|说明|
|TwoWay|双向绑定，即导致更改源属性或目标属性时自动更新另一方。|
|OneWay|单向绑定，在更改绑定源（源）时更新绑定目标（目标）。|
|OneTime|一次绑定，在应用程序启动或数据上下文更改时，更新绑定目标。|
|OneWayToSource|在目标属性更改时，更新源属性。|
|Default|默认绑定，文本框的默认绑定是双向的，而其他大多数属性默认为单向绑定。|

这里面常用的是OneWay和TwoWay。如果是TwoWay（双向绑定），意味着前端控件的属性改变时，后端的Model也跟着改变，反之亦然。就拿前端改变去影响后端的Model值来说，这里会多出来一个概念——改变时机。

什么是改变时机？其实就是如果前端的值发生改变，后端的值在什么时候跟着改变。它由Binding类的UpdateSourceTrigger属性的值决定 。这个属性也是一个枚举类型。

|   |   |
|---|---|
|枚举值|说明|
|Default|采用控件各自的UpdateSourceTrigger默认值。|
|PropertyChanged|每当绑定目标属性发生更改时，都会更新绑定源。|
|LostFocus|每当绑定目标元素失去焦点时，都会更新绑定源。|
|Explicit|仅在调用 System.Windows.Data.BindingExpression.UpdateSource 方法时更新绑定源。|

TextBox.Text 属性的默认值为 LostFocus，我们经常会把TextBox的UpdateSourceTrigger改成PropertyChanged，即文本框在输入内容时，实时的更新后端的Model属性值。

前端属性值发生改变后，由UpdateSourceTrigger决定什么时候更新后端的Model属性值，但是，后端那些Model的属性值发生改变后，前端什么时候跟着改变？需不需要做特殊的处理？

答案是需要！如果Model中的那些属性没有实现属性通知，就算前后端成功的建立了绑定关系，后端Model属性值改变时，前端的显示是没有变化的，如果要实时跟着变化，则需要掌握WPF的INotifyPropertyChanged接口以及属性通知的相关知识点。

关于INotifyPropertyChanged接口，我们将在下一节讲解。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：052-《Binding（绑定）》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年9月14日