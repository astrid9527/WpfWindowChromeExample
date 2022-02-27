# WpfWindowChromeExample

ԭ�ĵ�ַ��https://www.developerastrid.com/wpf/customize-title-bar-with-windowchrome-in-wpf/

���Ľ������� WPF �����ʹ�� WindowChrome �Զ����������

## һ��ʹ�� WindowChrome

�� `<Window></Window>` �ڵ���д���´�����ܹ���ɿͻ�����Client Area�����ǿͻ�����Non-client Area���ĸ��ǣ�

```xml
<WindowChrome.WindowChrome>
    <WindowChrome />
</WindowChrome.WindowChrome>
```

<figure><img src="https://cdn.developerastrid.com/img/202202271350196.png" alt="��ʽ�Ѿ����ڵ�" class="img-fluid mx-auto d-block figure-img"><figcaption class="figure-caption text-center">
            <p>��ʽ�Ѿ����ڵ�</p>
        </figcaption>
</figure>

���ڣ�Ϊ���ܹ��۲쵽 `WindowChrome` �����������õ�Ч��������Ϊ `Window` ����һ���µ� `Template`��������ǿյģ�������û��ʲô�����ܹ��ڵ��������õ���ʽ�ˡ�

```xml
<Window.Template>
    <ControlTemplate TargetType="Window">
        <Border />
    </ControlTemplate>
</Window.Template>
```

<figure><img src="https://cdn.developerastrid.com/img/202202271352396.png" alt="û���ڵ��Ĵ���" class="img-fluid mx-auto d-block figure-img"><figcaption class="figure-caption text-center">
            <p>û���ڵ��Ĵ���</p>
        </figcaption>
</figure>

Ȼ��������ˣ�����Ҳֻ�����ϵͳ����ɫ�߿�����⣬û�н���������ڵ���ק�������⡣���ұ߿����֮��

### 1.1 `GlassFrameThickness` �� `NonClientFrameEdges`

```xml
<WindowChrome.WindowChrome>
    <WindowChrome GlassFrameThickness="0 30 0 0" NonClientFrameEdges="Left,Bottom,Right"/>
</WindowChrome.WindowChrome>
```

<figure><img src="https://cdn.developerastrid.com/img/202202271356743.png" alt="���� GlassFrameThickness �� NonClientFrameEdges" class="img-fluid mx-auto d-block figure-img"><figcaption class="figure-caption text-center">
            <p>���� GlassFrameThickness �� NonClientFrameEdges</p>
        </figcaption>
</figure>

�������������ģ��ñȽ����ˡ�

## �������� Window �Ŀؼ�ģ��

`WindowChrome` �ṩ�ͻ������ݸ��ǵ��ǿͻ�������������������ͨ������ `Window` �� `ControlTemplate` �ܹ��ڱ�֤ԭ�����������ͬʱ�������ܶ������ǵĴ�����ʽ��

```xml
<Window.Template>
    <ControlTemplate TargetType="Window">
        <Border Name="RootBorder">
            <Grid Background="Aqua">
                <AdornerDecorator>
                    <ContentPresenter/>
                </AdornerDecorator>
            </Grid>
        </Border>
    </ControlTemplate>
</Window.Template>

<WindowChrome.WindowChrome>
    <WindowChrome GlassFrameThickness="0 30 0 0" NonClientFrameEdges="Left,Bottom,Right"/>
</WindowChrome.WindowChrome>

<Grid>
    <TextBlock Text="12345"></TextBlock>
</Grid>
```

<figure><img src="https://cdn.developerastrid.com/img/202202271421984.png" alt="�ͻ������ݸ��ǵ��ǿͻ���" class="img-fluid mx-auto d-block figure-img"><figcaption class="figure-caption text-center">
            <p>�ͻ������ݸ��ǵ��ǿͻ���</p>
        </figcaption>
</figure>

���Կ��� `<Grid>`���ͻ����������ݸ��ǵ��˷ǿͻ���������Ĭ�ϵġ���С����������󻯡������رա����ܻ����ڵģ�Ҳ�ǿ��Ե���ģ�ֻ�ǿ������˶��ѣ����ͻ������ǣ���

���ǵ���һ�±߾࣬�÷ǿͻ�����¶������

```xml
<Border Name="RootBorder"  Padding="0 30 0 0" BorderBrush="Transparent" BorderThickness="4 0 4 4">
    <Grid Background="Aqua">
        <AdornerDecorator>
            <ContentPresenter/>
        </AdornerDecorator>
    </Grid>
</Border>
```

<figure><img src="https://cdn.developerastrid.com/img/202202271426466.png" alt="�����ͻ�����ı߾�" class="img-fluid mx-auto d-block figure-img"><figcaption class="figure-caption text-center">
            <p>�����ͻ�����ı߾�</p>
        </figcaption>
</figure>

### 2.1 ���Ʋ���

�� `Grid` ��Ϊ�������У�����һ����Ϊ������������һ����Ϊ�������塣

```xml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"></RowDefinition>
        <RowDefinition Height="*"></RowDefinition>
    </Grid.RowDefinitions>
    <!--������ ��ʼ-->
    <Border Grid.Row="0" Height="30" Margin="0 -29 0 0">
        <DockPanel>
            <StackPanel Orientation="Horizontal" DockPanel.Dock="Right">
                <Button Content="��С��"></Button>
                <Button Content="���"></Button>
                <Button Content="���»�ԭ"></Button>
                <Button Content="�ر�"></Button>
            </StackPanel>
            <StackPanel Orientation="Horizontal">
                <TextBlock 
                    Text="�Զ���ı�����" 
                    VerticalAlignment="Center"></TextBlock>
            </StackPanel>
        </DockPanel>
    </Border>
    <!--������ ����-->
    <Grid Grid.Row="1">
        <TextBlock Text="������������"></TextBlock>
    </Grid>
</Grid>
```

<figure><img src="https://cdn.developerastrid.com/img/202202271434450.png" alt="���Ʋ���" class="img-fluid mx-auto d-block figure-img"><figcaption class="figure-caption text-center">
            <p>���Ʋ���</p>
        </figcaption>
</figure>

���ڣ�������Ͻǣ����ǻᴥ��Ĭ�ϵġ���С����������󻯡������رա���Ϊ�����ԣ�������Ҫ��������Ĭ����Ϊ��

```xml
<WindowChrome.WindowChrome>
    <WindowChrome GlassFrameThickness="0 30 0 0" NonClientFrameEdges="Left,Bottom,Right" UseAeroCaptionButtons="False"/>
</WindowChrome.WindowChrome>
```

### 2.2 �Զ��塰��С����������󻯡������رա��¼�

��������

```xml
<Window.CommandBindings>
    <CommandBinding Command="{x:Static SystemCommands.CloseWindowCommand}" CanExecute="CommandBinding_OnCanExecute" Executed="CommandBinding_OnExecuted_CloseWindow"/>
    <CommandBinding Command="{x:Static SystemCommands.MaximizeWindowCommand}" CanExecute="CommandBinding_OnCanExecute" Executed="CommandBinding_OnExecuted_MaximizeWindow"/>
    <CommandBinding Command="{x:Static SystemCommands.MinimizeWindowCommand}" CanExecute="CommandBinding_OnCanExecute" Executed="CommandBinding_OnExecuted_MinimizeWindow"/>
    <CommandBinding Command="{x:Static SystemCommands.RestoreWindowCommand}" CanExecute="CommandBinding_OnCanExecute" Executed="CommandBinding_OnExecuted_RestoreWindow"/>
</Window.CommandBindings>
```

��ť�ϰ�����

```xml
<Button Name="ButtonMinimizeWindow"  Content="��С��" 
        WindowChrome.IsHitTestVisibleInChrome="True"
        Command="{x:Static SystemCommands.MinimizeWindowCommand}"></Button>
<Button Name="ButtonMaximizeWindow" Content="���" 
        WindowChrome.IsHitTestVisibleInChrome="True"
        Command="{x:Static SystemCommands.MaximizeWindowCommand}"></Button>
<Button Name="ButtonRestoreWindow" Content="���»�ԭ" 
        WindowChrome.IsHitTestVisibleInChrome="True"
        Command="{x:Static SystemCommands.RestoreWindowCommand}"></Button>
<Button Name="ButtonCloseWindow" Content="�ر�"
        WindowChrome.IsHitTestVisibleInChrome="True"
        Command="{x:Static SystemCommands.CloseWindowCommand}"></Button>
```

### 2.3 ������ť

����������Դ���ο� [https://blog.csdn.net/mybelief321/article/details/102461597](https://blog.csdn.net/mybelief321/article/details/102461597)

```xml
<Window.Resources>
    <ResourceDictionary>
        <FontFamily x:Key="IconFont">pack://application:,,,/WindowChromeExample;component/#iconfont,</FontFamily>
    </ResourceDictionary>
</Window.Resources>
```

ʹ��������Դ

```xml
<Button Name="ButtonMinimizeWindow" FontFamily="{StaticResource IconFont}"  Content="&#xe629;"  Width="24" Height="24"
        WindowChrome.IsHitTestVisibleInChrome="True"
        Command="{x:Static SystemCommands.MinimizeWindowCommand}"></Button>
<Button Name="ButtonMaximizeWindow" FontFamily="{StaticResource IconFont}" Content="&#xe653;"  Width="24" Height="24"
        WindowChrome.IsHitTestVisibleInChrome="True"
        Command="{x:Static SystemCommands.MaximizeWindowCommand}"></Button>
<Button Name="ButtonRestoreWindow" FontFamily="{StaticResource IconFont}" Content="&#xe677;"  Width="24" Height="24"
        Visibility="Collapsed"
        WindowChrome.IsHitTestVisibleInChrome="True"
        Command="{x:Static SystemCommands.RestoreWindowCommand}"></Button>
<Button Name="ButtonCloseWindow" FontFamily="{StaticResource IconFont}" Content="&#xeaf2;"  Width="24" Height="24"
        WindowChrome.IsHitTestVisibleInChrome="True"
        Command="{x:Static SystemCommands.CloseWindowCommand}"></Button>
```

<figure><img src="https://cdn.developerastrid.com/img/202202271504795.png" alt="������ť" class="img-fluid mx-auto d-block figure-img"><figcaption class="figure-caption text-center">
            <p>������ť</p>
        </figcaption>
</figure>

Ĭ�ϵİ�ť���ǻ���ʾ�������Ӹ��������Ϳ������ˡ�

```xml
<Border Grid.Row="0" Height="30" Margin="0 -29 0 0">
    <DockPanel Background="Red">
        <StackPanel Orientation="Horizontal" DockPanel.Dock="Right">
            <Button Name="ButtonMinimizeWindow" FontFamily="{StaticResource IconFont}"  Content="&#xe629;"  Width="24" Height="24"
                    WindowChrome.IsHitTestVisibleInChrome="True"
                    Command="{x:Static SystemCommands.MinimizeWindowCommand}"></Button>
            <Button Name="ButtonMaximizeWindow" FontFamily="{StaticResource IconFont}" Content="&#xe653;"  Width="24" Height="24"
                    WindowChrome.IsHitTestVisibleInChrome="True"
                    Command="{x:Static SystemCommands.MaximizeWindowCommand}"></Button>
            <Button Name="ButtonRestoreWindow" FontFamily="{StaticResource IconFont}" Content="&#xe677;"  Width="24" Height="24"
                    Visibility="Collapsed"
                    WindowChrome.IsHitTestVisibleInChrome="True"
                    Command="{x:Static SystemCommands.RestoreWindowCommand}"></Button>
            <Button Name="ButtonCloseWindow" FontFamily="{StaticResource IconFont}" Content="&#xeaf2;"  Width="24" Height="24"
                    WindowChrome.IsHitTestVisibleInChrome="True"
                    Command="{x:Static SystemCommands.CloseWindowCommand}"></Button>
        </StackPanel>
        <StackPanel Orientation="Horizontal">
            <TextBlock 
                Text="�Զ���ı�����" 
                VerticalAlignment="Center"></TextBlock>
        </StackPanel>
    </DockPanel>
</Border>
```

<figure><img src="https://cdn.developerastrid.com/img/202202271507818.png" alt="��ɫ����" class="img-fluid mx-auto d-block figure-img"><figcaption class="figure-caption text-center">
            <p>��ɫ����</p>
        </figcaption>
</figure>

## ������������

### 3.1 XAML ����

```xml
<Window x:Class="WindowChromeExample.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        Title="MainWindow" Height="450" Width="800">

    <!--������Դ-->
    <Window.Resources>
        <ResourceDictionary>
            <FontFamily x:Key="IconFont">pack://application:,,,/WindowChromeExample;component/#iconfont,</FontFamily>
        </ResourceDictionary>
    </Window.Resources>

    <!--�Զ�������� ��ʼ-->
    <Window.Template>
        <ControlTemplate TargetType="Window">
            <Border Name="RootBorder"  Padding="0 30 0 0" BorderBrush="Transparent" BorderThickness="4 0 4 4">
                <Grid Background="{TemplateBinding Background}">
                    <AdornerDecorator>
                        <ContentPresenter/>
                    </AdornerDecorator>
                </Grid>
            </Border>
            <ControlTemplate.Triggers>
                <Trigger Property="WindowState" Value="Maximized">
                    <Setter TargetName="RootBorder" Property="BorderThickness" Value="8" />
                </Trigger>
            </ControlTemplate.Triggers>
        </ControlTemplate>
    </Window.Template>
    <!--�Զ�������� ����-->

    <WindowChrome.WindowChrome>
        <WindowChrome GlassFrameThickness="0 30 0 0" NonClientFrameEdges="Left,Bottom,Right" UseAeroCaptionButtons="False"/>
    </WindowChrome.WindowChrome>

    <!--��������ť�¼� ��ʼ-->
    <Window.CommandBindings>
        <CommandBinding Command="{x:Static SystemCommands.CloseWindowCommand}" CanExecute="CommandBinding_OnCanExecute" Executed="CommandBinding_OnExecuted_CloseWindow"/>
        <CommandBinding Command="{x:Static SystemCommands.MaximizeWindowCommand}" CanExecute="CommandBinding_OnCanExecute" Executed="CommandBinding_OnExecuted_MaximizeWindow"/>
        <CommandBinding Command="{x:Static SystemCommands.MinimizeWindowCommand}" CanExecute="CommandBinding_OnCanExecute" Executed="CommandBinding_OnExecuted_MinimizeWindow"/>
        <CommandBinding Command="{x:Static SystemCommands.RestoreWindowCommand}" CanExecute="CommandBinding_OnCanExecute" Executed="CommandBinding_OnExecuted_RestoreWindow"/>
    </Window.CommandBindings>
    <!--��������ť�¼� ����-->


    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"></RowDefinition>
            <RowDefinition Height="*"></RowDefinition>
        </Grid.RowDefinitions>
        <!--������ ��ʼ-->
        <Border Grid.Row="0" Height="30" Margin="0 -29 0 0">
            <DockPanel Background="White">
                <StackPanel Orientation="Horizontal" DockPanel.Dock="Right">
                    <Button 
                        Name="ButtonMinimizeWindow" 
                        WindowChrome.IsHitTestVisibleInChrome="True"
                        FontFamily="{StaticResource IconFont}"
                        Width="32"
                        Content="&#xe629;"
                        ToolTip="��С��" 
                        Command="{x:Static SystemCommands.MinimizeWindowCommand}"></Button>
                    <Button
                        Name="ButtonMaximizeWindow" 
                        WindowChrome.IsHitTestVisibleInChrome="True"
                        FontFamily="{StaticResource IconFont}"
                        Width="32"
                        Content="&#xe653;"
                        ToolTip="���" 
                        Command="{x:Static SystemCommands.MaximizeWindowCommand}"></Button>
                    <Button 
                        Name="ButtonRestoreWindow" 
                        WindowChrome.IsHitTestVisibleInChrome="True"
                        FontFamily="{StaticResource IconFont}"
                        Width="32"
                        Content="&#xe677;"
                        ToolTip="���»�ԭ" 
                        Visibility="Collapsed"
                        Command="{x:Static SystemCommands.RestoreWindowCommand}"></Button>
                    <Button 
                        Name="ButtonCloseWindow"
                        WindowChrome.IsHitTestVisibleInChrome="True"
                        FontFamily="{StaticResource IconFont}"
                        Width="32"
                        Content="&#xeaf2;"
                        ToolTip="�ر�" 
                        Command="{x:Static SystemCommands.CloseWindowCommand}"></Button>
                </StackPanel>
                <StackPanel Orientation="Horizontal">
                    <TextBlock 
                        Text="�Զ��������" 
                        VerticalAlignment="Center"></TextBlock>
                </StackPanel>
            </DockPanel>
        </Border>
        <!--������ ����-->
        <Grid Grid.Row="1">
            <TextBlock Text="��������"></TextBlock>
        </Grid>
    </Grid>
</Window>
```

### 3.2 C\#

```csharp
using System;
using System.Windows;
using System.Windows.Input;

namespace WindowChromeExample
{
    /// <summary>
    /// Interaction logic for MainWindow.xaml
    /// </summary>
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            StateChanged += MainWindow_StateChanged;
        }

        private void MainWindow_StateChanged(object? sender, EventArgs e)
        {
            if (WindowState == WindowState.Maximized)
            {
                ButtonRestoreWindow.Visibility = Visibility.Visible;
                ButtonMaximizeWindow.Visibility = Visibility.Collapsed;
            }
            else
            {
                ButtonRestoreWindow.Visibility = Visibility.Collapsed;
                ButtonMaximizeWindow.Visibility = Visibility.Visible;
            }
        }

        private void CommandBinding_OnCanExecute(object sender, CanExecuteRoutedEventArgs e)
        {
            e.CanExecute = true;
        }

        private void CommandBinding_OnExecuted_MinimizeWindow(object sender, ExecutedRoutedEventArgs e)
        {
            SystemCommands.MinimizeWindow(this);
        }

        private void CommandBinding_OnExecuted_MaximizeWindow(object sender, ExecutedRoutedEventArgs e)
        {
            SystemCommands.MaximizeWindow(this);
        }

        private void CommandBinding_OnExecuted_RestoreWindow(object sender, ExecutedRoutedEventArgs e)
        {
            SystemCommands.RestoreWindow(this);
        }

        private void CommandBinding_OnExecuted_CloseWindow(object sender, ExecutedRoutedEventArgs e)
        {
            SystemCommands.CloseWindow(this);
        }
    }
}
```

### 3.3 ����Ч��

<figure><img src="https://cdn.developerastrid.com/img/202202271523810.png" alt="����Ч��" class="img-fluid mx-auto d-block figure-img"><figcaption class="figure-caption text-center">
            <p>����Ч��</p>
        </figcaption>
</figure>

## Դ���ַ

������Ѿ������Ľ������� WPF �����ʹ�� WindowChrome �Զ����������

Դ���ַ��[https://github.com/astrid9527/WpfWindowChromeExample](https://github.com/astrid9527/WpfWindowChromeExample)

## �ο�

- [https://docs.microsoft.com/zh-cn/dotnet/api/system.windows.shell.windowchrome?view=net-6.0](https://docs.microsoft.com/zh-cn/dotnet/api/system.windows.shell.windowchrome?view=net-6.0)

- [https://blog.walterlv.com/post/wpf-simulate-native-window-style-using-window-chrome.html](https://blog.walterlv.com/post/wpf-simulate-native-window-style-using-window-chrome.html)

- [https://www.cnblogs.com/dino623/p/customwindowstyle.html](https://www.cnblogs.com/dino623/p/customwindowstyle.html)

- [https://www.codeproject.com/Articles/5255192/Use-WindowChrome-to-Customize-the-Title-Bar-in-WPF](https://www.codeproject.com/Articles/5255192/Use-WindowChrome-to-Customize-the-Title-Bar-in-WPF)


���꣩