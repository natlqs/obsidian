DataContext是FrameworkElement基类的一个属性。其含义：获取或设置元素参与数据绑定时的数据上下文。通常情况下，我们把它设计成一个class——也就是所谓的ViewModel。

那么，什么是ViewModel？这一切，还要从WPF的MVVM模式说起。

![[assets/WPF从小白到大佬：数据绑定：：DataContext数据上下文/e5f10a09843fd392f02ede7f284a5a60_MD5.jpg]]

如图所示，WPF将程序划分为3个部分，分别是View（UI界面或视图）、ViewModel（视图模型）、Model（数据模型）。

![[assets/WPF从小白到大佬：数据绑定：：DataContext数据上下文/091f3ab54812bb1b98eed0272734bb0b_MD5.jpg]]

View负责数据的输入与输出；ViewModel负责业务逻辑；Model则表示程序中具体要处理的数据。所以，Model将作为属性存在于ViewModel中，而Model最终是要显示在Ul界面（View）上的，怎么办呢？将ViewModel赋值给View的DataContext（数据上下文）属性，View就可以引用ViewModel中的那些Model了。

总结：Model在ViewModel中，ViewModel在View的DataContext中，View引用Model。

Model和ViewModel都需要开发者自己实现，而哪些View拥有DataContext属性？WPF所有的控件（包括Window窗体）都有DataContext属性，因为它们都继承于FrameworkElement基类。

> 重庆教主说
> 
> 通常一个Window窗体中有很多控件，我们只需要给Window的DataContext赋值一个ViewModel，窗体中其它的控件的DataContext会共享Window的DataContext。

**只有将ViewModel赋值到DataContext之后，才可以实现数据绑定。**接下来，我们写一个简单的例子演示Model、ViewModel、DataContext属性。

Model类

```cs
public class Person
{
    public string Name { get; set; }
    public int Age { get; set; }
    public string Address { get; set; }
 
}
```

ViewModel类

```cs
public class MainViewModel
{
    public Person Person { get; set; }
 
    public MainViewModel()
    {
        Person = new Person
        {
            Name = "张三",
            Age = 50,
            Address = "居无定所",
        };
    }
}
```

DataContext属性赋值

```cs
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
 
        this.DataContext = new MainViewModel();
    }        
}
```

我们给MainWindow主窗体的DataContext赋值了一个叫MainViewModel的视图模型。而MainViewModel中有一个叫Person的属性，它就代表了程序运行中要处理的数据实体。

现在MainWindow的DataContext（数据上下文）有了一个对象后，我们怎么将这个Person属性显示到前端呢，这需要使用Binding——数据绑定。

下一节，我们来介绍Binding数据绑定。

——重庆教主 2023年9月14日

