WPF的PasswordBox的Password属性不是依赖属性，所以不能绑定。为了实现这个控件的MVVM模式，我们可以利用附加属性实现PasswordBox控件的绑定使用。其主要的思想是，设计一个PasswordBoxHelper类型，并在其中定义一个附加属性，这个属性的名称也叫Password，将来作为PasswordBox控件的附加属性，假设我们有一个Person实体，这个实体有一个UserName属性和一个Password属性，分别表示用户登录时填写的账户和密码。通过PasswordBoxHelper，可以将PasswordBox控件与Person实体建立数据绑定的桥梁，换句话说，PasswordBox的Password属性、PasswordBoxHelper中的Password属性和Person实体中的Password属性三者建立了某种连接。形象表达如下：

PasswordBox.Password -> PasswordBoxHelper.Password -> Person.Password

为此，我们先做一些准备工作。

第一步，创建一个ObservableObject类型，用来实现属性通知。

```cs
public class ObservableObject : INotifyPropertyChanged
{
    public event PropertyChangedEventHandler PropertyChanged;
 
    public void RaisePropertyChanged([CallerMemberName] string propertyName = "")
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

第二步，创建一个Person实体。

```cs
public class Person : ObservableObject
{
    private string username;
    public string UserName
    {
        get { return username; }
        set { username = value;RaisePropertyChanged(); }
    }
 
    private string password;
    public string Password
    {
        get { return password; }
        set { password = value; RaisePropertyChanged(); }
    }
}
```

第三步，创建一个MainViewModel，并在其中实例化Person。

```cs
public class MainViewModel : ObservableObject
{
    private Person person = new Person();
    public Person Person
    {
        get { return person; }
        set { person = value;RaisePropertyChanged(); }
    }
}
```

第四步，将MainViewModel赋值给主窗体。

```xml
<Window.DataContext>
    <local:MainViewModel/>
</Window.DataContext>
```

第五步，创建PasswordBoxHelper类，实现附加属性的业务逻辑。

```cs
public class PasswordBoxHelper
{
    public static string GetPassword(DependencyObject obj)
    {
        return (string)obj.GetValue(PasswordProperty);
    }
 
    public static void SetPassword(DependencyObject obj, string value)
    {
        obj.SetValue(PasswordProperty, value);
    }
 
    public static readonly DependencyProperty PasswordProperty =
        DependencyProperty.RegisterAttached(
            "Password", typeof(string), typeof(PasswordBoxHelper), 
            new PropertyMetadata("",
                new PropertyChangedCallback(OnPasswordPropertyChangedCallback)));
 
    private static void OnPasswordPropertyChangedCallback(
        DependencyObject d, DependencyPropertyChangedEventArgs e)
    {
        if(d is PasswordBox passwordBox)
        {
            passwordBox.PasswordChanged -= PasswordBox_PasswordChanged;
            passwordBox.PasswordChanged += PasswordBox_PasswordChanged;
        }
    }
 
    private static void PasswordBox_PasswordChanged(object sender, RoutedEventArgs e)
    {
        if(sender is PasswordBox passwordBox)
        {
            SetPassword(passwordBox, passwordBox.Password);
        }
    }
}
```

分析PasswordBoxHelper的业务实现

这时我们定义了一个PasswordProperty 附加属性，当PasswordProperty 的值发生改变时，会调用OnPasswordPropertyChangedCallback回调函数，在这个回调函数中，我们会拿到PasswordBox 控件，并订阅它的PasswordChanged ，在PasswordChanged 事件中调用SetPassword()，实际上这里就是将PasswordBox 控件的值赋值到PasswordProperty ，而PasswordProperty 又在前端绑定了Person.Password，于是，PasswordBox的值就给到了Person.Password。

第六步，编写XAML前端代码，这里我们实例化了一个TextBox控件，将其绑定到Person的UserName属性上，实例化了一个PasswordBox控件，在其中引用了PasswordBoxHelper.Password附加属性，注意，引用之前要将其命名空间写好：xmlns:helper="clr-namespace:HelloWorld.MVVM"

```xml
<Window x:Class="HelloWorld.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:HelloWorld" 
        xmlns:controls="clr-namespace:HelloWorld.Controls"
        xmlns:helper="clr-namespace:HelloWorld.MVVM"
        mc:Ignorable="d" 
        Title="WPF中文网 - www.wpfsoft.com" Height="350" Width="500">
    <Window.DataContext>
        <local:MainViewModel/>
    </Window.DataContext>
    <StackPanel Margin="80">
        <StackPanel Orientation="Horizontal">
            <TextBlock Text="用户：" VerticalAlignment="Center"/>
            <TextBox Text="{Binding Person.UserName,UpdateSourceTrigger=PropertyChanged}" 
                     Width="200" Height="25"/>
            <TextBlock Text="{Binding Person.UserName}" VerticalAlignment="Center" Margin="5 0"/>
        </StackPanel>
        <Rectangle Height="10"/>
        <StackPanel Orientation="Horizontal">
            <TextBlock Text="密码：" VerticalAlignment="Center"/>
            <PasswordBox helper:PasswordBoxHelper.Password="{Binding Person.Password,Mode=TwoWay,UpdateSourceTrigger=PropertyChanged}"  
                         Width="200" Height="25"/>
            <TextBlock Text="{Binding Person.Password}" VerticalAlignment="Center" Margin="5 0"/>
        </StackPanel>
        <Rectangle Height="10"/>
        <Button Content="登录" Width="200" Height="25" HorizontalAlignment="Left" Margin="35,0"/>
    </StackPanel>
    
</Window>
```

最后，F5运行调试结果如下

![[assets/WPF从小白到大佬：依赖属性：：PasswordBox与附加属性实战/38573fb3adb11b29bd5f70c44cbf695b_MD5.jpg]]

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：085-《PasswordBox与附加属性实战》-源代码.rar  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年10月26