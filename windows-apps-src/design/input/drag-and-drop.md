---
description: 本文說明如何在您的 Windows 應用程式中新增拖放功能。
title: 拖放
ms.assetid: A15ED2F5-1649-4601-A761-0F6C707A8B7E
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9a0005ecf7d51cc6b08bc5cc61350489839d568f
ms.sourcegitcommit: 047004e2bf100e319d134c18518062bf7f3efb5d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/27/2020
ms.locfileid: "92763102"
---
# <a name="drag-and-drop"></a>拖放

拖放是在 Windows 桌面上於應用程式中或應用程式間傳輸資料的直覺方式。 拖放可讓使用者使用標準手勢 (使用手指按住不放然後平移，或使用滑鼠或手寫筆按住然後平移) 在應用程式間或在應用程式內傳輸資料。

> **重要 API** ： [CanDrag 屬性](/uwp/api/windows.ui.xaml.uielement.candrag)、 [AllowDrop 屬性](/uwp/api/windows.ui.xaml.uielement.allowdrop) 

拖曳來源 (即觸發拖曳手勢的應用程式或區域) 可藉由填入可包含標準資料格式，包含文字、RTF、HTML、點陣圖、儲存項目或自訂資料格式的資料套件物件，來提供要傳送的資料。 來源同時也指出其支援的作業類型：複製、移動或連結。 當指標釋放時，便會發生置放。 置放目標，即指標底下的應用程式或區域，會處理資料套件，然後傳回執行的作業類型。

在拖放過程中，拖曳 UI 會提供正在發生的拖放作業類型的視覺指示。 這項視覺回饋一開始是由來源提供的，但可在指標移動到目標時，由目標進行變更。

所有支援 UWP 的裝置都支援現代化的拖放。 它允許任何類型應用程式間或應用程式內的資料傳輸，包含傳統型 Windows 應用程式，雖然本文聚焦於現代化拖放的 XAML API。 實作之後，拖放不論以哪一個方向都能順暢運作，包括應用程式間、應用程式到傳統型應用程式，以及傳統型應用程式到應用程式。

以下是在您的應用程式中啟用拖放所需要進行之作業的概觀：

1. 將元素的 **CanDrag** 屬性設為 true 以啟用拖曳。  
2. 建置資料套件。 系統會自動處理影像和文字，但針對其他內容，您將需要處理 **DragStarted** 和 **DragCompleted** 事件，然後使用他們建構您自己的資料套件。 
3. 藉由將 **AllowDrop** 屬性設為 **true** ，來在所有能接收置放內容的元素上啟用置放。 
4. 處理 **DragOver** 事件，讓系統知道元素可接收什麼類型的置放作業。 
5. 處理 **Drop** 事件，以接收置放內容。 



## <a name="enable-dragging"></a>啟用拖曳

若要在元素上啟用拖曳，請將其 [**CanDrag**](/uwp/api/windows.ui.xaml.uielement.candrag) 屬性設為 **true** 。 這會使元素（以及它所包含的專案）在可拖曳的集合（例如 ListView）的情況下成為可拖曳的專案。

指定可拖曳的內容。 使用者不會想要拖曳您應用程式中的所有東西，通常只有特定項目，例如影像或文字。 

以下是如何設定 [**CanDrag**](/uwp/api/windows.ui.xaml.uielement.candrag) 的方式。

:::code language="xml" source="~/../snippets-windows/windows-uwp/design/input/drag_drop/cs/MainPage.xaml" id="SnippetDragArea":::

您不需要進行任何其他工作即可允許拖曳，除非您希望自訂 UI (本文章稍後將會說明)。 置放則需要較多的步驟。

## <a name="construct-a-data-package"></a>建構資料套件 

在大部分的案例中，系統會為您建構資料套件。 系統會自動處理：
* 影像
* Text 

針對其他內容，您將需要處理 **DragStarted** 和 **DragCompleted** 事件，然後使用他們建構您自己的 [DataPackage](/uwp/api/windows.applicationmodel.datatransfer.datapackage)。

## <a name="enable-dropping"></a>啟用置放

下列標記示範如何使用 XAML 中的 [**AllowDrop**](/uwp/api/windows.ui.xaml.uielement.allowdrop) 將應用程式的特定區域設為有效的置放區域。 如果使用者嘗試在其他地方放下，系統將不會允許這麼做。 如果您希望使用者可以在 app 中的任何位置放下項目，請將整個背景設定為放下目標。

:::code language="xml" source="~/../snippets-windows/windows-uwp/design/input/drag_drop/cs/MainPage.xaml" id="SnippetDropArea":::


## <a name="handle-the-dragover-event"></a>處理 DragOver 事件

當使用者拖曳項目到您的 App 上，但尚未放下時，會引發 [**DragOver**](/uwp/api/windows.ui.xaml.uielement.dragover) 事件。 在這個處理常式中，您需要使用 [**AcceptedOperation**](/uwp/api/windows.ui.xaml.drageventargs.acceptedoperation) 屬性指定 app 支援何種類型的作業。 最常見的是複製。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/design/input/drag_drop/cs/MainPage.xaml.cs" id="SnippetGrid_DragOver":::

## <a name="process-the-drop-event"></a>處理 Drop 事件

當使用者在有效的放下區域釋放項目時會發生 [**Drop**](/uwp/api/windows.ui.xaml.uielement.drop) 事件。 請使用 [**DataView**](/uwp/api/windows.ui.xaml.drageventargs.dataview) 屬性處理它們。

為了簡單起見，下面的範例假設使用者置放單一相片並直接存取。 事實上，使用者可以同時置放多個不同格式的項目。 您的應用程式應該檢查置放了什麼類型的檔案，以及其中有多少檔案，以便處理這種可能情況，然後相應處理每個項目。 您也應該考慮通知使用者是否要嘗試執行您的應用程式不支援的動作。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/design/input/drag_drop/cs/MainPage.xaml.cs" id="SnippetGrid_Drop":::

## <a name="customize-the-ui"></a>自訂 UI

系統會提供用於拖放的預設 UI。 不過，您也可以選擇透過設定自訂標題與字符來自訂 UI 的各個部分，或選擇完全不顯示 UI。 若要自訂 UI，請使用 [**DragEventArgs.DragUIOverride**](/uwp/api/windows.ui.xaml.drageventargs.draguioverride) 屬性。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/design/input/drag_drop/cs/MainPage.xaml.cs" id="SnippetGrid_DragOverCustom":::

## <a name="open-a-context-menu-on-an-item-you-can-drag-with-touch"></a>在您可以使用觸控方式拖曳的項目上開啟操作功能表

使用觸控時，拖曳 [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) 和開啟其操作功能表會共用類似的觸控手勢；每個手勢都是以長按做為開始。 以下說明系統如何釐清您 app 中同時支援以下兩個項目之元素的兩個動作： 

* 如果使用者長按某個項目並開始在 500 毫秒內拖曳它，就會拖曳該項目且不會顯示操作功能表。 
* 如果使用者長按該項目但沒有在 500 毫秒內拖曳它，就會開啟操作功能表。 
* 在操作功能表開啟之後，如果使用者嘗試拖曳項目 (手指沒有舉起離開螢幕)，就會關閉操作功能表並開始拖曳。

## <a name="designate-an-item-in-a-listview-or-gridview-as-a-folder"></a>將 ListView 或 GridView 中的項目指定為資料夾

您可以指定 [**ListViewItem**](/uwp/api/Windows.UI.Xaml.Controls.ListViewItem) 或 [**GridViewItem**](/uwp/api/Windows.UI.Xaml.Controls.GridViewItem) 做為資料夾。 對於樹狀檢視和檔案總管情況而言，這特別有用。 若要這樣做，請將該項目上的 [**AllowDrop**](/uwp/api/windows.ui.xaml.uielement.allowdrop) 屬性明確設為 **True** 。 

系統將會針對拖曳到資料夾中和非資料夾項目，自動顯示適當的動畫。 您的 app 程式碼必須在資料夾 (以及非資料夾項目) 上繼續處理 [**Drop**](/uwp/api/windows.ui.xaml.uielement.drop) 事件，以更新料來源並將放下的項目新增到目標資料夾。

## <a name="enable-drag-and-drop-reordering-within-listviews"></a>在 Listview 內啟用拖放重新排列

[**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView)可使用與本文所述的 **CanDrop** API 非常類似的 api，來支援以拖曳為基礎的重新排列。 您至少要加入 **system.windows.uielement.allowdrop** 和 **CanReorderItems** 屬性。

如需詳細資訊，請參閱 [**ListViewBase CanReorderItems**](/uwp/api/windows.ui.xaml.controls.listviewbase.canreorderitems) 。

## <a name="implementing-custom-drag-and-drop"></a>實作自訂拖放

[UIElement](/uwp/api/windows.ui.xaml.uielement) 類別會為您完成實作拖放的大部分工作。 但是，如果您想要的話，可以使用 [ApplicationModel. DataTransfer. system.windows.dragdrop.drop>.... Core 命名空間](/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core)中的 api 來執行您自己的版本。

| 功能 | WinRT API |
| --- | --- |
|  啟用拖曳 | [CoreDragOperation](/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core.coredragoperation)  |
|  建立資料套件 | [DataPackage](/uwp/api/windows.applicationmodel.datatransfer.datapackage)  |
| 將拖曳交給殼層  | [CoreDragOperation.StartAsync](/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core.coredragoperation)  |
| 從殼層接收置放  | [CoreDragDropManager](/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core.coredragdropmanager)<br/>[ICoreDropOperationTarget](/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core.icoredropoperationtarget)    |



## <a name="see-also"></a>請參閱

* [應用程式間通訊](index.md)
* [AllowDrop](/uwp/api/windows.ui.xaml.uielement.allowdrop)
* [CanDrag](/uwp/api/windows.ui.xaml.uielement.candrag)
* [DragOver](/uwp/api/windows.ui.xaml.uielement.dragover)
* [AcceptedOperation](/uwp/api/windows.ui.xaml.drageventargs.acceptedoperation)
* [DataView](/uwp/api/windows.ui.xaml.drageventargs.dataview)
* [DragUIOverride](/uwp/api/windows.ui.xaml.drageventargs.draguioverride)
* [下降](/uwp/api/windows.ui.xaml.uielement.drop)
* [IsDragSource](/uwp/api/windows.ui.xaml.controls.listviewbase.isdragsource)
