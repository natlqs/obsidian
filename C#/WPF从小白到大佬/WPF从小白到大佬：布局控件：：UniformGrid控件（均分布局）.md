UniformGrid和Grid有些相似，只不过UniformGrid的每个单元格面积都是相等的，不管是横向的单元格，或是纵向的单元格，它们会平分整个UniformGrid。我们先看看它的结构定义：
```cs
public class UniformGrid : Panel
{
    public static readonly DependencyProperty FirstColumnProperty;
    public static readonly DependencyProperty ColumnsProperty;
    public static readonly DependencyProperty RowsProperty;
 
    public UniformGrid();
 
    public int FirstColumn { get; set; }
    public int Columns { get; set; }
    public int Rows { get; set; }
 
    protected override Size ArrangeOverride(Size arrangeSize);
    protected override Size MeasureOverride(Size constraint);
 
}
```

UniformGrid控件提供了3个属性，分别是FirstColumn、Columns 、Rows 。FirstColumn表示第一行要空几个单元格，后面两个属性分别用于设置行数和列数。接下来我们以实际的例子来分析这3个属性的用法。
```cs
<UniformGrid>
    <Button Content="WPF中文网1" Margin="2"/>
    <Button Content="WPF中文网2" Margin="2"/>
    <Button Content="WPF中文网3" Margin="2"/>
    <Button Content="WPF中文网4" Margin="2"/>
</UniformGrid>
```

![[assets/WPF从小白到大佬：布局控件：：UniformGrid控件（均分布局）/0eb56f3f3936c12fece29bd3f03d2145_MD5.jpg]]

这是我们没有UniformGrid的属性的效果，它会根据子元素的数量和UniformGrid自身的尺寸来决定行数和列数。

```cs
<UniformGrid FirstColumn="1" Rows="3" Columns="3">
    <Button Content="WPF中文网1" Margin="2"/>
    <Button Content="WPF中文网2" Margin="2"/>
    <Button Content="WPF中文网3" Margin="2"/>
    <Button Content="WPF中文网4" Margin="2"/>
</UniformGrid>
```

![[assets/WPF从小白到大佬：布局控件：：UniformGrid控件（均分布局）/ec090960656168bddfd8a33515b5b76c_MD5.jpg]]

我们故意设计了当前UniformGrid为3行3列，同时设置第一行第一个单元格保持空白，于是我们就看到了上图中的效果。

UniformGrid控件使用非常简单方便，通常用于局部的布局。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：011-《UniformGrid控件（均分布局）》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年8月19日














