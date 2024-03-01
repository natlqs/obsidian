DataTrigger数据触发器，它会在绑定数据满足指定条件时应用属性值或执行操作。

```cs
public class DataTrigger : TriggerBase, IAddChild
{
    public DataTrigger();
 
    public BindingBase Binding { get; set; }
    public object Value { get; set; }
    public SetterBaseCollection Setters { get; }
 
    public static void ReceiveMarkupExtension(object targetObject, XamlSetMarkupExtensionEventArgs eventArgs);
 
}
```

DataTrigger拥有一个Binding属性，表明它可以绑定某个控件的属性，或者是某个ViewModel的属性，Value属性则是表示绑定的属性达到某个值时，触发条件成立，然后去执行Setters集合里面的内容。

接下来，我们以一个例子来演示DataTrigger绑定ViewModel属性以及DataTrigger绑定控件属性并触发相应的操作。

前端代码

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

我们将前面的例子稍做修改，采用DataGrid控件绑定Persons集合，在DataGridRow的Style样式中，实例化了4个DataTrigger，前端3个DataTrigger的条件是：当Person.Age等于19、20、21时，将当前行的背景颜色分别改成LightBlue、LightGreen和LightCoral。

注意最后一个DataTrigger，它绑定了CheckBox的IsChecked属性，当IsChecked=True时，将当前行的字体属性进行了一些设置。

最后，我们来生成一些Penson元素，看看前端的呈现效果。

![[assets/WPF从小白到大佬：样式：：DataTrigger数据触发器/d0ac2c35971c0a20f8fb29b5cb2ceda8_MD5.jpg]]

当CheckBox被选中后，即IsChecked=True，此时会将每一行的字体的属性都进行改动。

![[assets/WPF从小白到大佬：样式：：DataTrigger数据触发器/1133717ad7bc893d0ca07ebe2d35d5c3_MD5.jpg]]

完整的代码请在下方下载。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：058-《DataTrigger数据触发器》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年9月18日