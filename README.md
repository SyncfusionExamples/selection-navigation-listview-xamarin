# How to apply ListView selected item color in Xamarin.Forms navigation?

You can apply the selection color before navigating to another page using [thread](https://docs.microsoft.com/en-us/xamarin/essentials/main-thread) in Xamarin.Forms [SfListView](https://help.syncfusion.com/xamarin/listview/overview). 

You can also refer  the following article.

https://www.syncfusion.com/kb/11684/how-to-apply-listview-selected-item-color-in-xamarin-forms-navigation-sflistview

**XAML**

Bind [SfListView.SelectionChangedCommand](https://help.syncfusion.com/cr/cref_files/xamarin/Syncfusion.SfListView.XForms~Syncfusion.ListView.XForms.SfListView~SelectionChangedCommand.html) to navigate to the next page.
``` xml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ListViewXamarin"
             xmlns:syncfusion="clr-namespace:Syncfusion.ListView.XForms;assembly=Syncfusion.SfListView.XForms"
             x:Class="ListViewXamarin.MainPage">
    <ContentPage.BindingContext>
        <local:ContactsViewModel/>
    </ContentPage.BindingContext>

	 <ContentPage.Content>
        <StackLayout>
            <syncfusion:SfListView x:Name="listView" ItemSize="60" ItemsSource="{Binding ContactsInfo}" SelectionChangedCommand="{Binding SelectionCommand}">
                <syncfusion:SfListView.ItemTemplate >
                    <DataTemplate>
                        <Grid x:Name="grid">
                            <Grid.ColumnDefinitions>
                                <ColumnDefinition Width="70" />
                                <ColumnDefinition Width="*" />
                            </Grid.ColumnDefinitions>
                            <Image Source="{Binding ContactImage}" VerticalOptions="Center" HorizontalOptions="Center" HeightRequest="50" WidthRequest="50"/>
                            <Grid Grid.Column="1" RowSpacing="1" Padding="10,0,0,0" VerticalOptions="Center">
                                <Label LineBreakMode="NoWrap" TextColor="#474747" Text="{Binding ContactName}"/>
                                <Label Grid.Row="1" Grid.Column="0" TextColor="#474747" LineBreakMode="NoWrap" Text="{Binding ContactNumber}"/>
                            </Grid>
                        </Grid>
                    </DataTemplate>
                </syncfusion:SfListView.ItemTemplate>
            </syncfusion:SfListView>
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```
**C#**

In the **SelectionChangedCommand**, using **MainThread** to navigate to another page.
``` c#
public class ContactsViewModel : INotifyPropertyChanged
{
    public ObservableCollection<Contacts> ContactsInfo { get; set; }
    public Command<object> SelectionCommand { get; set; }

    public ContactsViewModel()
    {
        ContactsInfo = new ObservableCollection<Contacts>();
        SelectionCommand = new Command<object>(OnItemSelected);
        GenerateInfo();
    }

    private void OnItemSelected(object obj)
    {
        var selectedItem = (obj as Syncfusion.ListView.XForms.ItemSelectionChangedEventArgs).AddedItems[0] as Contacts;
        var newPage = new NewPage();
        newPage.BindingContext = selectedItem;

        Device.BeginInvokeOnMainThread(async () =>
        {
            await Task.Delay(200);
            await App.Current.MainPage.Navigation.PushAsync(newPage);
        });
    }
}
```
**Output**
 
![SelectionNavigation](https://github.com/SyncfusionExamples/selection-navigation-listview-xamarin/blob/master/ScreenShot/SelectionNavigation.gif)
