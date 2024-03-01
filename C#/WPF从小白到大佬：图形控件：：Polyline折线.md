Polyline表示由一系列线段组合绘制而成的折线，因为它有一个Points属性，用来保存这些点的坐标。这些坐标点用于绘制Polyline图形中各线段相接处的顶点。集合中第一个元素表示起点，最后一元素表示终点。

在XAML前端代码中定义Points的内容书写格式如下：假如我们有4个点，分别是起点(30,30)，中继点(200,30)，中继点(50,250)，终点(220,250)，那么，Points的内容书写为：Points="30,30 200,30 50,250 220,250"

完整代码如下所示

```cs
<Canvas x:Name="canvas">
    <Polyline StrokeThickness="20"  Points="30,30 200,30 50,250 220,250">
        <Polyline.Stroke>
            <LinearGradientBrush StartPoint="30,30" 
                                 EndPoint="220,250" 
                                 MappingMode="Absolute">
                <GradientStop Color="Red" Offset="1" />
                <GradientStop Color="Yellow" Offset="0.66" />
                <GradientStop Color="Green" Offset="0" />
            </LinearGradientBrush>
        </Polyline.Stroke>
    </Polyline>
</Canvas>
```

![[assets/WPF从小白到大佬：图形控件：：Polyline折线/d3ebcbe6b7f71d176d786ea0d59db5b6_MD5.jpg]]

通过上述4个点的连接绘制，我们就绘制了一个"Z"字型的折线图形。

另外，我们也可以通过代码的形式像在画布上作画一样，通过捕获鼠标位置，每次单击就向Polyline中增加一个坐标点，从而达到绘制目的。

首先我们在Window窗体中增加下面两个事件的订阅。

```cs
PreviewMouseLeftButtonUp="Window_PreviewMouseLeftButtonUp"
PreviewMouseRightButtonUp="Window_PreviewMouseRightButtonUp"
```

然后，在第一次按下鼠标左键时，实例化一个Polyline，并在Points中增加当前鼠标的坐标，此后每按下一次鼠标就增加一个坐标点，直到用户单击鼠标右键为止。

```cs
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();            
    }
 
    private int count = 0;
    private Polyline polyline = null;
 
    private void Window_PreviewMouseLeftButtonUp(object sender, MouseButtonEventArgs e)
    {
        if (count++ == 0)
        {
            polyline = new Polyline();
            polyline.StrokeThickness = 5;
            polyline.Stroke = Brushes.Red;
            canvas.Children.Add(polyline);
        }
 
        var point = e.GetPosition(canvas);
        polyline.Points.Add(point);
    }
 
    private void Window_PreviewMouseRightButtonUp(object sender, MouseButtonEventArgs e)
    {
        count = 0;
    }
}
 
 
```

最后，来看看效果

![[assets/WPF从小白到大佬：图形控件：：Polyline折线/fe9947ab617b63ddc26ba608a3bea19f_MD5.jpg]]

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：081-《Polyline折线》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年10月19日

