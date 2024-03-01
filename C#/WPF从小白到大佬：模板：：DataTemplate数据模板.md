什么是数据模板？其实就是数据的表现形式——数据外衣。

```cs
public class DataTemplate : FrameworkTemplate
{
    public DataTemplate();
    public DataTemplate(object dataType);
 
    public object DataType { get; set; }
    public TriggerCollection Triggers { get; }
    public object DataTemplateKey { get; }
 
    protected override void ValidateTemplatedParent(FrameworkElement templatedParent);
 
}
```

DataTemplate 继承于FrameworkTemplate基类，它有3个属性，分别是DataType 、Triggers 和DataTemplateKey 。DataType表示当前数据模板所针对的数据类型，Triggers 是触发器集合。

在ItemsControl集合控件中就有一个ItemTemplate属性，它的类型就是DataTemplate 。说明所有继承于ItemsControl的集合子控件都可以设置数据模板。

接下来，我们以ItemsControl控件为例，演示其数据模块的使用方法。

我们将上一章节的例子稍做修改，首先是MainViewModel，新建一个Person的集合作为ItemsControl控件的数据源。

```cs
public class MainViewModel : ObservableObject
{
    private List<Person> persons = new List<Person>();
    public List<Person> Persons
    {
        get { return persons; }
        set { persons = value;RaisePropertyChanged(); }
    }
 
    private Person person;
    public Person Person
    {
        get { return person; }
        set { person = value; RaisePropertyChanged(); }
    }
    public MainViewModel()
    {
        person = new Person()
        {
            Name = "Michael Jackson",
            Occupation = "Musicians",
            Age = 25,
            Money = 9999999,
            Address = "深圳市光明区智慧招商城B4栋5楼"
        };
 
        var bill = new Person()
        {
            Name = "比尔·盖茨（Bill Gates）",
            Occupation = "微软公司创始人",
            Age = 61,
            Money = 9999999,
            Address = "美国华盛顿州西雅图"
        };
 
        var musk = new Person()
        {
            Name = "Elon Reeve Musk",
            Occupation = "首席执行官",
            Age = 50,
            Money = 365214580,
            Address = "出生于南非的行政首都比勒陀利亚"
        };
 
        var jeff = new Person()
        {
            Name = "杰夫·贝索斯（Jeff Bezos）",
            Occupation = "董事会执行主席",
            Age = 25,
            Money = 85745845,
            Address = "杰夫·贝索斯出生于美国新墨西哥州阿尔布奎克。"
        };
 
        persons.Add(person);
        persons.Add(bill);
        persons.Add(musk);
        persons.Add(jeff);
    }
}
```

然后在XAML前端代码中实例化一个ItemsControl控件，并将其包含在ScrollViewer之中。

```xml
<Grid>
    <ScrollViewer>
        <ItemsControl ItemsSource="{Binding Persons}">
            <ItemsControl.ItemTemplate>
                <DataTemplate>
                    <Border x:Name="border"
                        Width="280"
                        Height="200"
                        Margin="5"
                        BorderThickness="1" 
                        BorderBrush="Gray">
                        <Grid>
                            <Grid.RowDefinitions>
                                <RowDefinition/>
                                <RowDefinition/>
                            </Grid.RowDefinitions>
                            <StackPanel Grid.Row="0" Margin="20">
                                <TextBlock Text="{Binding Name}" FontWeight="Bold" FontSize="20"/>
                                <Rectangle Height="5"/>
                                <TextBlock Text="{Binding Occupation}" FontSize="16"/>
                            </StackPanel>
                            <StackPanel Grid.Row="1" Orientation="Horizontal">
                                <TextBlock Grid.Column="0" Text="☻"  
                                           VerticalAlignment="Center"  Margin="20" 
                                           FontSize="50" Foreground="#E26441"/>
                                <StackPanel Margin="30 0 0 0" Width="150">
                                    <TextBlock Text="COMPANY NAME"/>
                                    <TextBlock Text="Age:">
                                        <Run Text="{Binding Age}"/>
                                    </TextBlock>
                                    <TextBlock Text="Money:">
                                        <Run Text="{Binding Money, StringFormat={}{0:C}}"/>
                                    </TextBlock>
                                    <TextBlock Text="Address:" TextWrapping="Wrap">
                                        <Run Text="{Binding Address}"/>
                                    </TextBlock>
                                </StackPanel>
                            </StackPanel>
                        </Grid>
                    </Border>
                    <DataTemplate.Triggers>
                        <Trigger Property="IsMouseOver" Value="True">
                            <Setter Property="Background" Value="#7AAB7D" TargetName="border" />
                        </Trigger>
                    </DataTemplate.Triggers>
                </DataTemplate>
            </ItemsControl.ItemTemplate>
        </ItemsControl>
    </ScrollViewer>    
</Grid>
```

注意：ScrollViewer外面不能是StackPanel，所以这里改成Grid之后，ItemsControl的滚动条效果才会起效。起初我们没有设置ItemsControl 的ItemTemplate(数据模板)，只是绑定了数据源，此时它的效果是下面这样子的。

![[assets/WPF从小白到大佬：模板：：DataTemplate数据模板/77e0ab4ba6061ec25a557ae267a6c1f4_MD5.jpg]]

然后，我们给ItemTemplate属性实例化了一个DataTemplate，其视觉树的样式参考前一节内容。它的效果如下

![[assets/WPF从小白到大佬：模板：：DataTemplate数据模板/27b73391355114157c81448ad83c649d_MD5.jpg]]

由此可见，ItemsControl的元素默认是垂直排列的，因为用于指定元素排列的默认布局控件是StackPanel。如果我们希望改变元素的排列布局方向，则需要修改ItemsControl控件的ItemsPanel属性——即ItemsPanelTemplate元素布局模板。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：066-《DataTemplate数据模板》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年10月7日