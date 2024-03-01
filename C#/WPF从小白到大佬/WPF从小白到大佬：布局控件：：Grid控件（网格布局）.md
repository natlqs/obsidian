Grid控件其实是一个窗体的默认控件，我们创建一个WPF应用程序后，其主窗体里面会有一个Grid控件。我们先来看一下此类的结构定义：

```cs
public class Grid : Panel, IAddChild
{
    public static readonly DependencyProperty ShowGridLinesProperty;
    public static readonly DependencyProperty ColumnProperty;
    public static readonly DependencyProperty RowProperty;
    public static readonly DependencyProperty ColumnSpanProperty;
    public static readonly DependencyProperty RowSpanProperty;
    public static readonly DependencyProperty IsSharedSizeScopeProperty;
 
    public Grid();
 
    public ColumnDefinitionCollection ColumnDefinitions { get; }
    public bool ShowGridLines { get; set; }
    public RowDefinitionCollection RowDefinitions { get; }
    protected override int VisualChildrenCount { get; }
    protected internal override IEnumerator LogicalChildren { get; }
 
    public static int GetColumn(UIElement element);
    public static int GetColumnSpan(UIElement element);
    public static bool GetIsSharedSizeScope(UIElement element);
    public static int GetRow(UIElement element);
    public static int GetRowSpan(UIElement element);
    public static void SetColumn(UIElement element, int value);
    public static void SetColumnSpan(UIElement element, int value);
    public static void SetIsSharedSizeScope(UIElement element, bool value);
    public static void SetRow(UIElement element, int value);
    public static void SetRowSpan(UIElement element, int value);
    public bool ShouldSerializeColumnDefinitions();
    public bool ShouldSerializeRowDefinitions();
    protected override Size ArrangeOverride(Size arrangeSize);
    protected override Visual GetVisualChild(int index);
    protected override Size MeasureOverride(Size constraint);
    protected internal override void OnVisualChildrenChanged(DependencyObject visualAdded, DependencyObject visualRemoved);
 
}
```

Grid有两个非常关键的属性ColumnDefinitions和RowDefinitions，分别表示列的数量集合和行的数量集合。ColumnDefinitions集合中的元素类型是ColumnDefinition类，RowDefinitions集合中元素类型是RowDefinition类。默认的Gridr控件没有定义行数和列数，也就是说，Grid默认情况下，行数和列数都等于1，那么它就只有一个单元格。

```cs
<Grid >
    <Button  Content="WPF中文网1" Panel.ZIndex="1" Margin="20 40 60 80" Padding="50" />
    <Button  Content="WPF中文网2" Panel.ZIndex="0" Margin="20 40 60 80" Padding="50" />
</Grid>
```

如上述代码所示，此时的Grid因为只有一个单元格，而Grid的Children属性里面有两个Button，势必有一个Button会被遮盖。假如我们希望两个按钮同时显示，应该怎么办呢？这时就有了两个选格，第一，两个Button左右排列显示，第二，两个Button上下排列显示。

一、**左右排列**

```cs
<Grid >
    <Grid.ColumnDefinitions>
        <ColumnDefinition/>
        <ColumnDefinition/>
    </Grid.ColumnDefinitions>
    <Button Grid.Column="0" Content="WPF中文网1" Panel.ZIndex="1" Margin="20" Padding="50" />
    <Button Grid.Column="1" Content="WPF中文网2" Panel.ZIndex="0" Margin="20" Padding="50" />
</Grid>
```

![[assets/WPF从小白到大佬：布局控件：：Grid控件（网格布局）/63372b852afef62619637c3e10339f3a_MD5.jpg]]

代码分析

我们在Grid控件的ColumnDefinitions属性增加了两个ColumnDefinition对象，如果分别设置了两个按钮的Grid.Column附加属性，指示两个Button分别显示在第一列和第二列，从而实现了左右排列，具两个按钮分别占据了50%的区域。这是因为我们并没有指定两个ColumnDefinition对象的宽度。

**二、上下排列**

```cs
<Grid>
        <Grid.RowDefinitions>
            <RowDefinition/>
            <RowDefinition/>
        </Grid.RowDefinitions>
        <Button Grid.Row="0" Content="WPF中文网1" Panel.ZIndex="1" Margin="20" Padding="50" />
        <Button Grid.Row="1" Content="WPF中文网2" Panel.ZIndex="0" Margin="20" Padding="50" />
    </Grid>
```

![[assets/WPF从小白到大佬：布局控件：：Grid控件（网格布局）/a75fae57320d113eee011df7a133f60e_MD5.jpg]]

要实现上下排列，我们只需要在Grid控件的RowDefinitions中增加两行元素即可，即RowDefinition对象。同时，指定每个Button显示在哪一行，例如Grid.Row="0"，表示显示在第一行。

**三、上下左右排列**

现在我们将左右排列和上下排列两种模式合并起来，看看会发生什么状况。

```cs
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition/>
        <RowDefinition/>
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition/>
        <ColumnDefinition/>
    </Grid.ColumnDefinitions>
    <Button Grid.Row="0" Grid.Column="0" Content="WPF中文网1" Panel.ZIndex="1" Margin="20" />
    <Button Grid.Row="0" Grid.Column="1" Content="WPF中文网2" Panel.ZIndex="0" Margin="20" />
    <Button Grid.Row="1" Grid.Column="0" Content="WPF中文网3" Panel.ZIndex="1" Margin="20" />
    <Button Grid.Row="1" Grid.Column="1" Content="WPF中文网4" Panel.ZIndex="0" Margin="20" />
</Grid>
```

![[assets/WPF从小白到大佬：布局控件：：Grid控件（网格布局）/963e93c6d981229d6ecdd51e1eea8670_MD5.jpg]]

我们创建了4个Button，并分别设置了它们所在的行和列，至此，网格的布局效果跃然纸上。以上就是Grid控件的基本用法。

**深入探究Grid控件**

在实际开发中，我们可能会遇到更复杂的用法，比如第一行只显示一个button，需要跨列显示，或者第一列只占整个Grid的20%的宽度等等，这就需要了解ColumnDefinition和RowDefinition。另外，我们需要显示Grid的网格线，又该如何实现呢？接下来，我们一一去探寻。

**四、跨列排列**

```cs
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition/>
        <RowDefinition/>
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition/>
        <ColumnDefinition/>
    </Grid.ColumnDefinitions>
    <Button Grid.Row="0" Grid.Column="0" Content="WPF中文网1" Panel.ZIndex="1" Margin="20" Grid.ColumnSpan="2"/>
    <Button Grid.Row="1" Grid.Column="0" Content="WPF中文网3" Panel.ZIndex="1" Margin="20" />
    <Button Grid.Row="1" Grid.Column="1" Content="WPF中文网4" Panel.ZIndex="0" Margin="20" />
</Grid>
```

![[assets/WPF从小白到大佬：布局控件：：Grid控件（网格布局）/8b1d4f038c5fa94b42f030bfae9999e0_MD5.jpg]]

我们在原有基础上删掉了一个按钮，并将第一个按钮的Grid.ColumnSpan附加属性设置为2，表示从第0列往右跨两列，正好就呈现出图中的效果。您也可以尝试跨行显示，只需要设置按钮的Grid.RowSpan属性。

**五、固定列宽**

```cs
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition/>
        <RowDefinition/>
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="120"/>
        <ColumnDefinition/>
    </Grid.ColumnDefinitions>
    <Button Grid.Row="0" Grid.Column="0" Content="WPF中文网1" Panel.ZIndex="1" Margin="20" />
    <Button Grid.Row="0" Grid.Column="1" Content="WPF中文网2" Panel.ZIndex="0" Margin="20" />
    <Button Grid.Row="1" Grid.Column="0" Content="WPF中文网3" Panel.ZIndex="1" Margin="20" />
    <Button Grid.Row="1" Grid.Column="1" Content="WPF中文网4" Panel.ZIndex="0" Margin="20" />
</Grid>
```

![[assets/WPF从小白到大佬：布局控件：：Grid控件（网格布局）/71ed34e806ef0454b06905c403347edb_MD5.jpg]]

如图所示，我们只需要设置第一行ColumnDefinition的Width属性，让其宽度固定为120像素，那么第二列的宽度等于Grid的宽度减去120像素，其内部的Button宽度也随之自适应。这就是WPF布局自适应的好处。

**六、调整行高和列宽**

Grid控件的行高和列宽的设置十分丰富，了解它们的用法，有助于设计出更出色的布局。

|   |   |
|---|---|
|名称|说明|
|绝对设置尺寸|使用设备无关单位准确地设置尺寸，就是给一个实际的数字，但通常将此值指定为整数（像素）。如：<ColumnDefinition Width="100"></ColumnDefinition>|
|自动设置尺寸|值为Auto，实际作用就是取实际控件所需的最小值，每行和每列的尺寸刚好满足需要，这是最有用的尺寸设置方式。如：<ColumnDefinition Width="Auto"></ColumnDefinition>|
|按比例设置设置尺寸|按比例将空间分割到一组行和列中。这是对所有行和列的标准设置。通常值为*或N*，实际作用就是取尽可能大的值，当某一列或行被定义为*则是尽可能大，当出现多列或行被定义为*则是代表几者之间按比例方设置尺寸。如：<ColumnDefinition Width="*"></ColumnDefinition>|

> 指定权重，即第2列的宽度是第1列的两倍
` <RowDefinition Height="*"></RowDefinition>  `
` <RowDefinition Height="2*"></RowDefinition>`

**七、Grid显示网格线**

```cs
<Grid ShowGridLines="True" Margin="5">
    <Grid.RowDefinitions>
        <RowDefinition/>
        <RowDefinition/>
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="120"/>
        <ColumnDefinition/>
    </Grid.ColumnDefinitions>
    <Button Grid.Row="0" Grid.Column="0" Content="WPF中文网1" Panel.ZIndex="1" Margin="20" />
    <Button Grid.Row="0" Grid.Column="1" Content="WPF中文网2" Panel.ZIndex="0" Margin="20" />
    <Button Grid.Row="1" Grid.Column="0" Content="WPF中文网3" Panel.ZIndex="1" Margin="20" />
    <Button Grid.Row="1" Grid.Column="1" Content="WPF中文网4" Panel.ZIndex="0" Margin="20" />
</Grid>
```

![[assets/WPF从小白到大佬：布局控件：：Grid控件（网格布局）/ccd400c079ff7282e201322c80b71801_MD5.jpg]]

只需要设置Grid的ShowGridLines=True，就可以显示Grid的网格线，但是这种虚线效果并不友好，我们还有曲线救国的方案。

```cs
<Grid Margin="5">
    <Grid.RowDefinitions>
        <RowDefinition/>
        <RowDefinition/>
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="120"/>
        <ColumnDefinition/>
    </Grid.ColumnDefinitions>
    <Border Grid.Row="0" Grid.RowSpan="2" Grid.Column="0" Grid.ColumnSpan="2" BorderBrush="Gray" BorderThickness="1"/>
    <Border Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2" BorderBrush="Gray" BorderThickness="0 0 0 1"/>
    <Border Grid.Row="0" Grid.RowSpan="2" Grid.Column="0" BorderBrush="Gray" BorderThickness="0 0 1 0"/>
    <Button Grid.Row="0" Grid.Column="0" Content="WPF中文网1" Panel.ZIndex="1" Margin="20" />
    <Button Grid.Row="0" Grid.Column="1" Content="WPF中文网2" Panel.ZIndex="0" Margin="20" />
    <Button Grid.Row="1" Grid.Column="0" Content="WPF中文网3" Panel.ZIndex="1" Margin="20" />
    <Button Grid.Row="1" Grid.Column="1" Content="WPF中文网4" Panel.ZIndex="0" Margin="20" />
</Grid>
```

![[assets/WPF从小白到大佬：布局控件：：Grid控件（网格布局）/4df28f899cd376332159c3f121626150_MD5.jpg]]

我们在Grid内部增加了3个Border，第一个Border用来显示外边框，第二个Border显示中间的横线，第三个Border显示中间的竖线，这时所用的知识点几乎都是Grid的跨行和跨列属性，另外还有边框颜色刷子BorderBrush和边框厚度BorderThickness。

总结

Grid控件绝对是WPF中所有布局控件中最好用的一个，因为它自适应屏幕的宽度，最关键的一点是，它在呈现时，其ActualWidth实际宽度和ActualHeight实际高度会有一个计算值，我们在业务开发中，有时候要根据父控件的实际宽度和高度来计算子控件的呈现位置和大小。

除了Grid这种网格化的布局，下面我们将介绍另一种布局方式——均分布局

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：007-《Grid控件（网格布局）》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年8月17日