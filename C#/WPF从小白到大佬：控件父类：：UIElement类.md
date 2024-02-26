UIElement类继承了Visual类，在WPF框架中排行老四（第4个基类）。它位于程序集:PresentationCore.dll之中，命名空间:System.Windows。

这个基类非常非常重要，理解了这个类，就理解了WPF所有控件1/3的知识与用法。我们先来看一下它的全貌。

```cs
public class UIElement : Visual, IAnimatable, IInputElement
{
public static readonly RoutedEvent PreviewMouseDownEvent;
public static readonly DependencyProperty AreAnyTouchesOverProperty;
public static readonly DependencyProperty AreAnyTouchesDirectlyOverProperty;
public static readonly DependencyProperty IsKeyboardFocusedProperty;
public static readonly DependencyProperty IsStylusCaptureWithinProperty;
public static readonly DependencyProperty IsStylusCapturedProperty;
public static readonly DependencyProperty IsMouseCaptureWithinProperty;
public static readonly DependencyProperty IsMouseCapturedProperty;
public static readonly DependencyProperty IsKeyboardFocusWithinProperty;
public static readonly DependencyProperty IsStylusOverProperty;
public static readonly DependencyProperty IsMouseOverProperty;
public static readonly DependencyProperty IsMouseDirectlyOverProperty;
public static readonly RoutedEvent TouchLeaveEvent;
public static readonly RoutedEvent TouchEnterEvent;
public static readonly RoutedEvent LostTouchCaptureEvent;
public static readonly RoutedEvent GotTouchCaptureEvent;
public static readonly RoutedEvent TouchUpEvent;
public static readonly RoutedEvent PreviewTouchUpEvent;
public static readonly RoutedEvent TouchMoveEvent;
public static readonly RoutedEvent PreviewTouchMoveEvent;
public static readonly RoutedEvent TouchDownEvent;
public static readonly RoutedEvent PreviewTouchDownEvent;
public static readonly RoutedEvent DropEvent;
public static readonly RoutedEvent PreviewDropEvent;
public static readonly RoutedEvent DragLeaveEvent;
public static readonly RoutedEvent PreviewDragLeaveEvent;
public static readonly DependencyProperty AreAnyTouchesCapturedProperty;
public static readonly DependencyProperty AreAnyTouchesCapturedWithinProperty;
public static readonly DependencyProperty AllowDropProperty;
public static readonly DependencyProperty RenderTransformProperty;
public static readonly RoutedEvent ManipulationCompletedEvent;
public static readonly RoutedEvent ManipulationBoundaryFeedbackEvent;
public static readonly RoutedEvent ManipulationInertiaStartingEvent;
public static readonly RoutedEvent ManipulationDeltaEvent;
public static readonly RoutedEvent ManipulationStartedEvent;
public static readonly RoutedEvent ManipulationStartingEvent;
public static readonly DependencyProperty IsManipulationEnabledProperty;
public static readonly DependencyProperty FocusableProperty;
public static readonly DependencyProperty IsVisibleProperty;
public static readonly DependencyProperty IsHitTestVisibleProperty;
public static readonly DependencyProperty IsEnabledProperty;
public static readonly DependencyProperty IsFocusedProperty;
public static readonly RoutedEvent DragOverEvent;
public static readonly RoutedEvent LostFocusEvent;
public static readonly DependencyProperty SnapsToDevicePixelsProperty;
public static readonly DependencyProperty ClipProperty;
public static readonly DependencyProperty ClipToBoundsProperty;
public static readonly DependencyProperty VisibilityProperty;
public static readonly DependencyProperty UidProperty;
public static readonly DependencyProperty CacheModeProperty;
public static readonly DependencyProperty BitmapEffectInputProperty;
public static readonly DependencyProperty EffectProperty;
public static readonly DependencyProperty BitmapEffectProperty;
public static readonly DependencyProperty OpacityMaskProperty;
public static readonly DependencyProperty OpacityProperty;
public static readonly DependencyProperty RenderTransformOriginProperty;
public static readonly RoutedEvent GotFocusEvent;
public static readonly RoutedEvent PreviewDragOverEvent;
public static readonly DependencyProperty IsStylusDirectlyOverProperty;
public static readonly RoutedEvent PreviewDragEnterEvent;
public static readonly RoutedEvent StylusMoveEvent;
public static readonly RoutedEvent PreviewStylusMoveEvent;
public static readonly RoutedEvent StylusUpEvent;
public static readonly RoutedEvent PreviewStylusUpEvent;
public static readonly RoutedEvent StylusDownEvent;
public static readonly RoutedEvent PreviewStylusDownEvent;
public static readonly RoutedEvent QueryCursorEvent;
public static readonly RoutedEvent LostMouseCaptureEvent;
public static readonly RoutedEvent GotMouseCaptureEvent;
public static readonly RoutedEvent MouseLeaveEvent;
public static readonly RoutedEvent MouseEnterEvent;
public static readonly RoutedEvent MouseWheelEvent;
public static readonly RoutedEvent PreviewStylusInAirMoveEvent;
public static readonly RoutedEvent PreviewMouseWheelEvent;
public static readonly RoutedEvent PreviewMouseMoveEvent;
public static readonly RoutedEvent MouseRightButtonUpEvent;
public static readonly RoutedEvent PreviewMouseRightButtonUpEvent;
public static readonly RoutedEvent MouseRightButtonDownEvent;
public static readonly RoutedEvent PreviewMouseRightButtonDownEvent;
public static readonly RoutedEvent DragEnterEvent;
public static readonly RoutedEvent PreviewMouseLeftButtonUpEvent;
public static readonly RoutedEvent MouseLeftButtonDownEvent;
public static readonly RoutedEvent PreviewMouseLeftButtonDownEvent;
public static readonly RoutedEvent MouseUpEvent;
public static readonly RoutedEvent PreviewMouseUpEvent;
public static readonly RoutedEvent MouseDownEvent;
public static readonly RoutedEvent MouseMoveEvent;
public static readonly RoutedEvent StylusInAirMoveEvent;
public static readonly RoutedEvent MouseLeftButtonUpEvent;
public static readonly RoutedEvent StylusLeaveEvent;
public static readonly RoutedEvent StylusEnterEvent;
public static readonly RoutedEvent GiveFeedbackEvent;
public static readonly RoutedEvent PreviewGiveFeedbackEvent;
public static readonly RoutedEvent QueryContinueDragEvent;
public static readonly RoutedEvent TextInputEvent;
public static readonly RoutedEvent PreviewTextInputEvent;
public static readonly RoutedEvent LostKeyboardFocusEvent;
public static readonly RoutedEvent PreviewLostKeyboardFocusEvent;
public static readonly RoutedEvent GotKeyboardFocusEvent;
public static readonly RoutedEvent PreviewGotKeyboardFocusEvent;
public static readonly RoutedEvent KeyUpEvent;
public static readonly RoutedEvent PreviewKeyUpEvent;
public static readonly RoutedEvent KeyDownEvent;
public static readonly RoutedEvent PreviewQueryContinueDragEvent;
public static readonly RoutedEvent PreviewStylusButtonUpEvent;
public static readonly RoutedEvent PreviewKeyDownEvent;
public static readonly RoutedEvent StylusInRangeEvent;
public static readonly RoutedEvent PreviewStylusInRangeEvent;
public static readonly RoutedEvent StylusOutOfRangeEvent;
public static readonly RoutedEvent PreviewStylusSystemGestureEvent;
public static readonly RoutedEvent PreviewStylusOutOfRangeEvent;
public static readonly RoutedEvent GotStylusCaptureEvent;
public static readonly RoutedEvent LostStylusCaptureEvent;
public static readonly RoutedEvent StylusButtonDownEvent;
public static readonly RoutedEvent StylusButtonUpEvent;
public static readonly RoutedEvent PreviewStylusButtonDownEvent;
public static readonly RoutedEvent StylusSystemGestureEvent;
 
public UIElement();
 
public string Uid { get; set; }
public Visibility Visibility { get; set; }
public bool ClipToBounds { get; set; }
public Geometry Clip { get; set; }
public bool SnapsToDevicePixels { get; set; }
public bool IsFocused { get; }
public bool IsEnabled { get; set; }
public bool IsHitTestVisible { get; set; }
public bool IsVisible { get; }
public bool AreAnyTouchesCapturedWithin { get; }
public int PersistId { get; }
public bool IsManipulationEnabled { get; set; }
public bool AreAnyTouchesOver { get; }
public bool AreAnyTouchesDirectlyOver { get; }
public bool AreAnyTouchesCaptured { get; }
public IEnumerable<TouchDevice> TouchesCaptured { get; }
public IEnumerable<TouchDevice> TouchesCapturedWithin { get; }
public IEnumerable<TouchDevice> TouchesOver { get; }
public CacheMode CacheMode { get; set; }
public bool Focusable { get; set; }
public BitmapEffectInput BitmapEffectInput { get; set; }
public bool IsMouseDirectlyOver { get; }
public BitmapEffect BitmapEffect { get; set; }
public Size RenderSize { get; set; }
public bool IsArrangeValid { get; }
public bool IsMeasureValid { get; }
public Size DesiredSize { get; }
public bool AllowDrop { get; set; }
public CommandBindingCollection CommandBindings { get; }
public InputBindingCollection InputBindings { get; }
public bool HasAnimatedProperties { get; }
public bool IsMouseOver { get; }
public Effect Effect { get; set; }
public bool IsStylusOver { get; }
public bool IsMouseCaptured { get; }
public bool IsMouseCaptureWithin { get; }
public bool IsStylusDirectlyOver { get; }
public bool IsStylusCaptured { get; }
public bool IsStylusCaptureWithin { get; }
public bool IsKeyboardFocused { get; }
public bool IsInputMethodEnabled { get; }
public double Opacity { get; set; }
public Brush OpacityMask { get; set; }
public bool IsKeyboardFocusWithin { get; }
public IEnumerable<TouchDevice> TouchesDirectlyOver { get; }
public Point RenderTransformOrigin { get; set; }
public Transform RenderTransform { get; set; }
protected StylusPlugInCollection StylusPlugIns { get; }
protected virtual bool IsEnabledCore { get; }
protected internal virtual bool HasEffectiveKeyboardFocus { get; }
 
public event KeyEventHandler KeyUp;
public event EventHandler<TouchEventArgs> TouchMove;
public event EventHandler<TouchEventArgs> PreviewTouchMove;
public event EventHandler<TouchEventArgs> TouchDown;
public event EventHandler<TouchEventArgs> PreviewTouchDown;
public event DragEventHandler Drop;
public event DragEventHandler PreviewDrop;
public event DragEventHandler DragLeave;
public event DragEventHandler PreviewDragLeave;
public event DragEventHandler DragOver;
public event DragEventHandler PreviewDragOver;
public event DragEventHandler DragEnter;
public event DragEventHandler PreviewDragEnter;
public event GiveFeedbackEventHandler GiveFeedback;
public event GiveFeedbackEventHandler PreviewGiveFeedback;
public event QueryContinueDragEventHandler QueryContinueDrag;
public event QueryContinueDragEventHandler PreviewQueryContinueDrag;
public event TextCompositionEventHandler TextInput;
public event EventHandler<TouchEventArgs> PreviewTouchUp;
public event EventHandler<TouchEventArgs> TouchUp;
public event EventHandler<TouchEventArgs> LostTouchCapture;
public event TextCompositionEventHandler PreviewTextInput;
public event EventHandler<ManipulationInertiaStartingEventArgs> ManipulationInertiaStarting;
public event EventHandler<ManipulationDeltaEventArgs> ManipulationDelta;
public event EventHandler<ManipulationStartedEventArgs> ManipulationStarted;
public event EventHandler<ManipulationStartingEventArgs> ManipulationStarting;
public event DependencyPropertyChangedEventHandler FocusableChanged;
public event DependencyPropertyChangedEventHandler IsVisibleChanged;
public event DependencyPropertyChangedEventHandler IsHitTestVisibleChanged;
public event DependencyPropertyChangedEventHandler IsEnabledChanged;
public event RoutedEventHandler LostFocus;
public event EventHandler<TouchEventArgs> GotTouchCapture;
public event RoutedEventHandler GotFocus;
public event DependencyPropertyChangedEventHandler IsKeyboardFocusedChanged;
public event DependencyPropertyChangedEventHandler IsStylusCaptureWithinChanged;
public event DependencyPropertyChangedEventHandler IsStylusDirectlyOverChanged;
public event DependencyPropertyChangedEventHandler IsMouseCaptureWithinChanged;
public event DependencyPropertyChangedEventHandler IsMouseCapturedChanged;
public event DependencyPropertyChangedEventHandler IsKeyboardFocusWithinChanged;
public event DependencyPropertyChangedEventHandler IsMouseDirectlyOverChanged;
public event EventHandler<TouchEventArgs> TouchLeave;
public event EventHandler<TouchEventArgs> TouchEnter;
public event EventHandler LayoutUpdated;
public event KeyboardFocusChangedEventHandler LostKeyboardFocus;
public event KeyboardFocusChangedEventHandler PreviewLostKeyboardFocus;
public event KeyboardFocusChangedEventHandler GotKeyboardFocus;
public event StylusEventHandler PreviewStylusMove;
public event StylusEventHandler StylusMove;
public event StylusEventHandler PreviewStylusInAirMove;
public event StylusEventHandler StylusInAirMove;
public event StylusEventHandler StylusEnter;
public event StylusEventHandler StylusLeave;
public event StylusEventHandler PreviewStylusInRange;
public event StylusEventHandler StylusInRange;
public event StylusEventHandler PreviewStylusOutOfRange;
public event StylusEventHandler StylusOutOfRange;
public event StylusSystemGestureEventHandler PreviewStylusSystemGesture;
public event StylusSystemGestureEventHandler StylusSystemGesture;
public event StylusEventHandler GotStylusCapture;
public event StylusEventHandler LostStylusCapture;
public event StylusButtonEventHandler StylusButtonDown;
public event StylusButtonEventHandler StylusButtonUp;
public event StylusButtonEventHandler PreviewStylusButtonDown;
public event StylusButtonEventHandler PreviewStylusButtonUp;
public event KeyEventHandler PreviewKeyDown;
public event KeyEventHandler KeyDown;
public event KeyEventHandler PreviewKeyUp;
public event StylusEventHandler StylusUp;
public event KeyboardFocusChangedEventHandler PreviewGotKeyboardFocus;
public event StylusEventHandler PreviewStylusUp;
public event StylusDownEventHandler PreviewStylusDown;
public event MouseButtonEventHandler PreviewMouseDown;
public event MouseButtonEventHandler MouseDown;
public event MouseButtonEventHandler PreviewMouseUp;
public event MouseButtonEventHandler MouseUp;
public event MouseButtonEventHandler PreviewMouseLeftButtonDown;
public event MouseButtonEventHandler MouseLeftButtonDown;
public event MouseButtonEventHandler PreviewMouseLeftButtonUp;
public event MouseButtonEventHandler MouseLeftButtonUp;
public event MouseButtonEventHandler PreviewMouseRightButtonDown;
public event MouseButtonEventHandler MouseRightButtonDown;
public event MouseButtonEventHandler PreviewMouseRightButtonUp;
public event MouseButtonEventHandler MouseRightButtonUp;
public event MouseEventHandler PreviewMouseMove;
public event MouseEventHandler MouseMove;
public event MouseWheelEventHandler PreviewMouseWheel;
public event MouseWheelEventHandler MouseWheel;
public event MouseEventHandler MouseEnter;
public event MouseEventHandler MouseLeave;
public event MouseEventHandler GotMouseCapture;
public event MouseEventHandler LostMouseCapture;
public event QueryCursorEventHandler QueryCursor;
public event StylusDownEventHandler StylusDown;
public event DependencyPropertyChangedEventHandler IsStylusCapturedChanged;
public event EventHandler<ManipulationCompletedEventArgs> ManipulationCompleted;
public event EventHandler<ManipulationBoundaryFeedbackEventArgs> ManipulationBoundaryFeedback;
 
public void AddHandler(RoutedEvent routedEvent, Delegate handler);
public void AddHandler(RoutedEvent routedEvent, Delegate handler, bool handledEventsToo);
public void AddToEventRoute(EventRoute route, RoutedEventArgs e);
public void ApplyAnimationClock(DependencyProperty dp, AnimationClock clock, HandoffBehavior handoffBehavior);
public void ApplyAnimationClock(DependencyProperty dp, AnimationClock clock);
public void Arrange(Rect finalRect);
public void BeginAnimation(DependencyProperty dp, AnimationTimeline animation, HandoffBehavior handoffBehavior);
public void BeginAnimation(DependencyProperty dp, AnimationTimeline animation);
public bool CaptureMouse();
public bool CaptureStylus();
public bool CaptureTouch(TouchDevice touchDevice);
public bool Focus();
public object GetAnimationBaseValue(DependencyProperty dp);
public IInputElement InputHitTest(Point point);
public void InvalidateArrange();
public void InvalidateMeasure();
public void InvalidateVisual();
public void Measure(Size availableSize);
public virtual bool MoveFocus(TraversalRequest request);
public virtual DependencyObject PredictFocus(FocusNavigationDirection direction);
public void RaiseEvent(RoutedEventArgs e);
public void ReleaseAllTouchCaptures();
public void ReleaseMouseCapture();
public void ReleaseStylusCapture();
public bool ReleaseTouchCapture(TouchDevice touchDevice);
public void RemoveHandler(RoutedEvent routedEvent, Delegate handler);
public bool ShouldSerializeCommandBindings();
public bool ShouldSerializeInputBindings();
public Point TranslatePoint(Point point, UIElement relativeTo);
public void UpdateLayout();
protected virtual void ArrangeCore(Rect finalRect);
protected virtual Geometry GetLayoutClip(Size layoutSlotSize);
protected override HitTestResult HitTestCore(PointHitTestParameters hitTestParameters);
protected override GeometryHitTestResult HitTestCore(GeometryHitTestParameters hitTestParameters);
protected virtual Size MeasureCore(Size availableSize);
protected virtual void OnAccessKey(AccessKeyEventArgs e);
protected virtual void OnChildDesiredSizeChanged(UIElement child);
protected virtual AutomationPeer OnCreateAutomationPeer();
protected virtual void OnDragEnter(DragEventArgs e);
protected virtual void OnDragLeave(DragEventArgs e);
protected virtual void OnDragOver(DragEventArgs e);
protected virtual void OnDrop(DragEventArgs e);
protected virtual void OnGiveFeedback(GiveFeedbackEventArgs e);
protected virtual void OnGotFocus(RoutedEventArgs e);
protected virtual void OnGotKeyboardFocus(KeyboardFocusChangedEventArgs e);
protected virtual void OnGotMouseCapture(MouseEventArgs e);
protected virtual void OnGotStylusCapture(StylusEventArgs e);
protected virtual void OnGotTouchCapture(TouchEventArgs e);
protected virtual void OnIsKeyboardFocusedChanged(DependencyPropertyChangedEventArgs e);
protected virtual void OnIsKeyboardFocusWithinChanged(DependencyPropertyChangedEventArgs e);
protected virtual void OnIsMouseCapturedChanged(DependencyPropertyChangedEventArgs e);
protected virtual void OnIsMouseCaptureWithinChanged(DependencyPropertyChangedEventArgs e);
protected virtual void OnIsMouseDirectlyOverChanged(DependencyPropertyChangedEventArgs e);
protected virtual void OnIsStylusCapturedChanged(DependencyPropertyChangedEventArgs e);
protected virtual void OnIsStylusCaptureWithinChanged(DependencyPropertyChangedEventArgs e);
protected virtual void OnIsStylusDirectlyOverChanged(DependencyPropertyChangedEventArgs e);
protected virtual void OnKeyDown(KeyEventArgs e);
protected virtual void OnKeyUp(KeyEventArgs e);
protected virtual void OnLostFocus(RoutedEventArgs e);
protected virtual void OnLostKeyboardFocus(KeyboardFocusChangedEventArgs e);
protected virtual void OnLostMouseCapture(MouseEventArgs e);
protected virtual void OnLostStylusCapture(StylusEventArgs e);
protected virtual void OnLostTouchCapture(TouchEventArgs e);
protected virtual void OnManipulationBoundaryFeedback(ManipulationBoundaryFeedbackEventArgs e);
protected virtual void OnManipulationCompleted(ManipulationCompletedEventArgs e);
protected virtual void OnManipulationDelta(ManipulationDeltaEventArgs e);
protected virtual void OnManipulationInertiaStarting(ManipulationInertiaStartingEventArgs e);
protected virtual void OnManipulationStarted(ManipulationStartedEventArgs e);
protected virtual void OnManipulationStarting(ManipulationStartingEventArgs e);
protected virtual void OnMouseDown(MouseButtonEventArgs e);
protected virtual void OnMouseEnter(MouseEventArgs e);
protected virtual void OnMouseLeave(MouseEventArgs e);
protected virtual void OnMouseLeftButtonDown(MouseButtonEventArgs e);
protected virtual void OnMouseLeftButtonUp(MouseButtonEventArgs e);
protected virtual void OnMouseMove(MouseEventArgs e);
protected virtual void OnMouseRightButtonDown(MouseButtonEventArgs e);
protected virtual void OnMouseRightButtonUp(MouseButtonEventArgs e);
protected virtual void OnMouseUp(MouseButtonEventArgs e);
protected virtual void OnMouseWheel(MouseWheelEventArgs e);
protected virtual void OnPreviewDragEnter(DragEventArgs e);
protected virtual void OnPreviewDragLeave(DragEventArgs e);
protected virtual void OnPreviewDragOver(DragEventArgs e);
protected virtual void OnPreviewDrop(DragEventArgs e);
protected virtual void OnPreviewGiveFeedback(GiveFeedbackEventArgs e);
protected virtual void OnPreviewGotKeyboardFocus(KeyboardFocusChangedEventArgs e);
protected virtual void OnPreviewKeyDown(KeyEventArgs e);
protected virtual void OnPreviewKeyUp(KeyEventArgs e);
protected virtual void OnPreviewLostKeyboardFocus(KeyboardFocusChangedEventArgs e);
protected virtual void OnPreviewMouseDown(MouseButtonEventArgs e);
protected virtual void OnPreviewMouseLeftButtonDown(MouseButtonEventArgs e);
protected virtual void OnPreviewMouseLeftButtonUp(MouseButtonEventArgs e);
protected virtual void OnPreviewMouseMove(MouseEventArgs e);
protected virtual void OnPreviewMouseRightButtonDown(MouseButtonEventArgs e);
protected virtual void OnPreviewMouseRightButtonUp(MouseButtonEventArgs e);
protected virtual void OnPreviewMouseUp(MouseButtonEventArgs e);
protected virtual void OnPreviewMouseWheel(MouseWheelEventArgs e);
protected virtual void OnPreviewQueryContinueDrag(QueryContinueDragEventArgs e);
protected virtual void OnPreviewStylusButtonDown(StylusButtonEventArgs e);
protected virtual void OnPreviewStylusButtonUp(StylusButtonEventArgs e);
protected virtual void OnPreviewStylusDown(StylusDownEventArgs e);
protected virtual void OnPreviewStylusInAirMove(StylusEventArgs e);
protected virtual void OnPreviewStylusInRange(StylusEventArgs e);
protected virtual void OnPreviewStylusMove(StylusEventArgs e);
protected virtual void OnPreviewStylusOutOfRange(StylusEventArgs e);
protected virtual void OnPreviewStylusSystemGesture(StylusSystemGestureEventArgs e);
protected virtual void OnPreviewStylusUp(StylusEventArgs e);
protected virtual void OnPreviewTextInput(TextCompositionEventArgs e);
protected virtual void OnPreviewTouchDown(TouchEventArgs e);
protected virtual void OnPreviewTouchMove(TouchEventArgs e);
protected virtual void OnPreviewTouchUp(TouchEventArgs e);
protected virtual void OnQueryContinueDrag(QueryContinueDragEventArgs e);
protected virtual void OnQueryCursor(QueryCursorEventArgs e);
protected virtual void OnRender(DrawingContext drawingContext);
protected virtual void OnStylusButtonDown(StylusButtonEventArgs e);
protected virtual void OnStylusButtonUp(StylusButtonEventArgs e);
protected virtual void OnStylusDown(StylusDownEventArgs e);
protected virtual void OnStylusEnter(StylusEventArgs e);
protected virtual void OnStylusInAirMove(StylusEventArgs e);
protected virtual void OnStylusInRange(StylusEventArgs e);
protected virtual void OnStylusLeave(StylusEventArgs e);
protected virtual void OnStylusMove(StylusEventArgs e);
protected virtual void OnStylusOutOfRange(StylusEventArgs e);
protected virtual void OnStylusSystemGesture(StylusSystemGestureEventArgs e);
protected virtual void OnStylusUp(StylusEventArgs e);
protected virtual void OnTextInput(TextCompositionEventArgs e);
protected virtual void OnTouchDown(TouchEventArgs e);
protected virtual void OnTouchEnter(TouchEventArgs e);
protected virtual void OnTouchLeave(TouchEventArgs e);
protected virtual void OnTouchMove(TouchEventArgs e);
protected virtual void OnTouchUp(TouchEventArgs e);
protected internal virtual DependencyObject GetUIParentCore();
protected internal virtual void OnRenderSizeChanged(SizeChangedInfo info);
protected internal override void OnVisualParentChanged(DependencyObject oldParent);
 
}
```


UIElement类代码分析

### 第一部分 路由事件

UIElement基类定义了大量的路由事件。什么是路由事件？路由事件和xaml的可视化树概念相关，控件的事件被触发后，会沿着这棵树广播，有两个方向，要么往树的根部广播，要么往树的枝叶广播，如果不广播就是直接事件。

所以，路由事件分为冒泡事件和隧道事件，冒泡，是从触发源为出发点，依次传递到父节点，直到最后的根节点。隧道事件是不管谁是触发源，都是从根节点触发，到子节点，直到触发节点。

从空间上来说，冒泡事件和隧道事件是成对出现的。从时间来说，都是先触发隧道事件，然后是冒泡事件。从命名来说，隧道事件都是以Preview开头的事件。

根据命名规则，我们可以大致猜测出一个结果，带Key的基本都是与键盘相关的事件（如按下键位、抬起键位），带Mouse的基本都是与鼠标相关的事件（如左键单击、双击），带Stylus的基本都是与触摸相关的事件，具体用到哪一类型的事件，再详细查阅一下相关说明文档即可。

重点：关于这些事件的回调函数，即以On开头的方法成员，都被声明成了protected virtual，意思是他们都可以被重载，这使得我们在开发业务时更加方便。

### 第二部分 依赖属性

UIElement基类还定义了大量的依赖属性。前面的章节中，在DependencyObject类中我们简单提到过依赖属性。在这里我们以UIElement基类的Visibility属性为例。

```cs
public Visibility Visibility { get; set; }
 
public static readonly DependencyProperty VisibilityProperty;
```
上面有两个成员，Visibility是普通的属性成员，VisibilityProperty是WPF的依赖属性成员，以Property结尾的字样作为WPF的依赖属性命名规则。而这两个成员合起来，才能被称为一个完整的依赖属性。这个Visibility 属性表示设置或获取控件的可见性。当我们要设置控件的可见性时，只需要如下设置即可。

```cs
<TextBlock Text="WPF中文网" 
                   Visibility="Visible"
                   FontSize="48" 
                   HorizontalAlignment="Center" 
                   VerticalAlignment="Center"/>
```
Visibility实际上是一个枚举，它包含3个值，分别是Visible、Hidden、Collapsed。其含义分别是显示、隐藏、彻底隐藏（不占布局位置）。

**Visibility 状态会影响该元素的所有输入处理。 不可见的元素不会参与命中测试，也不会接收输入事件，即使鼠标位于元素可见时所在的边界上也是如此。**

但是在这一节中，我们只是探讨UIElement基类提供了哪些方面的属性，并不详细探讨依赖属性，所以下面我们把目光聚焦到UIElement基类的常用属性上。另外由于WPF中几乎所有控件都继承了这个基类，意思就是说所有的控件都有这些属性可以使用。下面我在描述的时候将采用“控件”两字来解释一些技术细节。

**Uid属性**:获取或设置控件的唯一标识符，像人们的身份证一样。这个值默认是string.Empty。

**Visibility属性**：获取或设置控件的可见性。默认是Visible。

**ClipToBounds属性**：如果该值为true，表示进行裁剪，以适配它的父控件。比如有时候我们外面有一个Panel，里面的控件尺寸太大，势必会“撑破”外面的父控件，为了布局美观，只好削足适履。

**Clip属性**：用于剪裁区域大小的几何图形。需要注意的是，这个属性和上面的ClipToBounds属性是有区别的。ClipToBounds是裁剪控件自身，Clip是裁剪控件里面的内容。比如Image图像控件，我们在显示一张图时，就可以运用Clip进行裁剪后显示，通常在显示用户头像时裁剪成圆形时使用。如下所示

```html
<Image 
  Source="sampleImages\Waterlilies.jpg" 
  Width="200" Height="150" HorizontalAlignment="Left">
  <Image.Clip>
    <EllipseGeometry
      RadiusX="100"
      RadiusY="75"
      Center="100,75"/>
  </Image.Clip>
</Image>
```

[[assets/WPF从小白到大佬：控件父类：：UIElement类/9316f7eb5be8462366d9f1793003fda8_MD5.jpeg|Open: Pasted image 20240226163908.png]]
![[assets/WPF从小白到大佬：控件父类：：UIElement类/9316f7eb5be8462366d9f1793003fda8_MD5.jpeg]]

**SnapsToDevicePixels属性**：如果该值为true，表示控件的呈现是否应使用特定于设备的像素设置。意思是开启后可以最大限度的防锯齿效果，默认为false。

**IsFocused属性**：这是一个只读属性，表示当前控件是否有焦点。

**IsEnabled属性**：如果该值为true，表示禁用控件，反之启用控件。

**IsHitTestVisible属性**：获取或设置一个值，该值声明是否可以返回此元素作为其呈现内容的某些部分的点击测试结果。

**IsVisible属性**：这是一个只读属性，表示当前控件是否显示。

**Focusable属性**：如果该值为true，表示控件可以得到焦点，大部份内容控件都默认可以获得焦点。

**IsKeyboardFocused属性**：表示该控件是否具有键盘焦点。

**IsMouseOver属性**：表示鼠标是否在控件上面。通常在设计控件的样式（Style）时会用到。

**IsStylusOver属性**：表示触笔指针是否在控件的上方。

**IsSealed属性**：表示当前类型是否为只读类。

**Opacity属性**：设置控件的透明度，取值范围是0-1之间的double值。

**OpacityMask属性**：设置一个画笔，作为控件的蒙板。比如我们给一张图片设置一个掩码，就可以使用ImageBrush这种图片画笔来实现。

```html
<Image Height="150" Width="200" Source="sampleImages/Waterlilies.jpg" >
  <Image.OpacityMask>
    <ImageBrush ImageSource="sampleImages/tornedges.png"/>
  </Image.OpacityMask>
</Image>
```

**AllowDrop属性**：表示控件是否允许拖拽操作。

**RenderTransform属性**：（非常重要）如果要设置控件的移动、缩放、旋转，需要这此属性进行设置。

**UIElement类总结**

通过上述的代码分析，我们大致可以得出以下结论，UIElement基类为我们提供了一系列的鼠标、键盘和触摸事件，并提供了一些常用的依赖属性。它可以呈现继承它的所有控件，为控件布局时调整位置和大小，响应用户的输入，引发一系列的路由事件，并继承了IAnimatable动画接口，用于支持动画的某些方面。

我们熟悉了UIElement的这些属性和事件之后，实际上意味着我们也熟悉了WPF所有控件的这些属性。下一节，我们将探讨UIElement的子类FrameworkElement。

FrameworkElement基类论重要性，完全不亚于UIElement基类。甚至论起与开发者的“亲密度”，FrameworkElement更像是近水的楼台。

——重庆教主 2023年8月15日