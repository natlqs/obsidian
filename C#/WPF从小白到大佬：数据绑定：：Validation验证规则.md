ValidationRule是一个抽象类，提供创建自定义规则的一个方式，旨在检查用户输入的有效性。所以，我们要验证前端输入的各项数据的有效性时，需要自己定义各自的验证规则。

在数据绑定时，Binding类有一个ValidationRules属性，这个属性专门用来存放开发者自定义的验证规则。

例如，我们假定用户名的长度必须在1-10个字符之间，且用户的年龄在1-100之前，那么就可以围绕这两个条件自定义两个不同的验证规则，它们定义如下：

用户名验证规则

```cs
public class NameValidationRule : ValidationRule
{
    public override ValidationResult Validate(object value, CultureInfo cultureInfo)
    {
        if (value != null && value.ToString().Length > 1 && value.ToString().Length <= 10)
        {
            return new ValidationResult(true, "通过");
        }
 
        return new ValidationResult(false, "用户名长度1-10个字符");
    }
}
```

年龄验证规则

```cs
public class AgeValidationRule : ValidationRule
{
    public override ValidationResult Validate(object value, CultureInfo cultureInfo)
    {
        double myValue = 0;
        if (double.TryParse(value.ToString(), out myValue))
        {
            if (myValue >= 1 && myValue <= 100)
            {
                return new ValidationResult(true, null);
            }
        }
 
        return new ValidationResult(false, "请输入 1 至 100的年龄");
    }
}
```

在XAML前端代码中，TextBox输入框分别绑定了用户名和年龄，它们在绑定时如何调用验证规则呢？

前端代码

```cs
<StackPanel Orientation="Horizontal">
    <TextBlock Text="姓名:" Margin="5"/>
    <TextBox Width="145" Height="25">
        <TextBox.Text>
            <Binding Path="Person.Name" UpdateSourceTrigger="PropertyChanged">
                <Binding.ValidationRules>
                    <local:NameValidationRule ValidatesOnTargetUpdated="True" />
                </Binding.ValidationRules>
            </Binding>
        </TextBox.Text>
        <Validation.ErrorTemplate>
            <ControlTemplate>
                <DockPanel>
                    <Grid DockPanel.Dock="Right" Width="auto" Height="auto" VerticalAlignment="Center" Margin="3 0 0 0">
                        <TextBlock Width="auto" Height="auto" Foreground="Red"
                                 Text="{Binding ElementName=AdornedElementPlaceholder, Path=AdornedElement.(Validation.Errors).CurrentItem.ErrorContent}"/>
                    </Grid>
                    <Border BorderBrush="Red" BorderThickness="0" CornerRadius="2">
                        <AdornedElementPlaceholder x:Name="AdornedElementPlaceholder"/>
                    </Border>
                </DockPanel>
            </ControlTemplate>
        </Validation.ErrorTemplate>
    </TextBox>
</StackPanel>
<StackPanel Orientation="Horizontal">
    <TextBlock Text="年龄:" Margin="5"/>
    <TextBox Width="145" Height="25">
        <TextBox.Text>
            <Binding Path="Person.Age" UpdateSourceTrigger="PropertyChanged">
                <Binding.ValidationRules>
                    <local:AgeValidationRule ValidatesOnTargetUpdated="True" />
                </Binding.ValidationRules>
            </Binding>
        </TextBox.Text>
        <Validation.ErrorTemplate>
            <ControlTemplate>
                <DockPanel>
                    <Grid DockPanel.Dock="Right" Width="auto" Height="auto" VerticalAlignment="Center" Margin="3 0 0 0">
                        <TextBlock Width="auto" Height="auto" Foreground="Red"
                                 Text="{Binding ElementName=AdornedElementPlaceholder, Path=AdornedElement.(Validation.Errors).CurrentItem.ErrorContent}"/>
                    </Grid>
                    <Border BorderBrush="Red" BorderThickness="0" CornerRadius="2">
                        <AdornedElementPlaceholder x:Name="AdornedElementPlaceholder"/>
                    </Border>
                </DockPanel>
            </ControlTemplate>
        </Validation.ErrorTemplate>
    </TextBox>
</StackPanel>
```

ValidationRule会把验证结果保存在AdornedElementPlaceholder的AdornedElement属性中，所以，需要利用绑定的方法去绑定下面这个路径。

AdornedElement.(Validation.Errors).CurrentItem.ErrorContent

最后，我们来看一下效果。

![[assets/WPF从小白到大佬：数据绑定：：Validation验证规则/5afe1487f69d4ca1caad99c0dfba7d15_MD5.jpg]]

完整的源代码请在下面的链接中下载。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：057-《ValidationRule验证规则》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年9月15日