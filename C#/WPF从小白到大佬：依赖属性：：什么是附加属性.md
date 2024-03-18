一、附加属性的概念

要阐述附加属性这个概念，我们先简单聊一下属性的概念。属性就像一个标签，比如一个人，他的年龄、姓名、性别，这些都叫属性，且终生相随，不会因为时间或空间不同而不同。但是有些属性则不然，比如我们在学校的时候，填写表格会多一栏班级与年级属性，毕业了就没有这两个属性了，去电影院的时候会有一个座位号的属性，只有结婚了才多一个老婆属性，我们不可能在大街上对迎面走来的5岁小男孩说你老婆是谁？所以，出生就自带的属性我们可称为一般属性，而在特定条件下、特定场合下才有的属性，我们可看成是附加属性。

为什么特定场合下的属性叫附加属性呢？本质上讲，班级与年级是学校才有的概念，是学校主动附加到每一个学生身上的，为了方便管理嘛。去电影院也是如此，如果不给每一个客人一个座位，那大家都抢着坐C位，很可能会发生一些比银幕上还精彩的事情。

在WPF中，我们在学习布局控件时，其实也已经使用过附加属性了。下面我们来看一些例子。

```xml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition/>
        <RowDefinition/>
    </Grid.RowDefinitions>
    <Button Grid.Row="0" Content="按钮1"/>
    <Button Grid.Row="1" Content="按钮1"/>
</Grid>
```

上面的代码中，按钮1被放到Grid的第一行中，按钮2被放到Grid的第二行中。通过Grid.Row附加属性完成这一设置。实际上这个Row属性并没有定义在Button中，而是定义在Grid中，且被定义成附加属性。

```xml
<Canvas>
    <Button Canvas.Left="20" Canvas.Top="20" Content="按钮1"/>
    <Button Canvas.Left="80" Canvas.Top="20" Content="按钮1"/>
</Canvas>
```

观察上面Canvas中的两个按钮，这次为了让它们两个显示在Canvas合适的位置，我们使用了Canvas.Left和Canvas.Top两个属性，分别去设置按钮相对于Canvas的左边距和顶边距。此时，在Canvas类中就定义了Left和Top两个附加属性。

```xml
<DockPanel LastChildFill="False">
    <Button DockPanel.Dock="Left" Content="按钮1"/>
    <Button DockPanel.Dock="Right" Content="按钮1"/>
</DockPanel>
```

观察上面的DockPanel中的两个按钮，这次则采用了DockPanel.Dock附加属性去设置两个按钮的呈现位置。可以相像在DockPanel类中肯定定义了Dock附加属性。

综上所述，附加属性定义好后，是附加到别的控件上起作用的。像酒店一样，客人来开房，酒店给客人房间号，安排客人入住，而不是客人自带一个房间去开房。房间号原本是属于酒店的，只是客人来开房后，才临时属于那个客人，等退房后，房间号便从客人身上消失了。

二、附加属性的定义

在C#后端代码中，键入propa，然后按下tab键，VS会自动创建一个附加属性的定义模板，如下所示。

```cs
public static int GetMyProperty(DependencyObject obj)
{
    return (int)obj.GetValue(MyPropertyProperty);
}
 
public static void SetMyProperty(DependencyObject obj, int value)
{
    obj.SetValue(MyPropertyProperty, value);
}
 
// Using a DependencyProperty as the backing store for MyProperty.
// This enables animation, styling, binding, etc...
public static readonly DependencyProperty MyPropertyProperty =
    DependencyProperty.RegisterAttached(
        "MyProperty", 
        typeof(int), 
        typeof(ownerclass), 
        new PropertyMetadata(0));
```

附加属性利用DependencyProperty的RegisterAttached方法成员进行注册，在注册的时候要求传入一些参数，与注册依赖属性的参数完全相同。

只不过在设置或读取附加属性时，将采用SetMyProperty和GetMyProperty的形式，并最终利用SetValue和GetValue方法成员完成。咦？哪里来的SetValue和GetValue？原来，在DependencyObject基类中定义了这两个方法成员，它们是WPF属性系统的业务核心。所以，作为附加属性的调用者（实际受益人，好比上述代码中的Button对象），这个类可一定要继承DependencyObject基类哦。

——重庆教主 2023年10月26日