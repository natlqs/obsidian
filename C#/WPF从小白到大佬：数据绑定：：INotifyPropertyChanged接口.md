INotifyPropertyChanged接口，通知客户端属性值已更改。

```cs
public interface INotifyPropertyChanged
{
    event PropertyChangedEventHandler PropertyChanged;
}
```

这个接口只有一个PropertyChanged事件，该事件专门用来触发属性更改通知。通常情况下，我们会单独编写一个服务类（例如ObservableObject），以实现INotifyPropertyChanged接口的业务。这样做的好处是，将来的ViewModel、Model都可以继承这个ObservableObject，从而调用属性通知接口。

**一、实现INotifyPropertyChanged接口**

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

使用了CallerMemberName特性后，就不必再传入属性名字符串。

接下来，我们来编写一个ViewModel和一个Model，让它们都继承这个属性通知基类。

Person实体

```cs
public class Person : ObservableObject
{
    private string name;
    public string Name
    {
        get { return name; }
        set { name = value;RaisePropertyChanged(); }
    }
 
    private int age;
    public int Age
    {
        get { return age; }
        set { age = value; RaisePropertyChanged(); }
    }
 
    private string address;
    public string Address
    {
        get { return address; }
        set { address = value; RaisePropertyChanged(); }
    }
}
```

MainViewModel实体

```cs
public class MainViewModel : ObservableObject
{
    private Person person;
    public Person Person
    {
        get { return person; }
        set { person = value; RaisePropertyChanged(); }
    }
 
    public MainViewModel()
    {
        person = new Person
        {
            Name = "张三",
            Age = 50,
            Address = "居无定所",
        };
    }
}
```

我们修改了前面章节中的Person和MainViewModel两个类型，其中所有的属性都写成了完整的属性包装器形式，并在set语句块中增加了RaisePropertyChanged()方法成员的调用。

首先分析Person类，当修改Name、Age、Address属性时，调用RaisePropertyChanged()，此时如果前端UI有这几个属性的绑定，将会立即被更新内容。

然后是MainViewModel，这里的Person属性被重新赋值时，也将会通知到前端。如果MainViewModel中这个Person没有实现属性通知，那么，Person中的Name、Age、Address属性发生更改时，前端UI的内容会发生同步更新，但是，如果对Person属性本身重新赋值，前端UI是不会发生改变的。这一点细节需要了解清楚哦。

二、数据绑定示例

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
        Title="WPF中文网之数据绑定 - www.wpfsoft.com" Height="350" Width="500">
    <StackPanel x:Name="panel" VerticalAlignment="Center" Margin="80,0">
        <StackPanel Orientation="Horizontal">
            <TextBlock Text="姓名:" Margin="5"/>
            <TextBox Text="{Binding Person.Name,UpdateSourceTrigger=PropertyChanged}" Width="200" Height="25"/>
        </StackPanel>
        <StackPanel Orientation="Horizontal">
            <TextBlock Text="年龄:" Margin="5"/>
            <TextBox Text="{Binding Person.Age,UpdateSourceTrigger=LostFocus}" Width="200" Height="25"/>
        </StackPanel>
        <StackPanel Orientation="Horizontal">
            <TextBlock Text="地址:" Margin="5"/>
            <TextBox Text="{Binding Person.Address,UpdateSourceTrigger=Default}" Width="200" Height="25"/>
        </StackPanel>
        <TextBlock Margin="5" >
                <Run Text="姓名:"/>
                <Run Text="{Binding Person.Name}"/>
        </TextBlock>
        <TextBlock Margin="5" >
                <Run Text="年龄:"/>
                <Run Text="{Binding Person.Age}"/>
        </TextBlock>
        <TextBlock Margin="5" >
                <Run Text="住址:"/>
                <Run Text="{Binding Person.Address}"/>
        </TextBlock>
        <Button Content="随机更改内容" Click="Button_Click"/>
    </StackPanel>
</Window>
```

后端代码

```cs
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.IO;
using System.Runtime.CompilerServices;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Threading;
 
namespace HelloWorld
{
    public class ObservableObject : INotifyPropertyChanged
    {
        public event PropertyChangedEventHandler PropertyChanged;
 
        public void RaisePropertyChanged([CallerMemberName] string propertyName = "")
        {
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
        }
    }
    public class Person : ObservableObject
    {
        private string name;
        public string Name
        {
            get { return name; }
            set { name = value;RaisePropertyChanged(); }
        }
 
        private int age;
        public int Age
        {
            get { return age; }
            set { age = value; RaisePropertyChanged(); }
        }
 
        private string address;
        public string Address
        {
            get { return address; }
            set { address = value; RaisePropertyChanged(); }
        }
    }
 
    public class MainViewModel : ObservableObject
    {
        private Person person;
        public Person Person
        {
            get { return person; }
            set { person = value; RaisePropertyChanged(); }
        }
 
        public MainViewModel()
        {
            person = new Person
            {
                Name = "张三",
                Age = 50,
                Address = "居无定所",
            };
        }
    }
 
    /// <summary>
    /// MainWindow.xaml 的交互逻辑
    /// </summary>
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
 
            this.DataContext = new MainViewModel();
        }
 
        private void Button_Click(object sender, RoutedEventArgs e)
        {
            var vm = DataContext as MainViewModel;
 
            if (vm == null) return;
 
            vm.Person.Age = new Random().Next(1, 100);
            vm.Person.Address = DateTime.Now.ToString();
        }
    }
}
```

![[assets/WPF从小白到大佬：数据绑定：：INotifyPropertyChanged接口/232d8af298f525c2c6f6dfaae7ef24f5_MD5.jpg]]

在上面的例子中，我们在主窗体的构造函数中实例化了一个MainViewModel，并赋值给了DataContext。在前端代码中，我们分别用TextBlock和TextBox绑定了MainViewModel的Person对象。当在TextBox中改变内容时，可以看到下面的TextBlock的内容也会同步更新，注意UpdateSourceTrigger的不同用法。

在按钮的单击事件中，我们从DataContext属性中将MainViewModel取出来，并在后端修改了Person的Age和Address，与此同时，前端的显示也会同步更新。说明我们的属性通知起了作用。

除了INotifyPropertyChanged接口可以实现属性通知，还有一个接口也可以实现属性通知，它就是INotifyCollectionChanged。它的作用是：当添加和删除项或清除整个列表时，向侦听器通知动态更改。也就是说，它是专门来解决集合的元素数量发生变化时的通知问题的。我们在下一节来演示它的用法。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：053-《INotifyPropertyChanged接口》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年9月14日