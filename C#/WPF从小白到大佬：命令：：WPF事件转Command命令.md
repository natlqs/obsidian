通过前面的学习，我们发现Button拥有Command属性，开发者可以为其设置一条命令，当用户单击按钮的时候便执行这条命令。但是，一个控件往往不止一个事件，比如UIElement基类中便定义了大量的事件，PreviewMouseDown表示鼠标按下时引发的事件。

如何在PreviewMouseDown事件触发时去执行一条命令呢？这时候就需要用到WPF提供的一个组件，它的名字叫Microsoft.Xaml.Behaviors.Wpf，我们可以在nuget管理器中找到并下载安装它。

![[assets/WPF从小白到大佬：命令：：WPF事件转Command命令/d0b6ad1029922f503a21402bebde696e_MD5.jpg]]

安装Microsoft.Xaml.Behaviors.Wpf。

![[assets/WPF从小白到大佬：命令：：WPF事件转Command命令/4fc8cc4fee39a760e8581df8d13f8b86_MD5.jpg]]

然后，我们在window窗体中引入它的命名空间。

xmlns:i="http://schemas.microsoft.com/xaml/behaviors"

最后，我们以TextBox为例，因为TextBox也是UIElement基类的子类控件，所以，它也有PreviewMouseDown事件。

```xml
<TextBox x:Name="textbox" Grid.Row="1" Margin="5" TextWrapping="Wrap">
    <i:Interaction.Triggers>
        <i:EventTrigger EventName="PreviewMouseDown">
            <i:InvokeCommandAction Command="{Binding MouseDownCommand}"
                                   CommandParameter="{Binding RelativeSource={RelativeSource Mode=FindAncestor,AncestorType=TextBox}}">
            </i:InvokeCommandAction>
        </i:EventTrigger>
    </i:Interaction.Triggers>
</TextBox>
```

从上面的例子中，我们会发现，在TextBox 控件中增加了一个Interaction.Triggers附加属性，这个属性是一个集合，表示可以实例化多个Trigger触发器，于是，我们实例化了一个EventTrigger ，并指明它的EventName为PreviewMouseDown事件，关联的命令要写在InvokeCommandAction 对象中，命令绑定的方式采用Binding即可。然后我们来看看MouseDownCommand的执行代码：

```cs
public RelayCommand<TextBox> MouseDownCommand { get; set; } = new RelayCommand<TextBox>((textbox) =>
{
    textbox.Text += DateTime.Now + " 您单击了TextBox" + "\r";
});
```

在TextBox控件上单击鼠标时，执行MouseDownCommand 命令，同时将TextBox控件传入到命令的匿名回调函数中。

![[assets/WPF从小白到大佬：命令：：WPF事件转Command命令/c831c8c73f06eaebb161d5c1e22de667_MD5.jpg]]

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：074-《WPF事件转命令》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年10月12日