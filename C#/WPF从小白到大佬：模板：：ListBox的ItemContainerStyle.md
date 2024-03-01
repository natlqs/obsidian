众所周知，ListBox继承于ItemsControl控件，那么，它就与ItemsControl一样，拥有了可以设置的数据模板。当然，它也可以拥有自己的控件模板（在Control基类中定义的Template）。这一节，我们只探讨一下ListBox如何使用数据模板。

我们可以将上一章节中的ItemsControl直接改成ListBox。

```xml
<ListBox ItemsSource="{Binding Persons}" >
    <ItemsControl.ItemsPanel>
        <ItemsPanelTemplate>
            <WrapPanel/>
        </ItemsPanelTemplate>
    </ItemsControl.ItemsPanel>
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
</ListBox>```

![[assets/WPF从小白到大佬：模板：：ListBox的ItemContainerStyle/52db1605001e196ded75ae973345c010_MD5.jpg]]

因为ListBox是ItemsControl的子类，所以，这样的修改是没有问题的。只不过，如上图所示，在每个元素的外围，当鼠标移上去时，会出现一个淡蓝色的边框区域，这是为何呢？

这是因为在ListBox的父类[ItemsControl](http://www.wpfsoft.com/2023/08/31/1697.html)中定义了一个ItemContainerStyle的样式，这个样式决定了ListBox控件中每个元素的容器外观。原来，在集合控件中，并不是说将一堆元素直接丢到里面呈现，而是先给每个元素分配一个容器，再将它们呈现在集合控件中。就好比给每个学生发一套校服，穿好后再规规距距地坐在教室里。

既然如此，那我们就可以给每个学生重新发一套校服，或者干脆不穿校服——毕竟每个学生自己都穿了衣服的（数据模板）。

```xml
<ListBox.ItemContainerStyle>
    <Style TargetType="ListBoxItem">
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="ListBoxItem">
                    <ContentPresenter/>
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
</ListBox.ItemContainerStyle>
```

ItemContainerStyle的Template的内容必须是ControlTemplate （控件模板）。这里同样使用了ContentPresenter，我们已然在前面讲过，这里指的是，将来由每个元素进行替换。注意TargetType是ListBoxItem类型。因为这个校服的品牌方就是指ListBox的ListBoxItem元素。

![[assets/WPF从小白到大佬：模板：：ListBox的ItemContainerStyle/b65ed444288499392dfbe6cf76aa3480_MD5.jpg]]

如果我们要给每个学生穿一件金黄色的衣服，如下所示

```xml
<ListBox.ItemContainerStyle>
    <Style TargetType="ListBoxItem">
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="ListBoxItem">
                    <Border Background="LightGoldenrodYellow" 
                            Padding="15" Margin="5">
                        <ContentPresenter/>
                    </Border>                                
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
</ListBox.ItemContainerStyle>
```

![[assets/WPF从小白到大佬：模板：：ListBox的ItemContainerStyle/923b6cfac5092e5a33ff77e6f8fadb7b_MD5.jpg]]

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：068-《ListBox的ItemContainerStyle》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff