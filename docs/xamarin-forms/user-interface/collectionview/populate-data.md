---
title: データ CollectionView を Xamarin.Forms を設定します。
description: CollectionView に IEnumerable を実装する任意のコレクションをその ItemsSource プロパティを設定して、データを設定します。
ms.prod: xamarin
ms.assetid: E1783E34-1C0F-401A-80D5-B2BE5508F5F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/15/2019
ms.openlocfilehash: 70b241944376782ec4c9446878ee2a19dcee2bbd
ms.sourcegitcommit: 5d4e6677224971e2bc0268f405d192d0358c74b8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/21/2019
ms.locfileid: "58329939"
---
# <a name="populate-xamarinforms-collectionview-with-data"></a>データ CollectionView を Xamarin.Forms を設定します。

![[プレビュー]](~/media/shared/preview.png)

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)

> [!IMPORTANT]
> `CollectionView`は現在プレビュー段階で、その計画的な機能の一部が不足しています。 さらに、実装が完了すると、API を変更することがあります。

`CollectionView` 表示されるデータを定義する次のプロパティとその外観を定義します。

- `ItemsSource`、型の`IEnumerable`、表示される項目のコレクションを指定の既定値を持つと`null`します。
- `ItemTemplate`、型の[ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)、表示する項目のコレクション内の各項目に適用するテンプレートを指定します。

これらのプロパティが支え[ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty)オブジェクトで、このプロパティはデータ バインドの対象であることを意味します。

## <a name="populate-a-collectionview-with-data"></a>データの CollectionView を設定します。

A`CollectionView`設定によってデータが読み込まれて、`ItemsSource`プロパティを実装するコレクションを`IEnumerable`します。 初期化することにより XAML で項目を追加できる、`ItemsSource`文字列の配列からのプロパティ。

```xaml
<CollectionView>
  <CollectionView.ItemsSource>
    <x:Array Type="{x:Type x:String}">
      <x:String>Baboon</x:String>
      <x:String>Capuchin Monkey</x:String>
      <x:String>Blue Monkey</x:String>
      <x:String>Squirrel Monkey</x:String>
      <x:String>Golden Lion Tamarin</x:String>
      <x:String>Howler Monkey</x:String>
      <x:String>Japanese Macaque</x:String>
    </x:Array>
  </CollectionView.ItemsSource>
</CollectionView>
```

> [!NOTE]
> `x:Array` 要素には、配列内の項目の型を示す `Type` 属性が必要です。

同等の C# コードに示します。

```csharp
CollectionView collectionView = new CollectionView();
collectionView.ItemsSource = new string[]
{
    "Baboon",
    "Capuchin Monkey",
    "Blue Monkey",
    "Squirrel Monkey",
    "Golden Lion Tamarin",
    "Howler Monkey",
    "Japanese Macaque"
};
```

> [!IMPORTANT]
> 場合、`CollectionView`は項目の追加、削除、または基になるコレクションで変更されても更新するために必要な基になるコレクションである必要があります、`IEnumerable`プロパティを送信するコレクションの変更通知、`ObservableCollection`します。

既定では、`CollectionView`次のスクリーン ショットに示すように垂直方向に一覧項目が表示されます。

[![IOS と Android でのテキスト アイテムを格納しているスクリーン ショットの CollectionView](populate-data-images/text.png "collectionview テキスト アイテム")](populate-data-images/text-large.png#lightbox "collectionview テキスト アイテム")

変更する方法については、`CollectionView`レイアウトを参照してください[レイアウトを指定](layout.md)します。 内の各項目の外観を定義する方法については、`CollectionView`を参照してください[リスト項目の外観を定義する](#define-list-item-appearance)します。

### <a name="data-binding"></a>データ バインディング

`CollectionView` バインドするデータ バインディングを使用してデータを設定することができます、`ItemsSource`プロパティを`IEnumerable`コレクション。 XAML でこれは、`Binding`マークアップ拡張機能。

```xaml
<CollectionView ItemsSource="{Binding Monkeys}" />
```

同等の C# コードに示します。

```csharp
CollectionView collectionView = new CollectionView();
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

この例で、`ItemsSource`プロパティ データにバインド、`Monkeys`接続されているビュー モデルのプロパティ。

> [!NOTE]
> Xamarin.Forms アプリケーションのデータ バインディングのパフォーマンスを向上させるためには、コンパイル済みのバインドを有効にすることができます。 詳細については、次を参照してください。[コンパイル バインド](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md)します。

データ バインディングの詳細については、「[Xamarin.Forms Data Binding](~/xamarin-forms/app-fundamentals/data-binding/index.md)」 (Xamarin.Forms のデータ バインディング) を参照してください。

## <a name="define-item-appearance"></a>項目の外観を定義します。

内の各項目の外観、`CollectionView`を設定して定義することができます、`CollectionView.ItemTemplate`プロパティを[ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate):

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.RowDefinitions>
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="Auto" />
                </Grid.ColumnDefinitions>
                <Image Grid.RowSpan="2"
                       Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="60"
                       WidthRequest="60" />
                <Label Grid.Column="1"
                       Text="{Binding Name}"
                       FontAttributes="Bold" />
                <Label Grid.Row="1"
                       Grid.Column="1"
                       Text="{Binding Location}"
                       FontAttributes="Italic"
                       VerticalOptions="End" />
            </Grid>
        </DataTemplate>
    </CollectionView.ItemTemplate>
    ...
</CollectionView>
```

同等の C# コードに示します。

```csharp
CollectionView collectionView = new CollectionView();
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");

collectionView.ItemTemplate = new DataTemplate(() =>
{
    Grid grid = new Grid { Padding = 10 };
    grid.RowDefinitions.Add(new RowDefinition { Height = GridLength.Auto });
    grid.RowDefinitions.Add(new RowDefinition { Height = GridLength.Auto });
    grid.ColumnDefinitions.Add(new ColumnDefinition { Width = GridLength.Auto });
    grid.ColumnDefinitions.Add(new ColumnDefinition { Width = GridLength.Auto });

    Image image = new Image { Aspect = Aspect.AspectFill, HeightRequest = 60, WidthRequest = 60 };
    image.SetBinding(Image.SourceProperty, "ImageUrl");

    Label nameLabel = new Label { FontAttributes = FontAttributes.Bold };
    nameLabel.SetBinding(Label.TextProperty, "Name");

    Label locationLabel = new Label { FontAttributes = FontAttributes.Italic, VerticalOptions = LayoutOptions.End };
    locationLabel.SetBinding(Label.TextProperty, "Location");

    Grid.SetRowSpan(image, 2);

    grid.Children.Add(image);
    grid.Children.Add(nameLabel, 1, 0);
    grid.Children.Add(locationLabel, 1, 1);

    return grid;
});
```

指定した要素、 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)一覧の各項目の外観を定義します。 内のレイアウトの例で、`DataTemplate`によって管理される、 [ `Grid`](xref:Xamarin.Forms.Grid)します。 `Grid`が含まれています、 [ `Image` ](xref:Xamarin.Forms.Image)オブジェクト、および 2 [ `Label` ](xref:Xamarin.Forms.Label)オブジェクトの場合、すべてのプロパティにバインドする、`Monkey`クラス。

```csharp
public class Monkey
{
    public string Name { get; set; }
    public string Location { get; set; }
    public string Details { get; set; }
    public string ImageUrl { get; set; }
}
```

次のスクリーン ショットは、一覧に各項目のテンプレートの結果を表示します。

[![各項目が、iOS と Android で template 宣言されるスクリーン ショットの CollectionView](populate-data-images/datatemplate.png "collectionview テンプレート化された項目")](populate-data-images/datatemplate-large.png#lightbox "collectionview テンプレート化された項目")

データ テンプレートについて詳しくは「[Xamarin.Forms Data Templates](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)」(Xamarin.Forms のデータ テンプレート) をご覧ください。

## <a name="related-links"></a>関連リンク

- [CollectionView (サンプル)](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)
- [Xamarin.Forms のデータ バインディング](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [Xamarin.Forms データ テンプレート](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
