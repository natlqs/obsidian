Rectangle是一个比较简单而实用的图形控件，继承于Shape，有两个属性比较常用，即RadiusX和RadiusY，表示设置矩形的圆角。所以，通过这两个属性的设置，矩形也可以画出一个圆。

观察下面的例子。

```cs
<Window x:Class="HelloWorld.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:HelloWorld"
        mc:Ignorable="d"
        Title="WPF中文网 - wpfsoft.com -课程" Height="350" Width="500">
    <Canvas x:Name="canvas">
        <Rectangle RadiusX="{Binding ElementName=slider,Path=Value}" 
                   RadiusY="{Binding ElementName=slider,Path=Value}" 
                   Width="{Binding ElementName=slider,Path=Value}" 
                   Height="{Binding ElementName=slider,Path=Value}" 
                   Fill="Red" 
                   Canvas.Left="50" 
                   Canvas.Top="36"/>
 
        <Rectangle Width="100" 
                   Height="100" 
                   Fill="Green" 
                   Canvas.Left="313" 
                   Canvas.Top="36"/>
 
        <Slider x:Name="slider" 
                Width="450" 
                Value="100" 
                Maximum="450" 
                Canvas.Left="32" 
                Canvas.Top="291"/>
    </Canvas>
</Window>
```

![[assets/WPF从小白到大佬：图形控件：：Rectangle矩形/f9d9d1267ecb3c616579f09d568e2395_MD5.jpg]]

在本例中，我们实例化了两个Rectangle对象，如果RadiusX和RadiusY与Width、Height相等，则会显示一个正圆。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：080-《Rectangle矩形》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年10月18日e