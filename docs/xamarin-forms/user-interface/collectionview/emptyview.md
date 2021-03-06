---
title: データが使用できない場合に表示される、EmptyView
description: Collectionview は、表示できるデータがない場合、ユーザーにフィードバックを提供する空のビューを指定できます。 空のビューには、文字列、ビュー、または複数のビューができます。
ms.prod: xamarin
ms.assetid: 6CEBCFE6-5577-4F68-9709-431062609153
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/19/2019
ms.openlocfilehash: a430387bba83887045e5687c99d9295d4be373e4
ms.sourcegitcommit: 5d4e6677224971e2bc0268f405d192d0358c74b8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/21/2019
ms.locfileid: "58329977"
---
# <a name="display-an-emptyview-when-data-is-unavailable"></a>データが使用できない場合に表示される、EmptyView

![[プレビュー]](~/media/shared/preview.png)

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)

> [!IMPORTANT]
> `CollectionView`は現在プレビュー段階で、その計画的な機能の一部が不足しています。 さらに、実装が完了すると、API を変更することがあります。

`CollectionView` 表示するデータがない場合は、ユーザーからのフィードバックを提供するために使用できる次のプロパティを定義します。

- `EmptyView`、型の`object`、文字列、バインド、または表示されるときに表示される、`ItemsSource`プロパティが`null`、によって、コレクションが指定されている場合、または、`ItemsSource`プロパティが`null`または空です。 既定値は `null` です。
- `EmptyViewTemplate`、型の[ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)、テンプレートを使用して、指定した書式を`EmptyView`します。 既定値は `null` です。

これらのプロパティが支え[ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty)オブジェクトで、このプロパティはデータ バインドの対象であることを意味します。

設定の主な使用シナリオ、`EmptyView`プロパティでフィルター処理時にユーザーからのフィードバックが表示されて、`CollectionView`増えませんデータ、およびユーザーのフィードバックを表示する web サービスからデータを取得するときにします。

> [!NOTE]
> `EmptyView`プロパティは、必要な場合は、対話型コンテンツを含むビューに設定することができます。

データ テンプレートについて詳しくは「[Xamarin.Forms Data Templates](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)」(Xamarin.Forms のデータ テンプレート) をご覧ください。

## <a name="display-a-string-when-data-is-unavailable"></a>データが利用できない場合は、文字列を表示します。

`EmptyView`があると、文字列にプロパティを設定できるときに表示される、`ItemsSource`プロパティは`null`、によって、コレクションが指定されている場合、または、`ItemsSource`プロパティは`null`または空です。 次の XAML は、このシナリオの例を示します。

```xaml
<CollectionView ItemsSource="{Binding EmptyMonkeys}"
                EmptyView="No items to display" />
```

同等の C# コードに示します。

```csharp
CollectionView collectionView = new CollectionView
{
    EmptyView = "No items to display"
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "EmptyMonkeys");
```

結果は、コレクションがデータにバインドされているために、 `null`、として、文字列の設定、`EmptyView`プロパティの値が表示されます。

[![IOS と Android でテキスト空のビューを垂直方向の CollectionView 一覧のスクリーン ショット](emptyview-images/null-itemssource.png "CollectionView 空のテキスト ビューを垂直方向に一覧")](emptyview-images/null-itemssource-large.png#lightbox "CollectionView 空のテキストを垂直方向に一覧ビュー")

## <a name="display-views-when-data-is-unavailable"></a>データが利用できない場合は、ビューを表示します。

`EmptyView`となる、ビューにプロパティを設定できるときに表示される、`ItemsSource`プロパティは`null`、によって、コレクションが指定されている場合、または、`ItemsSource`プロパティは`null`または空です。 これには、1 つのビューまたは複数の子ビューを含むビューを指定できます。 次の XAML の例は、`EmptyView`プロパティを複数の子ビューを含むビューに設定します。

```xaml
<StackLayout Margin="20">
    <SearchBar x:Name="searchBar"
               SearchCommand="{Binding FilterCommand}"
               SearchCommandParameter="{Binding Source={x:Reference searchBar}, Path=Text}"
               Placeholder="Filter" />
    <CollectionView ItemsSource="{Binding Monkeys}">
        <CollectionView.ItemTemplate>
            <DataTemplate>
                ...
            </DataTemplate>
        </CollectionView.ItemTemplate>
        <CollectionView.EmptyView>
            <StackLayout>
                <Label Text="No results matched your filter."
                       Margin="10,25,10,10"
                       FontAttributes="Bold"
                       FontSize="18"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
                <Label Text="Try a broader filter?"
                       FontAttributes="Italic"
                       FontSize="12"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
            </StackLayout>
        </CollectionView.EmptyView>
    </CollectionView>
</StackLayout>
```

同等の C# コードに示します。

```csharp
SearchBar searchBar = new SearchBar { ... };
CollectionView collectionView = new CollectionView
{
    EmptyView = new StackLayout
    {
        Children =
        {
            new Label { Text = "No results matched your filter.", ... },
            new Label { Text = "Try a broader filter?", ... }
        }
    }
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

ときに、 [ `SearchBar` ](xref:Xamarin.Forms.SearchBar)実行、 `FilterCommand`、によって表示されるコレクション、`CollectionView`に格納されている検索用語のフィルター処理されて、 [ `SearchBar.Text` ](xref:Xamarin.Forms.SearchBar.Text)プロパティ。 フィルター処理の結果、データがない場合、 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)として設定、`EmptyView`プロパティの値が表示されます。

[![IOS と Android で、カスタムの空のビューを垂直方向の CollectionView 一覧のスクリーン ショット](emptyview-images/filter-multiple-views.png "CollectionView 空のカスタム ビューを垂直方向に一覧")](emptyview-images/filter-multiple-views-large.png#lightbox "CollectionView カスタムと垂直方向に一覧空のビュー")

## <a name="display-a-templated-custom-type-when-data-is-unavailable"></a>データが利用できない場合は、テンプレート化されたカスタム型を表示します。

`EmptyView`プロパティは、テンプレートがあるは、カスタムの型に設定することができるときに表示される、`ItemsSource`プロパティは`null`、によって、コレクションが指定されている場合、または、`ItemsSource`プロパティは`null`または空です。 `EmptyViewTemplate`にプロパティを設定することができます、 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)の外観を定義する、`EmptyView`します。 次の XAML は、このシナリオの例を示します。

```xaml
<StackLayout Margin="20">
    <SearchBar x:Name="searchBar"
               SearchCommand="{Binding FilterCommand}"
               SearchCommandParameter="{Binding Source={x:Reference searchBar}, Path=Text}"
               Placeholder="Filter" />
    <CollectionView ItemsSource="{Binding Monkeys}">
        <CollectionView.ItemTemplate>
            <DataTemplate>
                ...
            </DataTemplate>
        </CollectionView.ItemTemplate>
        <CollectionView.EmptyView>
            <views:FilterData Filter="{Binding Source={x:Reference searchBar}, Path=Text}" />
        </CollectionView.EmptyView>
        <CollectionView.EmptyViewTemplate>
            <DataTemplate>
                <Label Text="{Binding Filter, StringFormat='Your filter term of {0} did not match any records.'}"
                       Margin="10,25,10,10"
                       FontAttributes="Bold"
                       FontSize="18"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
            </DataTemplate>
        </CollectionView.EmptyViewTemplate>
    </CollectionView>
</StackLayout>
```

同等の C# コードに示します。

```csharp
SearchBar searchBar = new SearchBar { ... };
CollectionView collectionView = new CollectionView
{
    EmptyView = new FilterData { Filter = searchBar.Text },
    EmptyViewTemplate = new DataTemplate(() =>
    {
        return new Label { ... };
    })
};
```

`FilterData`型を定義、`Filter`プロパティ、および対応する[ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty):

```csharp
public class FilterData : BindableObject
{
    public static readonly BindableProperty FilterProperty = BindableProperty.Create(nameof(Filter), typeof(string), typeof(FilterData), null);

    public string Filter
    {
        get { return (string)GetValue(FilterProperty); }
        set { SetValue(FilterProperty, value); }
    }
}
```

`EmptyView`プロパティに設定されて、`FilterData`オブジェクト、および`Filter`プロパティ データにバインド、 [ `SearchBar.Text` ](xref:Xamarin.Forms.SearchBar.Text)プロパティ。 ときに、 [ `SearchBar` ](xref:Xamarin.Forms.SearchBar)実行、 `FilterCommand`、によって表示されるコレクション、`CollectionView`に格納されている検索用語のフィルター処理されて、`Filter`プロパティ。 フィルター処理の結果、データがない場合、 [ `Label` ](xref:Xamarin.Forms.Label)で定義されている、 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)、として設定されている、`EmptyViewTemplate`プロパティの値が表示されます。

[![IOS と Android での空のビュー テンプレートを使用した CollectionView 垂直方向の一覧のスクリーン ショット](emptyview-images/emptyviewtemplate.png "CollectionView 垂直方向に空のビュー テンプレートを使用した一覧")](emptyview-images/emptyviewtemplate-large.png#lightbox "CollectionView を垂直方向に一覧空のビュー テンプレート")

> [!NOTE]
> データが使用できないときに、テンプレート化されたカスタム型を表示するときに、`EmptyViewTemplate`プロパティは、複数の子ビューを含むビューに設定することができます。

## <a name="choose-an-emptyview-at-runtime"></a>実行時に、EmptyView を選択します。

ビューとして表示される、`EmptyView`データが利用できない場合は、として定義できます[ `ContentView` ](xref:Xamarin.Forms.ContentView)内のオブジェクトを[ `ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)します。 `EmptyView`に特定のプロパティを設定できます`ContentView`実行時に、いくつかのビジネス ロジックに基づいて、します。 次の XAML の例では、このシナリオの例を示します。

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="CollectionViewDemos.Views.EmptyViewSwapPage"
             Title="EmptyView (swap)">
    <ContentPage.Resources>
        <ContentView x:Key="BasicEmptyView">
            <StackLayout>
                <Label Text="No items to display."
                       Margin="10,25,10,10"
                       FontAttributes="Bold"
                       FontSize="18"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
            </StackLayout>
        </ContentView>
        <ContentView x:Key="AdvancedEmptyView">
            <StackLayout>
                <Label Text="No results matched your filter."
                       Margin="10,25,10,10"
                       FontAttributes="Bold"
                       FontSize="18"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
                <Label Text="Try a broader filter?"
                       FontAttributes="Italic"
                       FontSize="12"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
            </StackLayout>
        </ContentView>
    </ContentPage.Resources>

    <StackLayout Margin="20">
        <SearchBar x:Name="searchBar"
                   SearchCommand="{Binding FilterCommand}"
                   SearchCommandParameter="{Binding Source={x:Reference searchBar}, Path=Text}"
                   Placeholder="Filter" />
        <StackLayout Orientation="Horizontal">
            <Label Text="Toggle EmptyViews" />
            <Switch Toggled="OnEmptyViewSwitchToggled" />
        </StackLayout>
        <CollectionView x:Name="collectionView"
                        ItemsSource="{Binding Monkeys}">
            <CollectionView.ItemTemplate>
                <DataTemplate>
                    ...
                </DataTemplate>
            </CollectionView.ItemTemplate>
        </CollectionView>
    </StackLayout>
</ContentPage>
```

この XAML は、2 つ定義[ `ContentView` ](xref:Xamarin.Forms.ContentView)ページ レベルのオブジェクト[ `ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)で、 [ `Switch` ](xref:Xamarin.Forms.Switch)を制御するオブジェクト`ContentView`オブジェクトが設定されると、`EmptyView`プロパティの値。 ときに、 [ `Switch` ](xref:Xamarin.Forms.Switch)が切り替えられた、`OnEmptyViewSwitchToggled`イベント ハンドラーが実行される、`ToggleEmptyView`メソッド。

```csharp
void ToggleEmptyView(bool isToggled)
{
    collectionView.EmptyView = isToggled ? Resources["BasicEmptyView"] : Resources["AdvancedEmptyView"];
}
```

`ToggleEmptyView`メソッドのセット、`EmptyView`のプロパティ、`collectionView`オブジェクト、2 つのいずれかに[ `ContentView` ](xref:Xamarin.Forms.ContentView)オブジェクトに格納されている、 [ `ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)の値に基づいて、[ `Switch.IsToggled` ](xref:Xamarin.Forms.Switch.IsToggled)プロパティ。 ときに、 [ `SearchBar` ](xref:Xamarin.Forms.SearchBar)実行、 `FilterCommand`、によって表示されるコレクション、`CollectionView`に格納されている検索用語のフィルター処理されて、 [ `SearchBar.Text` ](xref:Xamarin.Forms.SearchBar.Text)プロパティ。 フィルター処理の結果、データがない場合、`ContentView`オブジェクトのセットとして、`EmptyView`プロパティが表示されます。

[![IOS と Android でのスワップ空ビューを垂直方向の CollectionView 一覧のスクリーン ショット](emptyview-images/swap.png "CollectionView スワップの空のビューを垂直方向に一覧")](emptyview-images/swap-large.png#lightbox "CollectionView を垂直方向に一覧空のビューのスワップ")

リソース ディクショナリの詳細については、次を参照してください。 [Xamarin.Forms リソース ディクショナリ](~/xamarin-forms/xaml/resource-dictionaries.md)します。

## <a name="related-links"></a>関連リンク

- [CollectionView (サンプル)](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)
- [Xamarin.Forms データ テンプレート](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Xamarin.Forms のリソース ディクショナリ](~/xamarin-forms/xaml/resource-dictionaries.md)
