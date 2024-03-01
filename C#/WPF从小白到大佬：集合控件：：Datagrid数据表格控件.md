DataGrid是一个可以多选的数据表格控件。所以，它继承一个支持多选的父类——MultiSelector。

```cs
public abstract class MultiSelector : Selector
{
    protected MultiSelector();
 
    public IList SelectedItems { get; }
    protected bool CanSelectMultipleItems { get; set; }
    protected bool IsUpdatingSelectedItems { get; }
 
    public void SelectAll();
    public void UnselectAll();
    protected void BeginUpdateSelectedItems();
    protected void EndUpdateSelectedItems();
 
}
```

从上面的定义来看，DataGrid多选的结果会保存在SelectedItems 只读属性中，CanSelectMultipleItems 属性用来设置是否开启多选。好，然后我们来看看DataGrid控件的定义，虽然它的属性众多，在学习模板样式之后，我们还会进一步学习这个控件的用法。

```cs
public class DataGrid : MultiSelector
{
    public static readonly DependencyProperty CanUserResizeColumnsProperty;
    public static readonly DependencyProperty CurrentItemProperty;
    public static readonly DependencyProperty CurrentColumnProperty;
    public static readonly DependencyProperty CurrentCellProperty;
    public static readonly DependencyProperty CanUserAddRowsProperty;
    public static readonly DependencyProperty CanUserDeleteRowsProperty;
    public static readonly DependencyProperty RowDetailsVisibilityModeProperty;
    public static readonly DependencyProperty AreRowDetailsFrozenProperty;
    public static readonly DependencyProperty RowDetailsTemplateProperty;
    public static readonly DependencyProperty RowDetailsTemplateSelectorProperty;
    public static readonly DependencyProperty CanUserResizeRowsProperty;
    public static readonly DependencyProperty NewItemMarginProperty;
    public static readonly DependencyProperty SelectionModeProperty;
    public static readonly DependencyProperty SelectionUnitProperty;
    public static readonly DependencyProperty CanUserSortColumnsProperty;
    public static readonly DependencyProperty AutoGenerateColumnsProperty;
    public static readonly DependencyProperty FrozenColumnCountProperty;
    public static readonly DependencyProperty NonFrozenColumnsViewportHorizontalOffsetProperty;
    public static readonly DependencyProperty EnableColumnVirtualizationProperty;
    public static readonly DependencyProperty CanUserReorderColumnsProperty;
    public static readonly DependencyProperty DragIndicatorStyleProperty;
    public static readonly DependencyProperty DropLocationIndicatorStyleProperty;
    public static readonly DependencyProperty ClipboardCopyModeProperty;
    public static readonly DependencyProperty CellsPanelHorizontalOffsetProperty;
    public static readonly DependencyProperty IsReadOnlyProperty;
    public static readonly RoutedCommand CancelEditCommand;
    public static readonly DependencyProperty EnableRowVirtualizationProperty;
    public static readonly RoutedCommand BeginEditCommand;
    public static readonly RoutedCommand CommitEditCommand;
    public static readonly DependencyProperty ColumnWidthProperty;
    public static readonly DependencyProperty MinColumnWidthProperty;
    public static readonly DependencyProperty MaxColumnWidthProperty;
    public static readonly DependencyProperty HorizontalGridLinesBrushProperty;
    public static readonly DependencyProperty VerticalGridLinesBrushProperty;
    public static readonly DependencyProperty RowStyleProperty;
    public static readonly DependencyProperty RowValidationErrorTemplateProperty;
    public static readonly DependencyProperty RowStyleSelectorProperty;
    public static readonly DependencyProperty RowBackgroundProperty;
    public static readonly DependencyProperty AlternatingRowBackgroundProperty;
    public static readonly DependencyProperty RowHeightProperty;
    public static readonly DependencyProperty GridLinesVisibilityProperty;
    public static readonly DependencyProperty RowHeaderWidthProperty;
    public static readonly DependencyProperty VerticalScrollBarVisibilityProperty;
    public static readonly DependencyProperty MinRowHeightProperty;
    public static readonly DependencyProperty HorizontalScrollBarVisibilityProperty;
    public static readonly DependencyProperty RowHeaderTemplateProperty;
    public static readonly DependencyProperty RowHeaderStyleProperty;
    public static readonly DependencyProperty RowHeaderTemplateSelectorProperty;
    public static readonly DependencyProperty CellStyleProperty;
    public static readonly DependencyProperty HeadersVisibilityProperty;
    public static readonly DependencyProperty ColumnHeaderHeightProperty;
    public static readonly DependencyProperty RowHeaderActualWidthProperty;
    public static readonly DependencyProperty ColumnHeaderStyleProperty;
 
    public DataGrid();
 
    public static ComponentResourceKey FocusBorderBrushKey { get; }
    public static RoutedUICommand SelectAllCommand { get; }
    public static IValueConverter HeadersVisibilityConverter { get; }
    public static IValueConverter RowDetailsScrollingConverter { get; }
    public static RoutedUICommand DeleteCommand { get; }
    public DataTemplate RowHeaderTemplate { get; set; }
    public DataTemplateSelector RowHeaderTemplateSelector { get; set; }
    public ScrollBarVisibility VerticalScrollBarVisibility { get; set; }
    public ScrollBarVisibility HorizontalScrollBarVisibility { get; set; }
    public bool CanUserAddRows { get; set; }
    public object CurrentItem { get; set; }
    public DataGridColumn CurrentColumn { get; set; }
    public DataGridCellInfo CurrentCell { get; set; }
    public bool CanUserDeleteRows { get; set; }
    public Style RowHeaderStyle { get; set; }
    public DataGridRowDetailsVisibilityMode RowDetailsVisibilityMode { get; set; }
    public bool IsReadOnly { get; set; }
    public Style ColumnHeaderStyle { get; set; }
    public Style RowStyle { get; set; }
    public DataGridHeadersVisibility HeadersVisibility { get; set; }
    public bool AreRowDetailsFrozen { get; set; }
    public Brush AlternatingRowBackground { get; set; }
    public Brush RowBackground { get; set; }
    public StyleSelector RowStyleSelector { get; set; }
    public ObservableCollection<ValidationRule> RowValidationRules { get; }
    public ControlTemplate RowValidationErrorTemplate { get; set; }
    public Brush VerticalGridLinesBrush { get; set; }
    public Brush HorizontalGridLinesBrush { get; set; }
    public DataGridGridLinesVisibility GridLinesVisibility { get; set; }
    public double MaxColumnWidth { get; set; }
    public double MinColumnWidth { get; set; }
    public DataGridLength ColumnWidth { get; set; }
    public bool CanUserResizeColumns { get; set; }
    public ObservableCollection<DataGridColumn> Columns { get; }
    public double RowHeaderWidth { get; set; }
    public double RowHeaderActualWidth { get; }
    public double ColumnHeaderHeight { get; set; }
    public Style CellStyle { get; set; }
    public DataTemplate RowDetailsTemplate { get; set; }
    public double MinRowHeight { get; set; }
    public bool CanUserResizeRows { get; set; }
    public double RowHeight { get; set; }
    public DataTemplateSelector RowDetailsTemplateSelector { get; set; }
    public double CellsPanelHorizontalOffset { get; }
    public DataGridClipboardCopyMode ClipboardCopyMode { get; set; }
    public Style DropLocationIndicatorStyle { get; set; }
    public bool CanUserReorderColumns { get; set; }
    public bool EnableColumnVirtualization { get; set; }
    public bool EnableRowVirtualization { get; set; }
    public Style DragIndicatorStyle { get; set; }
    public double NonFrozenColumnsViewportHorizontalOffset { get; }
    public int FrozenColumnCount { get; set; }
    public bool AutoGenerateColumns { get; set; }
    public Thickness NewItemMargin { get; }
    public bool CanUserSortColumns { get; set; }
    public DataGridSelectionUnit SelectionUnit { get; set; }
    public DataGridSelectionMode SelectionMode { get; set; }
    public IList<DataGridCellInfo> SelectedCells { get; }
    protected internal override bool HandlesScrolling { get; }
 
    public event DataGridSortingEventHandler Sorting;
    public event EventHandler AutoGeneratedColumns;
    public event EventHandler<DataGridAutoGeneratingColumnEventArgs> AutoGeneratingColumn;
    public event EventHandler<DragDeltaEventArgs> ColumnHeaderDragDelta;
    public event EventHandler<DragStartedEventArgs> ColumnHeaderDragStarted;
    public event EventHandler<DragCompletedEventArgs> ColumnHeaderDragCompleted;
    public event SelectedCellsChangedEventHandler SelectedCellsChanged;
    public event EventHandler<DataGridColumnReorderingEventArgs> ColumnReordering;
    public event EventHandler<DataGridRowDetailsEventArgs> RowDetailsVisibilityChanged;
    public event EventHandler<DataGridRowEventArgs> UnloadingRow;
    public event EventHandler<DataGridRowDetailsEventArgs> LoadingRowDetails;
    public event InitializingNewItemEventHandler InitializingNewItem;
    public event EventHandler<DataGridPreparingCellForEditEventArgs> PreparingCellForEdit;
    public event EventHandler<DataGridBeginningEditEventArgs> BeginningEdit;
    public event EventHandler<EventArgs> CurrentCellChanged;
    public event EventHandler<DataGridCellEditEndingEventArgs> CellEditEnding;
    public event EventHandler<DataGridRowEditEndingEventArgs> RowEditEnding;
    public event EventHandler<DataGridRowEventArgs> LoadingRow;
    public event EventHandler<DataGridColumnEventArgs> ColumnDisplayIndexChanged;
    public event EventHandler<DataGridRowDetailsEventArgs> UnloadingRowDetails;
    public event EventHandler<AddingNewItemEventArgs> AddingNewItem;
    public event EventHandler<DataGridRowClipboardEventArgs> CopyingRowClipboardContent;
    public event EventHandler<DataGridColumnEventArgs> ColumnReordered;
 
    public static Collection<DataGridColumn> GenerateColumns(IItemProperties itemProperties);
    public bool BeginEdit();
    public bool BeginEdit(RoutedEventArgs editingEventArgs);
    public bool CancelEdit();
    public bool CancelEdit(DataGridEditingUnit editingUnit);
    public void ClearDetailsVisibilityForItem(object item);
    public DataGridColumn ColumnFromDisplayIndex(int displayIndex);
    public bool CommitEdit();
    public bool CommitEdit(DataGridEditingUnit editingUnit, bool exitEditingMode);
    public Visibility GetDetailsVisibilityForItem(object item);
    public override void OnApplyTemplate();
    public void ScrollIntoView(object item);
    public void ScrollIntoView(object item, DataGridColumn column);
    public void SelectAllCells();
    public void SetDetailsVisibilityForItem(object item, Visibility detailsVisibility);
    public void UnselectAllCells();
    protected override void ClearContainerForItemOverride(DependencyObject element, object item);
    protected override DependencyObject GetContainerForItemOverride();
    protected override bool IsItemItsOwnContainerOverride(object item);
    protected override Size MeasureOverride(Size availableSize);
    protected virtual void OnAddingNewItem(AddingNewItemEventArgs e);
    protected virtual void OnAutoGeneratedColumns(EventArgs e);
    protected virtual void OnAutoGeneratingColumn(DataGridAutoGeneratingColumnEventArgs e);
    protected virtual void OnBeginningEdit(DataGridBeginningEditEventArgs e);
    protected virtual void OnCanExecuteBeginEdit(CanExecuteRoutedEventArgs e);
    protected virtual void OnCanExecuteCancelEdit(CanExecuteRoutedEventArgs e);
    protected virtual void OnCanExecuteCommitEdit(CanExecuteRoutedEventArgs e);
    protected virtual void OnCanExecuteCopy(CanExecuteRoutedEventArgs args);
    protected virtual void OnCanExecuteDelete(CanExecuteRoutedEventArgs e);
    protected virtual void OnCellEditEnding(DataGridCellEditEndingEventArgs e);
    protected override void OnContextMenuOpening(ContextMenuEventArgs e);
    protected virtual void OnCopyingRowClipboardContent(DataGridRowClipboardEventArgs args);
    protected override AutomationPeer OnCreateAutomationPeer();
    protected virtual void OnCurrentCellChanged(EventArgs e);
    protected virtual void OnExecutedBeginEdit(ExecutedRoutedEventArgs e);
    protected virtual void OnExecutedCancelEdit(ExecutedRoutedEventArgs e);
    protected virtual void OnExecutedCommitEdit(ExecutedRoutedEventArgs e);
    protected virtual void OnExecutedCopy(ExecutedRoutedEventArgs args);
    protected virtual void OnExecutedDelete(ExecutedRoutedEventArgs e);
    protected virtual void OnInitializingNewItem(InitializingNewItemEventArgs e);
    protected override void OnIsMouseCapturedChanged(DependencyPropertyChangedEventArgs e);
    protected override void OnItemsChanged(NotifyCollectionChangedEventArgs e);
    protected override void OnItemsSourceChanged(IEnumerable oldValue, IEnumerable newValue);
    protected override void OnKeyDown(KeyEventArgs e);
    protected virtual void OnLoadingRow(DataGridRowEventArgs e);
    protected virtual void OnLoadingRowDetails(DataGridRowDetailsEventArgs e);
    protected override void OnMouseMove(MouseEventArgs e);
    protected virtual void OnRowEditEnding(DataGridRowEditEndingEventArgs e);
    protected virtual void OnSelectedCellsChanged(SelectedCellsChangedEventArgs e);
    protected override void OnSelectionChanged(SelectionChangedEventArgs e);
    protected virtual void OnSorting(DataGridSortingEventArgs eventArgs);
    protected override void OnTemplateChanged(ControlTemplate oldTemplate, ControlTemplate newTemplate);
    protected override void OnTextInput(TextCompositionEventArgs e);
    protected virtual void OnUnloadingRow(DataGridRowEventArgs e);
    protected virtual void OnUnloadingRowDetails(DataGridRowDetailsEventArgs e);
    protected override void PrepareContainerForItemOverride(DependencyObject element, object item);
    protected internal virtual void OnColumnDisplayIndexChanged(DataGridColumnEventArgs e);
    protected internal virtual void OnColumnHeaderDragCompleted(DragCompletedEventArgs e);
    protected internal virtual void OnColumnHeaderDragDelta(DragDeltaEventArgs e);
    protected internal virtual void OnColumnHeaderDragStarted(DragStartedEventArgs e);
    protected internal virtual void OnColumnReordered(DataGridColumnEventArgs e);
    protected internal virtual void OnColumnReordering(DataGridColumnReorderingEventArgs e);
    protected internal virtual void OnPreparingCellForEdit(DataGridPreparingCellForEditEventArgs e);
    protected internal virtual void OnRowDetailsVisibilityChanged(DataGridRowDetailsEventArgs e);
 
}
```

二、属性分析

DataGrid提供了大量的依赖属性，合理充分利用这些属性，在开发ERP、CMS、报表等软件时可达到事半功倍的效果。下面我们以表格的形式，先了解一下各属性的功能，然后在本节中学习一些基础属性，以掌握该控件的基本用法，剩下的属性放到模板样式的章节中学习。

|                                                |                                       |     |
| ---------------------------------------------- | ------------------------------------- | --- |
| 属性名称                                           | 说明                                    | 备注  |
| FocusBorderBrushKey                            | 获取引用焦点的单元格的默认边框画笔的键。                  |     |
| SelectAllCommand                               | 表示指示想要选择的所有单元格的命令                     |     |
| HeadersVisibilityConverter                     | 获取标题显示隐藏的转换器，即HeadersVisibility属性的转换器 |     |
| RowDetailsScrollingConverter                   | 获取将转换为一个布尔值转换器                        |     |
| DeleteCommand                                  | 表示指示想要删除当前行的命令。                       |     |
| RowHeaderTemplate                              | 获取或设置行标题的模板。                          | 重要  |
| RowHeaderTemplateSelector                      | 获取或设置行标题的模板选择器。                       |     |
| VerticalScrollBarVisibility                    | 是否显示垂直滚动条                             |     |
| HorizontalScrollBarVisibility                  | 是否显示水平滚动条                             |     |
| CanUserAddRows                                 | 是否可以添加新行                              | 重要  |
| CurrentItem                                    | 当前选中行（一般指绑定的数据源的某一个元素）                | 常用  |
| CurrentColumn                                  | 获取或设置包含当前单元格的列。                       |     |
| CurrentCell                                    | 获取或设置具有焦点的单元格。                        |     |
| CanUserDeleteRows                              | 是否可以删除行                               | 重要  |
| RowHeaderStyle                                 | 获取或设置应用于所有行标题的样式。                     | 重要  |
| RowDetailsVisibilityMode                       | 获取或设置一个值，该值指示何时显示某行的详细信息部分。           |     |
| IsReadOnly                                     | 当前控件是否只读                              | 常用  |
| ColumnHeaderStyle                              | 获取或设置所有列标题的样式                         | 重要  |
| RowStyle                                       | 获取或设置应用到的所有行的样式。                      | 重要  |
| HeadersVisibility                              | 获取或设置用于指定行和列标题的可见性的值。                 |     |
| AreRowDetailsFrozen                            | 获取或设置一个值，该值指示是否可水平滚动行详细信息。            |     |
| AlternatingRowBackground                       | 获取或设置交替行上使用的背景画笔。                     | 重要  |
| RowBackground                                  | 获取或设置用于行背景的默认画笔。                      |     |
| RowStyleSelector                               | 获取或设置行的样式选择器。                         |     |
| RowValidationRules                             | 获取用于验证每个行中的数据的规则。                     |     |
| RowValidationErrorTemplate                     | 获取或设置用于以可视方式指示行验证中的错误的模板。             |     |
| VerticalGridLinesBrush                         | 获取或设置用于绘制垂直网格线的画笔。                    | 常用  |
| HorizontalGridLinesBrush                       | 获取或设置用于绘制水平网格线的画笔。                    |     |
| GridLinesVisibility                            | 获取或设置一个值，该值指示显示哪些网格线。                 |     |
| MaxColumnWidth                                 | 获取或设置列和标头中的最大宽度约束                     |     |
| MinColumnWidth                                 | 获取或设置列和标头中的最小宽度约束                     |     |
| ColumnWidth                                    | 获取或设置标准宽度和列和中的标头的大小调整模式               |     |
| CanUserResizeColumns                           | 获取或设置用户是否可以通过使用鼠标调整列的宽度。              |     |
| Columns                                        | 获取一个集合中的所有列                           | 常用  |
| RowHeaderWidth                                 | 获取或设置行标题列的宽度。                         |     |
| RowHeaderActualWidth                           | 获取呈现的行标题列的宽度。                         |     |
| ColumnHeaderHeight                             | 获取或设置列标题行的高度。                         |     |
| CellStyle                                      | 获取或设置所有单元格的样式                         | 常用  |
| RowDetailsTemplate                             | 获取或设置用于显示行详细信息的模板。                    |     |
| MinRowHeight                                   | 获取或设置行和中的标头的最小高度约束                    |     |
| CanUserResizeRows                              | 获取或设置用户是否可以通过使用鼠标调整行的高度。              |     |
| RowHeight                                      | 获取或设置的所有行的建议的高度。                      |     |
| RowDetailsTemplateSelector                     | 获取或设置用于行详细信息的模板选择器。                   |     |
| CellsPanelHorizontalOffset                     | 获取DataGridCellsPanel的水平偏移量            |     |
| ClipboardCopyMode                              | 获取或设置一个值，指示如何将内容复制到剪贴板。               |     |
| NonFrozenColumns  <br>ViewportHorizontalOffset | 获取在视区的可滚动列的水平偏移量。                     |     |
| FrozenColumnCount                              | 获取或设置非滚动列的数量。                         | 常用  |
| AutoGenerateColumns                            | 获取或设置一个值，该值指示是否自动创建列。                 | 常用  |
| NewItemMargin                                  | 获取或设置新的项目行的边距。                        |     |
| CanUserSortColumns                             | 是否可以单击列标题来对列排序。                       | 常用  |
| SelectionUnit                                  | 选择行的模式                                |     |
| SelectionMode                                  | 是否支持多选                                | 重要  |
| SelectedCells                                  | 获取当前选定的单元格的列表。                        |     |
| HandlesScrolling                               | 是否支持自定义键盘滚动。                          |     |

在上述表格中，Columns属性是DataGrid最基本的一个属性。它是一个`ObservableCollection<DataGridColumn>`类型的集合，表示DataGrid的列的集合。其实DataGridColumn只是一个抽象基类，我们真正在实例化时，是实例化DataGridColumn的子类，然后放到Columns属性中。

那么DataGridColumn有哪些子类呢？

- DataGridTextColumn 表示文本内容的列
- DataGridCheckBoxColumn 表示复选框的列
- DataGridComboBoxColumn 表示下拉框的列
- DataGridTemplateColumn 表示模板的列（万金油）

在本列中，我们将以最简单的DataGridTextColumn 为例。

三、事件成员

DataGrid一共有23个事件成员，它们分别如下所示
  
|   |   |
| ------|------|
| 事件名称|说明|
| Sorting|对列进行排序时发生。|
| AutoGeneratedColumns|所有列的自动生成完成后发生。|
| AutoGeneratingColumn|自动生成单独的列时出现。|
| ColumnHeaderDragDelta|每次鼠标位置发生更改时在用户拖动列标题时发生。|
| ColumnHeaderDragStarted|当用户开始使用鼠标拖动列标题时发生。|
|ColumnHeaderDragCompleted|当用户使用鼠标拖动后释放列标题时发生。|
|SelectedCellsChanged|发生时 DataGrid.SelectedCells 集合更改。|
|ColumnReordering|在列移至的显示顺序中的新位置之前发生。|
|RowDetailsVisibilityChanged|当某一行的可见性详细信息元素更改时发生。|
|UnloadingRow|发生时 DataGridRow 对象将成为可供重用。|
|LoadingRowDetails|新的行的详细信息模板应用于行时发生。|
|InitializingNewItem|创建一个新项时出现。|
|PreparingCellForEdit|在单元格进入编辑模式时发生。|
|BeginningEdit|发生行或单元格进入编辑模式之前。|
|CurrentCellChanged|在 DataGrid.CurrentCell 属性的值更改后发生。|
|CellEditEnding|在单元格的编辑将在提交或取消前发生。|
|RowEditEnding|在提交或取消行编辑之前发生。|
|LoadingRow|加载row时|
|ColumnDisplayIndexChanged|其中一个列更改属性时|
|UnloadingRowDetails|行详细信息元素成为可供重用时发生。|
|AddingNewItem|新项添加到DataGrid之前发生|
|CopyingRowClipboardContent|默认行内容准备好之后发生。|
|ColumnReordered|在列移至的显示顺序中的新位置时发生。|

四、基本示例

前端代码

```cs
<Window x:Class="HelloWorld.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:HelloWorld" 
        xmlns:forms="clr-namespace:System.Windows.Forms;assembly=System.Windows.Forms"
        mc:Ignorable="d" FontSize="14"
        Title="WPF中文网之控件课程 - www.wpfsoft.com" Height="350" Width="500">
    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition/>
            <ColumnDefinition Width="200"/>
        </Grid.ColumnDefinitions>
        <DataGrid x:Name="datagrid" 
                  SelectionMode="Extended"
                  IsReadOnly="True" 
                  SelectionChanged="datagrid_Selected">
            <DataGrid.Columns>
                <DataGridTextColumn Header="姓名" Binding="{Binding Name}" />
                <DataGridTextColumn Header="年龄" Binding="{Binding Age}" />
                <DataGridTextColumn Header="地址" Binding="{Binding Address}" />
            </DataGrid.Columns>
        </DataGrid>
        
        <StackPanel Grid.Column="1">
            <StackPanel Orientation="Horizontal" Margin="5">
                <TextBlock Text="姓名:"/>
                <TextBlock x:Name="_TextBlockName"/>
            </StackPanel>
            <StackPanel Orientation="Horizontal" Margin="5">
                <TextBlock Text="年龄:"/>
                <TextBlock x:Name="_TextBlockAge"/>
            </StackPanel>
            <StackPanel Orientation="Horizontal" Margin="5">
                <TextBlock Text="地址:"/>
                <TextBlock x:Name="_TextBlockAddress"/>
            </StackPanel>
        </StackPanel>
    </Grid>
</Window>
```

后端代码

```cs
namespace HelloWorld
{
    public class Person
    {
        public string Name { get; set; }
        public int Age { get; set; }
        public string Address { get; set; }
 
    }
 
    /// <summary>
    /// MainWindow.xaml 的交互逻辑
    /// </summary>
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            
            datagrid.Items.Add(new Person { Name = "张三", Age = 22, Address = "广东省廉江市车板镇大坝村" });
            datagrid.Items.Add(new Person { Name = "李四", Age = 23, Address = "江西省景德镇市市辖区" });
            datagrid.Items.Add(new Person { Name = "王五", Age = 24, Address = "上海市虹口区" });
        }
 
        private void datagrid_Selected(object sender, RoutedEventArgs e)
        {
            DataGrid datagrid = sender as DataGrid;
            if (datagrid == null) return;
 
            var person = datagrid.SelectedItem as Person;
            if (person == null) return;
 
            _TextBlockName.Text = person.Name;
            _TextBlockAge.Text = person.Age + "岁";
            _TextBlockAddress.Text = person.Address;
        }
    }
}
```

![[assets/WPF从小白到大佬：集合控件：：Datagrid数据表格控件/b4b6ce75cab332f337fcb9837a353a53_MD5.jpg]]

在这个例子中，我们尽量还原了与ListView控件一致的功能， 需要注意的细节是：我们将DataGrid的IsReadOnly="True"，这是因为我们直接将数据元素一条一条的加入到DataGrid的Items属性中，而Items属性本身是一个只读属性，不支持写入。这样的话，当鼠标双击时会报错。错误提示为：

![[assets/WPF从小白到大佬：集合控件：：Datagrid数据表格控件/93e606cc4a4a2ed7f6a837a2484519cf_MD5.jpg]]

如何解决这个问题呢？这就要用到ItemsControl基类中的ItemsSource数据源属性。

我们需要采用DataGrid另外一种赋值方式——数据源赋值。即把一个集合绑定到该属性上，这样在前端就可以编辑数据源，从而不会引发报错。

只需要改动一点点代码。

```cs
List<Person> list = new List<Person>();
 
list.Add(new Person { Name = "张三", Age = 22, Address = "广东省廉江市车板镇大坝村" });
list.Add(new Person { Name = "李四", Age = 23, Address = "江西省景德镇市市辖区" });
list.Add(new Person { Name = "王五", Age = 24, Address = "上海市虹口区" });
 
datagrid.ItemsSource = list;
```

前端代码也可以修改如下，AutoGenerateColumns属性设为不可自动创建列。

```cs
<DataGrid x:Name="datagrid" 
          SelectionMode="Extended"
          IsReadOnly="False"
          AutoGenerateColumns="False"
          SelectionChanged="datagrid_Selected">
    <DataGrid.Columns>
        <DataGridTextColumn Header="姓名" Binding="{Binding Name}" />
        <DataGridTextColumn Header="年龄" Binding="{Binding Age}" />
        <DataGridTextColumn Header="地址" Binding="{Binding Address}" />
    </DataGrid.Columns>
</DataGrid>
```

![[assets/WPF从小白到大佬：集合控件：：Datagrid数据表格控件/622c2d9340ff84bdcc5a213bab1a3122_MD5.jpg]]

如此，我们便可以在DataGrid中新增一行，并输入新的数据。

本节内容我们用了两个例子来说明datagrid最基本的数据加载、显示与获取的功能，请在下面的地址中下载示例。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：041-《DataGrid数据表格控件》-源代码 -1，041-《DataGrid数据表格控件》-源代码 -2  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年9月6日


