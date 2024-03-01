Binding类还有一个Converter属性，其实，它是一个IValueConverter接口。它的主要作用是：前后端建立绑定时，定义一套自定义逻辑，让前端显示的数据与后端获取的数据建立一定的对应关系。

比如Person对象有一个年龄（Age）属性，我们在前端显示某个人的年龄时，可以根据不同的年龄，显示不同的背景颜色。这个时候，实际上是根据这个输入的整型数据返回一个不同颜色的画刷。

一、IValueConverter的定义

```cs
public interface IValueConverter
{
    object Convert(object value, Type targetType, object parameter, CultureInfo culture);
    object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture);
}
 ```

IValueConverter接口有两个方法成员，分别是Convert和ConvertBack。

Convert方法成员：输入的value及parameter参数，根据自定义逻辑判断，返回一个object对象给前端XAML使用。

ConvertBack方法成员：与Convert相反，将前端输入的数据转换成另一个对象返回给后端的数据源。

二、IValueConverter示例

首先，我们定义一个根据年龄转换成不同颜色的画刷的转换器。它必须继承IValueConverter接口

```cs
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
```

然后，我们将上一节的示例代码稍作修改，将前端的StackPanel的背景颜色与当前Person的Age属性用AgeToColorConverter转换器建议绑定关系。在使用转换器之前，一定要先在XAML前端对转换器进行实例化。具体方法如下：

```cs
<Window.Resources>
    <local:AgeToColorConverter x:Key="AgeToColorConverter"/>
</Window.Resources>
```

然后，我们就可以使用这个Key名叫AgeToColorConverter的实例。

```cs
Background="{Binding Person.Age,Converter={StaticResource AgeToColorConverter}}"
```

完整的前端代码如下所示：

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
                    <GridViewColumn Header="地址" DisplayMemberBinding="{Binding Address}" Width="auto"/>
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

![[assets/WPF从小白到大佬：数据绑定：：IValueConverter转换器/13997031edcd5278b562c2a1421d28fe_MD5.jpg]]

当我们选择左侧不同的行，当前的Person就会被更新，于是，当前Person的Age也会更新，同时会调用AgeToColorConverter转换器，将前端StackPanel的背景颜色更新。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：055-《IValueConverter转换器》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年9月15日