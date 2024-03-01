通过前面的模板样式相关知识的学习，接下来，我们选几个具有代表性的控件进行实战演练，以便灵活掌握WPF的模板和样式的应用。

第一步，创建一个数据实体

```cs
public class Sentence : ObservableObject
{
    private string content;
    public string Content
    {
        get { return content; }
        set { content = value; RaisePropertyChanged(); }
    }
}
```

第二步，将数据实体放到ViewModel中

```cs
public class MainViewModel : ObservableObject
{
 
    private ObservableCollection<Sentence> poetries = new ObservableCollection<Sentence>();
    public ObservableCollection<Sentence> Poetries
    {
        get { return poetries; }
        set { poetries = value; RaisePropertyChanged(); }
    }
 
    public MainViewModel()
    {
        Poetries.Add(new Sentence() { Content = "汉皇重色思倾国，御宇多年求不得。" });
        Poetries.Add(new Sentence() { Content = "杨家有女初长成，养在深闺人未识。" });
        Poetries.Add(new Sentence() { Content = "天生丽质难自弃，一朝选在君王侧。" });
        Poetries.Add(new Sentence() { Content = "回眸一笑百媚生，六宫粉黛无颜色。" });
        Poetries.Add(new Sentence() { Content = "春寒赐浴华清池，温泉水滑洗凝脂。" });
        Poetries.Add(new Sentence() { Content = "侍儿扶起娇无力，始是新承恩泽时。" });
        Poetries.Add(new Sentence() { Content = "云鬓花颜金步摇，芙蓉帐暖度春宵。" });
    }
}
```

第三步，在XAML代码中实例化一个ListBox，并修改它的数据模板、样式和元素布局模板。

```xml
 <Grid>
     <Border Width="268" 
             BorderBrush="#DFDFDF" 
             BorderThickness="1" 
             CornerRadius="5" 
             Margin="10">
         <ListBox ItemsSource="{Binding Poetries}" >
             <ListBox.ItemTemplate>
                 <DataTemplate>
                     <Border>
                         <TextBlock Text="{Binding Content}" 
                                    FontSize="14"
                                    Margin="10 5 10 5"/>
                     </Border>
                 </DataTemplate>
             </ListBox.ItemTemplate>
             <ListBox.Style>
                 <Style TargetType="ListBox">
                     <Setter Property="Focusable" Value="False"/>
                     <Setter Property="Padding" Value="0"/>
                     <Setter Property="Margin" Value="0"/>
                     <Setter Property="Background" Value="Transparent"/>
                     <Setter Property="BorderBrush" Value="Transparent"/>
                     <Setter Property="ItemContainerStyle">
                         <Setter.Value>
                             <Style TargetType="ListBoxItem">
                                 <Setter Property="Height" Value="40"/>
                                 <Setter Property="Template">
                                     <Setter.Value>
                                         <ControlTemplate TargetType="ListBoxItem">
                                             <Border Height="{TemplateBinding Height}" 
                                                     BorderThickness="0 0 0 1"
                                                     BorderBrush="#DFDFDF"
                                                     Background="{TemplateBinding Background}">
                                                 <ContentPresenter VerticalAlignment="Center"/>
                                             </Border>
                                         </ControlTemplate>
                                     </Setter.Value>
                                 </Setter>
                                 <Style.Triggers>
                                     <Trigger Property="IsMouseOver" Value="True">
                                         <Setter Property="Background" Value="#F5F7FA"/>
                                     </Trigger>
                                     <Trigger Property="IsSelected" Value="True">
                                         <Setter Property="Background" Value="#F5F7FA"/>
                                     </Trigger>
                                 </Style.Triggers>
                             </Style>
                         </Setter.Value>
                     </Setter>
                 </Style>
             </ListBox.Style>
         </ListBox>
     </Border>        
 </Grid>
```

在ListBoxItem中有一个IsSelected属性，表示当前项是否被选中。ListBoxItem作为ListBox的元素子项而存在，所以我们在ListBox列表控件中选中某一个项时，实际上是将当前项的IsSelected值为True。于是，就可以利用这个属性做一个触发器，当ListBoxItem被选中时，修改其背景颜色。

![[assets/WPF从小白到大佬：模板：：ListBox模板样式实战/b0692680601725fafe4da8baac8a0c73_MD5.jpg]]

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：069-《ListBox模板样式实战》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年10月7日