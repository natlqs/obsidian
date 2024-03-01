与IValueConverter类似的，还有一个叫IMultiValueConverter——多值转换器。它的定义和IValueConverter也十分类似。

```cs
public interface IMultiValueConverter
{
    object Convert(object[] values, Type targetType, object parameter, CultureInfo culture);
    object[] ConvertBack(object value, Type[] targetTypes, object parameter, CultureInfo culture);
 
}
```

只是第一个参数变成了values，表示它可以传入多个值。

一、IMultiValueConverter示例

```cs
 /// <summary>
 /// 多值转换器
 /// </summary>
 public class MultiColorConverter : IMultiValueConverter
 {
     public object Convert(object[] values, Type targetType, object parameter, CultureInfo culture)
     {
         if (values != null && values.Length == 2)
         {
             var age_result = int.TryParse(values[0].ToString(), out int age);
             var money_result = int.TryParse(values[1].ToString(), out int money);
             if(age_result&& money_result)
             {
                 if (age < 30 && money > 50000)
                 {
                     return "年纪轻轻的有钱人";
                 }
                 else if (age >= 30 && age <= 60 && money < 5000)
                 {
                     return "悲催的中年人";
                 }
                 else if (age < 30 && money < 5000)
                 {
                     return "这个年轻人没什么钱";
                 }
                 else if (age >= 30 && money > 90000)
                 {
                     return "富豪";
                 }
                 else
                 {
                     return "一个平凡的人";
                 }
             }
             
         }
         return null;
     }
 
     public object[] ConvertBack(object value, Type[] targetTypes, object parameter, CultureInfo culture)
     {
         throw new NotImplementedException();
     }
 }
```

如上所示，我们定义了一个多值转换器，values参数传入了两个元素，分别是年龄和金钱。这里为什么能确定是两个元素？因为我们在前端使用这个转换器时，明确的传入了两个值。

```cs
<TextBlock Margin="5" >
    <Run Text="称号:"/>
    <Run>
        <Run.Text>
            <MultiBinding Converter="{StaticResource MultiColorConverter}">
                <Binding Path="Person.Age" />
                <Binding Path="Person.Money"/>
            </MultiBinding>
        </Run.Text>
    </Run>                    
</TextBlock>
```

这里需要着重的讲解一下多值转换器在前端的使用。MultiBinding和IMultiValueConverter通常是配套使用的。MultiBinding表示多路绑定的意思，和Binding的用法类似，只是多了一个Bindings集合——表示拥有多个绑定源。

上面的Path="Person.Age"，表示将DataContext中的Person对象的Age属性作为其中一个绑定源。因为MainViewModel中有一个Person属性，所以直接写到Path中即可。

最后，通过调用MultiColorConverter转换器，判断Age和Money的值，返回一个字符串显示出来。

![[assets/WPF从小白到大佬：数据绑定：：IMultiValueConverter多值转换器/c3f08138d59252b699e079e4b5f4cfdb_MD5.jpg]]

完整的前端代码如下 ：

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
    <Window.Resources>
        <local:AgeToColorConverter x:Key="AgeToColorConverter"/>
        <local:MultiColorConverter x:Key="MultiColorConverter"/>
    </Window.Resources>
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
                    <GridViewColumn Header="财富" DisplayMemberBinding="{Binding Money}" Width="70"/>
                    <GridViewColumn Header="时间" DisplayMemberBinding="{Binding Address}" Width="auto"/>
                </GridView>
            </ListView.View>
        </ListView>
        <StackPanel Grid.Column="1" x:Name="panel" VerticalAlignment="Center" Margin="5,0" 
                    Background="{Binding Person.Age,Converter={StaticResource AgeToColorConverter}}">            
            <StackPanel Orientation="Horizontal">
                <TextBlock Text="姓名:" Margin="5"/>
                <TextBox Text="{Binding Person.Name,UpdateSourceTrigger=PropertyChanged}" Width="145" Height="25"/>
            </StackPanel>
            <StackPanel Orientation="Horizontal">
                <TextBlock Text="年龄:" Margin="5"/>
                <TextBox Text="{Binding Person.Age,UpdateSourceTrigger=LostFocus}" Width="145" Height="25"/>
            </StackPanel>
            <StackPanel Orientation="Horizontal">
                <TextBlock Text="财富:" Margin="5"/>
                <TextBox Text="{Binding Person.Money,UpdateSourceTrigger=LostFocus}" Width="145" Height="25"/>
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
                <Run Text="财富:"/>
                <Run Text="{Binding Person.Money}"/>
            </TextBlock>
            <TextBlock Margin="5" >
                <Run Text="住址:"/>
                <Run Text="{Binding Person.Address}"/>
            </TextBlock>
            <TextBlock Margin="5" >
                <Run Text="称号:"/>
                <Run>
                    <Run.Text>
                        <MultiBinding Converter="{StaticResource MultiColorConverter}">
                            <Binding Path="Person.Age" />
                            <Binding Path="Person.Money"/>
                        </MultiBinding>
                    </Run.Text>
                </Run>                    
            </TextBlock>
            <Button Content="增加用户" Click="Button_Click" Margin="5,0"/>
        </StackPanel>
    </Grid>
    
</Window>
```

完整的后端代码如下：

```cs
	using System;
using System.Collections.Generic;
using System.Collections.ObjectModel;
using System.ComponentModel;
using System.Globalization;
using System.IO;
using System.Runtime.CompilerServices;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Data;
using System.Windows.Media;
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
 
        private int money;
        public int Money
        {
            get { return money; }
            set { money = value; RaisePropertyChanged(); }
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
    /// 多值转换器
    /// </summary>
    public class MultiColorConverter : IMultiValueConverter
    {
        public object Convert(object[] values, Type targetType, object parameter, CultureInfo culture)
        {
            if (values != null && values.Length == 2)
            {
                var age_result = int.TryParse(values[0].ToString(), out int age);
                var money_result = int.TryParse(values[1].ToString(), out int money);
                if(age_result&& money_result)
                {
                    if (age < 30 && money > 50000)
                    {
                        return "年纪轻轻的有钱人";
                    }
                    else if (age >= 30 && age <= 60 && money < 5000)
                    {
                        return "悲催的中年人";
                    }
                    else if (age < 30 && money < 5000)
                    {
                        return "这个年轻人没什么钱";
                    }
                    else if (age >= 30 && money > 90000)
                    {
                        return "富豪";
                    }
                    else
                    {
                        return "一个平凡的人";
                    }
                }
                
            }
            return null;
        }
 
        public object[] ConvertBack(object value, Type[] targetTypes, object parameter, CultureInfo culture)
        {
            throw new NotImplementedException();
        }
    }
 
    public class AgeToColorConverter : IValueConverter
    {
        public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
        {
            SolidColorBrush background = Brushes.Black;
 
            if (value != null && int.TryParse(value.ToString(), out int age))
            {
                if (age < 20)
                {
                    background = Brushes.Green;
                }
                else if (age < 40)
                {
                    background = Brushes.Blue;
                }
                else if (age < 60)
                {
                    background = Brushes.Orange;
                }
                else if (age < 80)
                {
                    background = Brushes.Red;
                }
                else if (age < 90)
                {
                    background = Brushes.Purple;
                }
                else
                {
                    background = Brushes.Gray;
                }
            }
 
            return background;
        }
 
        public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
        {
            throw new NotImplementedException(); 
 
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
 
        int number = 1;
        private void Button_Click(object sender, RoutedEventArgs e)
        {
            var vm = DataContext as MainViewModel;
 
            if (vm == null) return;
 
            Person person = new Person();
            person.Name = "新人" + number++;
            person.Age = new Random().Next(1, 100);
            person.Money = new Random().Next(1, new Random().Next(1,1000000));
            person.Address = DateTime.Now.ToLongTimeString();
 
            vm.Persons.Add(person);
        }
    }
}
```

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：056-《IMultiValueConverter多值转换器》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年9月15日