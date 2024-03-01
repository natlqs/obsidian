当学会了DataTrigger数据触发器和MultiTrigger多条件触发器，MultiDataTrigger就比较好理解了，它就是前面两个触发器的结合体。

```cs
public sealed class MultiDataTrigger : TriggerBase, IAddChild
{
    public MultiDataTrigger();
 
    public ConditionCollection Conditions { get; }
    public SetterBaseCollection Setters { get; }
 
}
```

从定义上看，它与MultiTrigger多条件触发器一模一样，只是内部的实现略有不同。我们可以将一节的例子稍作修改，来演示一下MultiDataTrigger的用法。

```xml
<DataGrid ItemsSource="{Binding Persons}" SelectedItem="{Binding Person}" AutoGenerateColumns="False">
    <DataGrid.RowStyle>
        <Style TargetType="DataGridRow">
            <Style.Triggers>
                <DataTrigger Binding="{Binding Age}" Value="19">
                    <Setter Property="Background" Value="LightBlue"/>
                </DataTrigger>
                <DataTrigger Binding="{Binding Age}" Value="20">
                    <Setter Property="Background" Value="LightGreen"/>
                </DataTrigger>
                <DataTrigger Binding="{Binding Age}" Value="21">
                    <Setter Property="Background" Value="LightCoral"/>
                </DataTrigger>
                <DataTrigger Binding="{Binding ElementName=_CheckBox,Path=IsChecked}" Value="True">
                    <Setter Property="Foreground" Value="Red"/>
                    <Setter Property="FontSize" Value="16"/>
                    <Setter Property="FontWeight" Value="Bold"/>
                </DataTrigger>
                <MultiDataTrigger>
                    <MultiDataTrigger.Conditions>
                        <Condition Binding="{Binding Path=Age}" Value="20"/>
                        <Condition Binding="{Binding Path=Name}" Value="张三"/>
                    </MultiDataTrigger.Conditions>
                    <MultiDataTrigger.Setters>
                        <Setter Property="Foreground" Value="Red"/>
                        <Setter Property="FontSize" Value="16"/>
                        <Setter Property="FontWeight" Value="Bold"/>
                    </MultiDataTrigger.Setters>
                </MultiDataTrigger>
            </Style.Triggers>
        </Style>
    </DataGrid.RowStyle>
    <DataGrid.Columns>
        <DataGridTextColumn Header="姓名" Binding="{Binding Name}"/>
        <DataGridTextColumn Header="年龄" Binding="{Binding Age}"/>
        <DataGridTextColumn Header="生日" Binding="{Binding Address}"/>
    </DataGrid.Columns>
    <!--<ListView.View>
        <GridView>
            <GridViewColumn Header="姓名" DisplayMemberBinding="{Binding Name}" Width="60"/>
            <GridViewColumn Header="年龄" DisplayMemberBinding="{Binding Age}" Width="auto"/>
            <GridViewColumn Header="地址" DisplayMemberBinding="{Binding Address}" Width="auto"/>
        </GridView>
    </ListView.View>-->
</DataGrid>
```

在上面的前端代码中，我们增加了一个MultiDataTrigger触发器的实例。

```xml
<MultiDataTrigger>
    <MultiDataTrigger.Conditions>
        <Condition Binding="{Binding Path=Age}" Value="20"/>
        <Condition Binding="{Binding Path=Name}" Value="张三"/>
    </MultiDataTrigger.Conditions>
    <MultiDataTrigger.Setters>
        <Setter Property="Foreground" Value="Red"/>
        <Setter Property="FontSize" Value="16"/>
        <Setter Property="FontWeight" Value="Bold"/>
    </MultiDataTrigger.Setters>
</MultiDataTrigger>
```

条件是，当Person是一个20岁的“张三”时，把当前行的字体进行了一些特别设置。

![[assets/WPF从小白到大佬：样式：：MultiDataTrigger数据触发器/a93ef3eaf80c94a5e0db1f8652189486_MD5.jpg]]

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：059-《MultiDataTrigger数据触发器》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年9月18日