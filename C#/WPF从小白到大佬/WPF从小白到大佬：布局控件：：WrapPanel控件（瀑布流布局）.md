WrapPanel控件表示将其子控件从左到右的顺序排列，如果第一行显示不了，则自动换至第二行，继续显示剩余的子控件。我们来看看它的结构定义：

```cs
public class WrapPanel : Panel
{
    public static readonly DependencyProperty ItemWidthProperty;
    public static readonly DependencyProperty ItemHeightProperty;
    public static readonly DependencyProperty OrientationProperty;
 
    public WrapPanel();
 
    public double ItemWidth { get; set; }
    public double ItemHeight { get; set; }
    public Orientation Orientation { get; set; }
 
    protected override Size ArrangeOverride(Size finalSize);
    protected override Size MeasureOverride(Size constraint);
 
}
```

这个控件比较简单，只提供了3个属性，分别是Orientation代表子控件的排列方向，ItemWidth代表子控件的（最大）宽度，ItemHeight代表子控件的（最大）高度。默认的排列方向是水平方向。

**一、水平排列**

```cs
<WrapPanel Orientation="Horizontal">
    <Button Content="WPF中文网1" Margin="5" HorizontalAlignment="Left"/>
    <Button Content="WPF中文网2" Margin="5" HorizontalAlignment="Right"/>
    <Button Content="WPF中文网3" Margin="5" HorizontalAlignment="Center"/>
    <Button Content="WPF中文网4" Margin="5" HorizontalAlignment="Stretch"/>
    <Button Content="WPF中文网5" Margin="5" />
    <Button Content="WPF中文网6" Margin="5" />
    <Button Content="WPF中文网7" Margin="5" />
    <Button Content="WPF中文网8" Margin="5" />
    <Button Content="WPF中文网9" Margin="5" />
    <Button Content="WPF中文网10" Margin="5" />
</WrapPanel>
```

![[assets/WPF从小白到大佬：布局控件：：WrapPanel控件（瀑布流布局）/51b86f500a8524f758a9d09ea01302a7_MD5.jpg]]

由上图所示，我们在WrapPanel的Children属性中放了10个Button，分成了两行显示，请注意一个细节，**WrapPanel的子元素的高度和宽度都是根据子元素自身内容的尺寸呈现**。另外，当WrapPanel处于水平排列时，子元素的HorizontalAlignment是不起作用的。

**二、垂直排列**

```cs
<WrapPanel Orientation="Vertical">
    <Button Content="WPF中文网1" Margin="5" HorizontalAlignment="Left"/>
    <Button Content="WPF中文网2" Margin="5" HorizontalAlignment="Right"/>
    <Button Content="WPF中文网3" Margin="5" HorizontalAlignment="Center"/>
    <Button Content="WPF中文网4" Margin="5" HorizontalAlignment="Stretch"/>
    <Button Content="WPF中文网5" Margin="5" />
    <Button Content="WPF中文网6" Margin="5" />
    <Button Content="WPF中文网7" Margin="5" />
    <Button Content="WPF中文网8" Margin="5" />
    <Button Content="WPF中文网9" Margin="5" />
    <Button Content="WPF中文网10" Margin="5" />
    <Button Content="WPF中文网12" Margin="5" />
    <Button Content="WPF中文网13" Margin="5" />
    <Button Content="WPF中文网14" Margin="5" />
    <Button Content="WPF中文网15" Margin="5" />
    <Button Content="WPF中文网16" Margin="5" />
    <Button Content="WPF中文网17" Margin="5" />
    <Button Content="WPF中文网18" Margin="5" />
    <Button Content="WPF中文网19" Margin="5" />
    <Button Content="WPF中文网20" Margin="5" />
 
</WrapPanel>
```

![[assets/WPF从小白到大佬：布局控件：：WrapPanel控件（瀑布流布局）/056445daff71bbdda4196d9ce89f6158_MD5.jpg]]

这里我们放了20个button在WrapPanel控件中，并设置Orientation属性为Vertical（垂直排列），此时，请观察前面3个按钮的HorizontalAlignment状态，可以很清晰的看到，第一个按钮居左显示，第二个按钮居右显示，第三个按钮居中显示，说明在Vertical垂直排列下，子元素的水平状态才会生效，反之亦然。

三、指定子元素宽高

```cs
<WrapPanel Orientation="Horizontal" ItemWidth="80" ItemHeight="80">
    <Button Content="WPF中文网1" Margin="5" HorizontalAlignment="Left"/>
    <Button Content="WPF中文网2" Margin="5" HorizontalAlignment="Right"/>
    <Button Content="WPF中文网3" Margin="5" HorizontalAlignment="Center"/>
    <Button Content="WPF中文网4" Margin="5" HorizontalAlignment="Stretch"/>
    <Button Content="WPF中文网5" Margin="5" />
    <Button Content="WPF中文网6" Margin="5" />
    <Button Content="WPF中文网7" Margin="5" />
    <Button Content="WPF中文网8" Margin="5" />
    <Button Content="WPF中文网9" Margin="5" />
    <Button Content="WPF中文网10" Margin="5" />
</WrapPanel>
```

![[assets/WPF从小白到大佬：布局控件：：WrapPanel控件（瀑布流布局）/b6192575176011ac1c9a800a7f86b3e0_MD5.jpg]]

由此可以，我们也可以指定子元素的宽度和高度，以便统一观察。下一节，我们将探讨与WrapPanel非常相似的布局控件DockPanel。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：009-《WrapPanel控件（栈式布局）》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年8月18日