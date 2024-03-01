Polygon叫多边形，与Polyline类似，都有一个Points属性，只不过，Polygon会把起点和终点连接起来。就拿上一节的例子，我们只是简单地把Polyline换成Polygon，其它设置保持不变。如下所示：

```cs
<Canvas x:Name="canvas">
    <Polygon StrokeThickness="20"  Points="30,30 200,30 50,250 220,250">
        <Polygon.Stroke>
            <LinearGradientBrush StartPoint="30,30" 
                                 EndPoint="220,250" 
                                 MappingMode="Absolute">
                <GradientStop Color="Red" Offset="1" />
                <GradientStop Color="Yellow" Offset="0.66" />
                <GradientStop Color="Green" Offset="0" />
            </LinearGradientBrush>
        </Polygon.Stroke>
    </Polygon>
</Canvas>
```

![[assets/WPF从小白到大佬：图形控件：：Polygon多边形/e8159c690fd8172dbbe3006a4775f495_MD5.jpg]]

结果，在Polyline下面原本呈现的Z字形，在Polygon下面就变成了一个8字形，可以很明显的看到它的起点和终点相连起来了。

同理，我们在C#后端，也相应的修改成Polygon对象。

```cs
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();            
    }
 
    private int count = 0;
    private Polygon polygon = null;
 
    private void Window_PreviewMouseLeftButtonUp(object sender, MouseButtonEventArgs e)
    {
        if (count++ == 0)
        {
            polygon = new Polygon();
            polygon.StrokeThickness = 5;
            polygon.Stroke = Brushes.Red;
            canvas.Children.Add(polygon);
        }
 
        var point = e.GetPosition(canvas);
        polygon.Points.Add(point);
    }
 
    private void Window_PreviewMouseRightButtonUp(object sender, MouseButtonEventArgs e)
    {
        count = 0;
    }
}
```

![[assets/WPF从小白到大佬：图形控件：：Polygon多边形/a3f9b4b9df445a2c61ca5d595a5fc4d6_MD5.jpg]]

最后，效果如上，用鼠标绘制出来的就是封闭的多边形。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：082-《Polygon多边形》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年10月19日
