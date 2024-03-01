StatusBar是一个“包容性”极强的控件，通常的作用是作为程序的状态内容显示。它同样继承于ItemsControl基类，所以，它也是一个集合控件。

它的元素是StatusBarItem类型，而StatusBarItem继承于ContentControl内容控件，所以，本质上讲，StatusBar的元素可以是任意类型的控件。因为StatusBarItem元素有一个叫Content的属性。

这个控件其实并不常用，通常情况下被当成一个布局控件来使用。如下所示

```cs
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition/>
        <RowDefinition Height="auto"/>
    </Grid.RowDefinitions>
    <StatusBar Grid.Row="1">
        <StatusBarItem Content="版权所有 @WPF中文网"/>
        <StatusBarItem>
            <CheckBox Content="CheckBox"/>
        </StatusBarItem>
        <StatusBarItem>
            <RadioButton Content="RadioButton"/>
        </StatusBarItem>
        <StatusBarItem>
            <Button Content="Button"/>
        </StatusBarItem>
        <TextBlock Text="文字块"/>
    </StatusBar>
</Grid>
```

![[assets/WPF从小白到大佬：集合控件：：StatusBar状态栏/e78a167004f03da0e9760c3eee337988_MD5.jpg]]

如上所示，StatusBar的元素除了StatusBarItem，甚至可以直接实例化其它控件，比如最后一个TextBlock就是这样的用法。

——重庆教主 2023年9月8日