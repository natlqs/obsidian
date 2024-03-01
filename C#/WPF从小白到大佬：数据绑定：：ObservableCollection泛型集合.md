`ObservableCollection<T>`泛型集合是一个经常使用的集合，表示一个动态数据集合，它可在添加、删除项目或刷新整个列表时提供通知。它继承了INotifyCollectionChanged和INotifyPropertyChanged，表示它在元素数量发生变化时，前端界面也会同步发生变化。

**一、`ObservableCollection<T>`的定义**

```cs
public class ObservableCollection<T> : Collection<T>, INotifyCollectionChanged, INotifyPropertyChanged
{
    public ObservableCollection();
    public ObservableCollection(List<T> list);
    public ObservableCollection(IEnumerable<T> collection);
 
    public event NotifyCollectionChangedEventHandler CollectionChanged;
    protected event PropertyChangedEventHandler PropertyChanged;
 
    public void Move(int oldIndex, int newIndex);
    protected IDisposable BlockReentrancy();
    protected void CheckReentrancy();
    protected override void ClearItems();
    protected override void InsertItem(int index, T item);
    protected virtual void MoveItem(int oldIndex, int newIndex);
    protected virtual void OnCollectionChanged(NotifyCollectionChangedEventArgs e);
    protected virtual void OnPropertyChanged(PropertyChangedEventArgs e);
    protected override void RemoveItem(int index);
    protected override void SetItem(int index, T item);
 
}
```

它除了继承两个属性通知接口，还继承了一大波的接口，例如：IList, ICollection, IEnumerable, IEnumerable, IList, ICollection, IReadOnlyList, IReadOnlyCollection。所以，它拥有List集合同等的功能，而且，ObservableCollection还可以很方便的转换成List集合。

**二、ObservableCollection示例**

我们来创建一个**ObservableCollection**集合，利用之前学过的ListView控件，演示一下这个集合的功能。

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
    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition/>
            <ColumnDefinition Width="200"/>
        </Grid.ColumnDefinitions>
        <ListView ItemsSource="{Binding Persons}" SelectedItem="{Binding Person}">
            <ListView.View>
                <GridView>
                    <GridViewColumn Header="姓名" DisplayMemberBinding="{Binding Name}" Width="60"/>
                    <GridViewColumn Header="年龄" DisplayMemberBinding="{Binding Age}" Width="auto"/>
                    <GridViewColumn Header="地址" DisplayMemberBinding="{Binding Address}" Width="auto"/>
                </GridView>
            </ListView.View>
        </ListView>
        <StackPanel Grid.Column="1" x:Name="panel" VerticalAlignment="Center" Margin="5,0">
            <StackPanel Orientation="Horizontal">
                <TextBlock Text="姓名:" Margin="5"/>
                <TextBox Text="{Binding Person.Name,UpdateSourceTrigger=PropertyChanged}" Width="145" Height="25"/>
            </StackPanel>
            <StackPanel Orientation="Horizontal">
                <TextBlock Text="年龄:" Margin="5"/>
                <TextBox Text="{Binding Person.Age,UpdateSourceTrigger=LostFocus}" Width="145" Height="25"/>
            </StackPanel>
            <StackPanel Orientation="Horizontal">
                <TextBlock Text="地址:" Margin="5"/>
                <TextBox Text="{Binding Person.Address,UpdateSourceTrigger=Default}" Width="145" Height="25"/>
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
            <Button Content="增加用户" Click="Button_Click" Margin="5,0"/>
        </StackPanel>
    </Grid>
    
</Window>
```

后端代码

```cs
using System;
using System.Collections.Generic;
using System.Collections.ObjectModel;
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
 
        public ObservableCollection<Person> Persons { get; set; } = new ObservableCollection<Person>();
        public MainViewModel()
        {
            
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
 
            Person person = new Person();
            person.Name = "新人";
            person.Age = new Random().Next(1, 100);
            person.Address = DateTime.Now.ToString();
 
            vm.Persons.Add(person);
        }
    }
}
```

![[assets/WPF从小白到大佬：数据绑定：：ObservableCollection泛型集合/ccbf898fd172b4aab9a4e243f158ab5f_MD5.jpg]]

当增加用户时，向Persons集合中增加一个Person对象，此时，前端的ListView便会同步增加一个用户。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：054-《ObservableCollection泛型集合》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年9月14日