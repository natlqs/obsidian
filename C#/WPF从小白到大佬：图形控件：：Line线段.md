Line(线段)继承于Shape，它自身只有4个属性，分别用于定义线段两端的端点坐标。

```cs
public sealed class Line : Shape
{
    public static readonly DependencyProperty X1Property;
    public static readonly DependencyProperty Y1Property;
    public static readonly DependencyProperty X2Property;
    public static readonly DependencyProperty Y2Property;
 
    public Line();
 
    public double X1 { get; set; }
    public double Y1 { get; set; }
    public double X2 { get; set; }
    public double Y2 { get; set; }
    protected override Geometry DefiningGeometry { get; }
}
```

其中X1,Y1表示第一个点坐标，X2,Y2表示第二个点坐标。

下面的属性位于Shape基类，在Line线段中设置后，会有意想不到的效果。

StrokeStartLineCap属性：表示线段前头的开关。

Stroke：线条颜色。

StrokeThickness：线条宽度。

StrokeDashArray：设置虚线。

StrokeDashOffset：虚线位置偏移量。

接下来，我们以一个示例来说明Line的用法。

前端代码

```cs

<Window x:Class="HelloWorld.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:HelloWorld"
        mc:Ignorable="d"
        Title="WPF中文网 - wpfsoft.com - Line课程" Height="350" Width="500">
    <Canvas x:Name="canvas">
        <Line x:Name="flowLine" 
              X1="20" 
              Y1="40" 
              X2="400" 
              Y2="100" 
              StrokeDashArray="2,1" 
              Stroke="Green" 
              StrokeThickness="8"/>
        <Line X1="{Binding ElementName=x1,Path=Value}"
              Y1="{Binding ElementName=y1,Path=Value}"
              X2="{Binding ElementName=x2,Path=Value}"
              Y2="{Binding ElementName=y2,Path=Value}"
              StrokeStartLineCap="Round"
              Stroke="Red"
              StrokeThickness="5"/>
        <Slider x:Name="x1" 
                Value="10" 
                Maximum="450" 
                Width="450" 
                Canvas.Left="10" 
                Canvas.Top="237"/>
        <Slider x:Name="y1" 
                Value="10" 
                Maximum="450" 
                Width="450" 
                Canvas.Left="10" 
                Canvas.Top="256"/>
        <Slider x:Name="x2" 
                Value="300" 
                Maximum="450" 
                Width="450" 
                Canvas.Left="10" 
                Canvas.Top="276"/>
        <Slider x:Name="y2" 
                Value="300" 
                Maximum="450" 
                Width="450" 
                Canvas.Left="10" 
                Canvas.Top="295"/>
    </Canvas>
</Window>
```

后端代码

```cs
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
 
        Loaded += (s, e) =>
        {
            int number = 10;
            Task.Run(() =>
            {
                while (true)
                {
                    if (number == 1)
                        number = 10;
                    Application.Current.Dispatcher.BeginInvoke(new Action(() =>
                    {
                        flowLine.StrokeDashOffset = number;
                    }));
                    number--;
                    Thread.Sleep(250);
                }
            });
        };
    }
}
```

在XAML代码中，我们实例化了两个Line，其中一个Line的端点坐标绑定了4个Slider的Value属性，可以通过滑动改变线条的位置，第二Line设置为虚线，并通过C#代码开辟子线程，在子线程中通过动态设置StrokeDashOffset属性，来模拟流动的线段效果。

![[assets/WPF从小白到大佬：图形控件：：Line线段/436ccfcf2983cef246180bc83e36f55c_MD5.jpg]]

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**  
> 文件名：079-《Line线段》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年10月17日