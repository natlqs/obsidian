
官方的说法是：命令是WPF 中的一种输入机制，与设备输入相比，它提供的输入处理更侧重于语义级别。我觉得这样的解释有点抽象，在实际的使用过程中我们会发现，传统的事件驱动模式下，比如一个按钮的Click单击事件，我们会给它订阅一个回调函数，以便触发该事件时，去执行这个回调函数的业务逻辑代码。

```cs
Button button = new Button();
button.Click += (s, e) =>
{
    MessageBox.Show("HelloWorld");
};
```

或者在XAML前端实例化一个Button，然后订阅Click事件。

```xml
//XAML代码
<Grid>
    <Button Content="确定" Click="Button_Click"/>
</Grid>
 
//C#代码
private void Button_Click(object sender, RoutedEventArgs e)
{
    MessageBox.Show("HelloWorld");
}
```

事件驱动模式有一个不太好的地方是，它的前端XAML代码和后端的C#代码建立了一种强关联的耦合关系，无法体现WPF的MVVM开发模式的优势， 于是，WPF提供了一个ICommand接口，以及一个强大的命令系统，将控件的各种事件都能转换成一个命令。这个命令依然像绑定依赖属性一样，将命令定义成一个ViewModel中的命令属性，而ViewModel又被赋值到控件或窗体的DataContext数据上下文中，于是，窗体内的所有控件的事件都可以绑定这个命令，只要控件的事件被触发，命令将会被执行。

WPF的命令是通过ICommand接口创建的，并且微软已经为我们预定义了一部分命令，这些我们都称为预定义命令，它们的类型是RoutedUICommand或RoutedCommand。RoutedUICommand继承于RoutedCommand，而RoutedCommand继承于ICommand。

事实上，在WPF应用开发过程中，光靠这些预定义命令是不足以满足我们的开发需求的，所以我们肯定要另写一些命令类型，当然，自定义命令必须继承于ICommand。

厘清这一点之后，我们就明白了接下来要学习的几个方面：

- 第一点：命令的4个主要概念，它解决了我们如何正确运用命令知识。
- 第二点：预定义命令的熟悉与使用。
- 第三点：自定义命令的定义与应用。
- 第四点：常见框架（例如Prism，mvvmlight，ReactiveUI）中的命令应用。
- 第五点：控件的普通事件转Command命令

**第一点，WPF 命令中的四个主要概念**

WPF 中的路由命令模型可分解为四个主要概念：命令、命令源、命令目标和命令绑定：

- _命令_是要执行的操作。
- _命令源_是调用命令的对象。
- _命令目标_是在其上执行命令的对象。
- _命令绑定_是将命令逻辑映射到命令的对象。

如上所述，**命令**其实就是ICommand接口的实现，**命令源**是调用命令的对象，这些对象一定会继承ICommandSource接口，而**命令绑定**就像是一座桥梁，它将命令与逻辑代码建立一种映射，这座桥梁就是CommandBinding。最后，命令如何与命令源建立绑定？当然就是**Binding对象**了。

较形象的总结一下命令的路线：ICommandSource命令源（或控件的事件）->绑定一个ICommand命令->CommandBinding桥梁->真正的业务逻辑代码块。

下一节，我们以这个路线先介绍命令源。

——重庆教主 2023年10月11日
