TextBlock是专业处理文本显示的控件，在功能上比Label更全面。先看结构定义，后举例说明。

```cs
public class TextBlock : FrameworkElement, IContentHost, IAddChildInternal, IAddChild, IServiceProvider
{
    public static readonly DependencyProperty BaselineOffsetProperty;
    public static readonly DependencyProperty IsHyphenationEnabledProperty;
    public static readonly DependencyProperty TextWrappingProperty;
    public static readonly DependencyProperty TextAlignmentProperty;
    public static readonly DependencyProperty PaddingProperty;
    public static readonly DependencyProperty LineStackingStrategyProperty;
    public static readonly DependencyProperty LineHeightProperty;
    public static readonly DependencyProperty TextEffectsProperty;
    public static readonly DependencyProperty TextDecorationsProperty;
    public static readonly DependencyProperty TextTrimmingProperty;
    public static readonly DependencyProperty ForegroundProperty;
    public static readonly DependencyProperty FontSizeProperty;
    public static readonly DependencyProperty FontStretchProperty;
    public static readonly DependencyProperty FontWeightProperty;
    public static readonly DependencyProperty FontStyleProperty;
    public static readonly DependencyProperty FontFamilyProperty;
    public static readonly DependencyProperty TextProperty;
    public static readonly DependencyProperty BackgroundProperty;
 
    public TextBlock();
    public TextBlock(Inline inline);
 
    public FontWeight FontWeight { get; set; }
    public FontStyle FontStyle { get; set; }
    public FontFamily FontFamily { get; set; }
    public string Text { get; set; }
    public TextPointer ContentEnd { get; }
    public Typography Typography { get; }
    public LineBreakCondition BreakAfter { get; }
    public LineBreakCondition BreakBefore { get; }
    public FontStretch FontStretch { get; set; }
    public double BaselineOffset { get; set; }
    public double FontSize { get; set; }
    public TextWrapping TextWrapping { get; set; }
    public Brush Background { get; set; }
    public TextDecorationCollection TextDecorations { get; set; }
    public TextEffectCollection TextEffects { get; set; }
    public double LineHeight { get; set; }
    public LineStackingStrategy LineStackingStrategy { get; set; }
    public Thickness Padding { get; set; }
    public TextAlignment TextAlignment { get; set; }
    public TextTrimming TextTrimming { get; set; }
    public TextPointer ContentStart { get; }
    public bool IsHyphenationEnabled { get; set; }
    public Brush Foreground { get; set; }
    public InlineCollection Inlines { get; }
    protected virtual IEnumerator<IInputElement> HostedElementsCore { get; }
    protected override int VisualChildrenCount { get; }
    protected internal override IEnumerator LogicalChildren { get; }
 
    public static double GetBaselineOffset(DependencyObject element);
    public static FontFamily GetFontFamily(DependencyObject element);
    public static double GetFontSize(DependencyObject element);
    public static FontStretch GetFontStretch(DependencyObject element);
    public static FontStyle GetFontStyle(DependencyObject element);
    public static FontWeight GetFontWeight(DependencyObject element);
    public static Brush GetForeground(DependencyObject element);
    public static double GetLineHeight(DependencyObject element);
    public static LineStackingStrategy GetLineStackingStrategy(DependencyObject element);
    public static TextAlignment GetTextAlignment(DependencyObject element);
    public static void SetBaselineOffset(DependencyObject element, double value);
    public static void SetFontFamily(DependencyObject element, FontFamily value);
    public static void SetFontSize(DependencyObject element, double value);
    public static void SetFontStretch(DependencyObject element, FontStretch value);
    public static void SetFontStyle(DependencyObject element, FontStyle value);
    public static void SetFontWeight(DependencyObject element, FontWeight value);
    public static void SetForeground(DependencyObject element, Brush value);
    public static void SetLineHeight(DependencyObject element, double value);
    public static void SetLineStackingStrategy(DependencyObject element, LineStackingStrategy value);
    public static void SetTextAlignment(DependencyObject element, TextAlignment value);
    public TextPointer GetPositionFromPoint(Point point, bool snapToText);
    public bool ShouldSerializeBaselineOffset();
    public bool ShouldSerializeInlines(XamlDesignerSerializationManager manager);
    public bool ShouldSerializeText();
    protected sealed override Size ArrangeOverride(Size arrangeSize);
    protected virtual ReadOnlyCollection<Rect> GetRectanglesCore(ContentElement child);
    protected override Visual GetVisualChild(int index);
    protected sealed override HitTestResult HitTestCore(PointHitTestParameters hitTestParameters);
    protected virtual IInputElement InputHitTestCore(Point point);
    protected sealed override Size MeasureOverride(Size constraint);
    protected virtual void OnChildDesiredSizeChangedCore(UIElement child);
    protected override AutomationPeer OnCreateAutomationPeer();
    protected sealed override void OnPropertyChanged(DependencyPropertyChangedEventArgs e);
    protected sealed override void OnRender(DrawingContext ctx);
 
}
```

TextBlock提供了非常丰富的文本相关的属性。

|                |                                                                                                                                  |
| -------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| 属性             | 说明                                                                                                                               |
| FontWeight     | 获取或设置TextBlock的字体粗细                                                                                                              |
| FontStyle      | 获取或设置TextBlock的字体样式，如斜体字体                                                                                                        |
| FontFamily     | 获取或设置TextBlock的字体系列，如微软雅黑                                                                                                        |
| Text           | 获取或设置TextBlock的字体内容。                                                                                                             |
| ContentEnd     | 表示获取TextBlock内容的最末尾的TextPointer对象                                                                                                |
| Typography     | 获取此元素的内容当前有效的版式变体。                                                                                                               |
| FontStretch    | 获取或设置 TextBlock 的常用字体拉伸特征。                                                                                                       |
| BaselineOffset | 获取或设置文本的每个行相对于基线的偏移量。                                                                                                            |
| FontSize       | 获取或设置TextBlock的字号                                                                                                                |
| TextWrapping   | 获取或设置TextBlock的文字的换行方式                                                                                                           |
| Background     | 获取或设置TextBlock控件的背景颜色（画刷）                                                                                                        |
| TextEffects    | 获取或设置要应用于此元素中的文本内容的效果。                                                                                                           |
| LineHeight     | 获取或设置各行内容的高度。                                                                                                                    |
| Padding        | 指示内容区域的边界之间填充空间的宽度                                                                                                               |
| TextAlignment  | 指示文本内容的水平对齐方式。                                                                                                                   |
| TextTrimming   | 获取或设置在内容超出内容区域时要采用的文本剪裁行为。                                                                                                       |
| Foreground     | 获取或设置文本内容的字体颜色（画刷）                                                                                                               |
| Inlines        | 这个属性是一个集合，其中的元素表示内联流内容元素，简单点说，一行文本可以看成是一个Inline元素，而TextBlock可以接受多个Inline。Run继承于Inline，实际使用中，我们会创建多个Run实例，可以单独为每个Run对象设置字体字号颜色等等。 |
| ContentStart   | 表示获取TextBlock内容的最开始的TextPointer对象                                                                                                |

接下来， 我们将上面常用的属性在例子中得以体现。

```cs
<Window x:Class="HelloWorld.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:HelloWorld"
        mc:Ignorable="d" FontSize="16"
        Title="HelloWorld - www.wpfsoft.com" Height="350" Width="500">
    <WrapPanel>
        <TextBlock Text="这是一个TextBlock文字块" Margin="5"/>
        <TextBlock Text="粗体文字" FontWeight="Bold" Margin="5"/>
        <TextBlock Text="粗体文字" FontWeight="Light" Margin="5"/>
        <TextBlock Text="斜体文字" FontStyle="Italic"  Margin="5"/>
        <TextBlock Text="微软雅黑" FontFamily="Microsoft YaHei UI"  Margin="5"/>
        <TextBlock Text="大号字体" FontSize="30"  Margin="5"/>
        <TextBlock Text="红色文字" Foreground="Red" Margin="5"/>
        <TextBlock Text="底色文字" Foreground="Yellow" Background="Red" Margin="5"/>
        <TextBlock Text="内间距文字" Foreground="Yellow" Background="Red" Padding="10" Margin="5"/>
        <TextBlock Background="LightGray" Height="25">
            <Run Foreground="Red">这行文字</Run>
            <Run Foreground="Green">由三部分</Run>
            <Run Foreground="Blue">组成</Run>
        </TextBlock>
        <Grid Width="150" Height="100" Margin="5" Background="LightGoldenrodYellow">
            <TextBlock Text="这段文字体现了文字的文本换行属性TextWrapping" TextWrapping="Wrap" Margin="10"/>
        </Grid>
        <!--使用Run-->
        <Grid>
            <TextBlock x:Name="textblock"  
                       Width="320" 
                       Height="100" 
                       FontSize="15" 
                       FontFamily="微软雅黑" 
                       FontWeight="Black" 
                       FontStretch="Condensed" 
                       Foreground="#dddddd" 
                       Background="Teal" 
                       TextAlignment="Center" 
                       TextWrapping="Wrap" 
                       TextTrimming="CharacterEllipsis" 
                       Margin="10" Padding="5"
                       HorizontalAlignment="Left" 
                       VerticalAlignment="Center" 
                       LineHeight="30" 
                       ToolTip="《临江仙·滚滚长江东逝水》">
		<Run Foreground="#CDB632" TextDecorations="Underline">
			滚滚长江东逝水，浪花淘尽英雄。是非成败转头空。青山依旧在，几度夕阳红。
		</Run>
		<Run Text="白发渔樵江渚上，惯看秋月春风。一壶浊酒喜相逢。古今多少事，都付笑谈中。 ">
		</Run>
            </TextBlock>
        </Grid>
    </WrapPanel>
</Window>
```

[[assets/WPF从小白到大佬：内容控件：：TextBlock文字块/60a8e203b7662c1333e991323f81570c_MD5.jpeg|Open: Pasted image 20240227152138.png]]
![[assets/WPF从小白到大佬：内容控件：：TextBlock文字块/60a8e203b7662c1333e991323f81570c_MD5.jpeg]]

TextBlock大多数的属性应用都比较简单，容易理解。Inlines属性是一个比较强大的属性，深入理解后，可以实现意想不到的效果。另外，TextEffects也是一个非常强大的属性，这需要掌握WPF的动画、触发器、关键帧等知识，才能实现文本的动画特效。我们将在学完动画后，再回头探讨这些内容。

与文本相关的还有两个输入控件，即TextBox和PasswordBox。下一节，我们来探讨TextBox。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：022-《TextBlock文字块》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff