MultiTrigger表示多个条件同时满足时才会触发的触发器。

```cs
public sealed class MultiTrigger : TriggerBase, IAddChild
{
    public MultiTrigger();
 
    public ConditionCollection Conditions { get; }
    public SetterBaseCollection Setters { get; }
 
}
```

一、属性成员

1.1 Conditions属性

MultiTrigger的Conditions属性是一个集合，表示条件。这个集合的元素是Condition类型。

```cs
public sealed class Condition : ISupportInitialize
{
    public Condition();
    public Condition(DependencyProperty conditionProperty, object conditionValue);
    public Condition(BindingBase binding, object conditionValue);
    public Condition(DependencyProperty conditionProperty, object conditionValue, string sourceName);
 
    public DependencyProperty Property { get; set; }
    public BindingBase Binding { get; set; }
    public object Value { get; set; }
    public string SourceName { get; set; }
 
    public static void ReceiveMarkupExtension(object targetObject, XamlSetMarkupExtensionEventArgs eventArgs);
    public static void ReceiveTypeConverter(object targetObject, XamlSetTypeConverterEventArgs eventArgs);
 
}
```

通常情况下，我们只需要使用Property属性和Value属性即可，即“某个属性达到某个值时”这样一种条件定义。

```cs
<Condition Property="IsMouseOver" Value="True"/>
```

1.2 Setters属性

MultiTrigger的Setters属性也是一个集合，表示触发器触发后要设置的项。比如：

```cs
	                                
<Setter Property="Foreground" Value="Red"/>
```

二、MultiTrigger示例

```cs
<CheckBox x:Name="checkbox">
    <CheckBox.Style>
        <Style TargetType="CheckBox">
            <Setter Property="Content" Value="MultiTrigger"/>
            <Setter Property="Width" Value="150"/>
            <Setter Property="Height" Value="30"/>
            <Setter Property="Background" Value="Orange"/>
            <Setter Property="Foreground" Value="Green"/>
            <Setter Property="Margin" Value="3"/>
            <Style.Triggers>
                <MultiTrigger>
                    <MultiTrigger.Conditions>
                        <Condition Property="IsMouseOver" Value="True"/>
                        <Condition Property="IsChecked" Value="True"/>
                    </MultiTrigger.Conditions>
                    <MultiTrigger.Setters>
                        <Setter Property="Foreground" Value="Red"/>
                        <Setter Property="Content" Value="多条件触发器"/>
                    </MultiTrigger.Setters>
                </MultiTrigger>                        
            </Style.Triggers>
        </Style>
    </CheckBox.Style>
</CheckBox>      
```

在上面的例子中，我们通过Style样式设置了CheckBox的Content及其它属性。然后在样式中实例化了一个MultiTrigger多条件触发器，并为其Conditions集合增加了两个条件，分别是IsMouseOver等于True和IsChecked等于True，当条件满足时，会改变CheckBox的前景色为红色，同时，Content变成"多条件触发器"。

![[assets/WPF从小白到大佬：样式：：MultiTrigger多条件触发器/423d95c4cc33068c4b897ebbefb2c44b_MD5.jpg]]

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：051-《MultiTrigger多条件触发器》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年9月13日