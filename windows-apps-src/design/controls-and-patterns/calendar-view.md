---
description: 行事曆檢視可讓使用者檢視行事曆並與其互動，以便依月份、年份或 10 年瀏覽行事曆。
title: 行事曆檢視
ms.assetid: d8ec5ba8-7a9d-405d-a1a5-5a1b502b9e64
label: Calendar view
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: f26f2dce4847587869b35c7f04f0da2e45e3c0e2
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030951"
---
# <a name="calendar-view"></a>行事曆檢視

行事曆檢視可讓使用者檢視行事曆並與其互動，以便依月份、年份或 10 年瀏覽行事曆。 使用者可以選取單一日期或日期範圍。 它沒有選擇器介面，而且行事曆一律會顯示。

**取得 Windows UI 程式庫**

:::row:::
   :::column:::
      ![WinUI 標誌](images/winui-logo-64x64.png)
   :::column-end:::
   :::column span="3":::
      Windows UI 程式庫 2.2 或更新版本中有這個控制項使用圓角的新範本。 如需詳細資訊，請參閱[圓角半徑](../style/rounded-corner.md)。 WinUI 是 NuGet 套件，其中包含適用於 Windows 應用程式的新控制項和 UI 功能。 如需詳細資訊 (包括安裝指示)，請參閱 [Windows UI 程式庫](/uwp/toolkits/winui/)。
   :::column-end:::
   :::column:::

   :::column-end:::
:::row-end:::

> **平台 API：** [CalendarView 類別](/uwp/api/Windows.UI.Xaml.Controls.CalendarView)、 [SelectedDatesChanged 事件](/uwp/api/windows.ui.xaml.controls.calendarview.selecteddateschanged)

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？
您可以使用行事曆檢視，讓使用者從一律顯示的行事曆中選取單一日期或日期範圍。

如果您需要讓使用者一次選取多個日期，則必須使用行事曆檢視。 如果您需要讓使用者只挑選單一日期，而且不需要一律顯示行事曆，請考慮使用[行事曆日期選擇器](calendar-date-picker.md)或[日期選擇器](date-picker.md)控制項。

如需有關如何選擇正確控制項的詳細資訊，請參閱[日期和時間控制項](date-and-time.md)文章。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡<a href="xamlcontrolsgallery:/item/CalendarView">開啟應用程式並查看 CalendarView 運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始程式碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

行事曆檢視是由 3 個個別檢視所組成：月份檢視、年份檢視和十年份檢視。 根據預設，開啟時會顯示月份檢視。 您可以設定 [DisplayMode](/uwp/api/windows.ui.xaml.controls.calendarview.displaymode) 屬性，以指定啟動檢視。

![行事曆檢視的 3 個檢視](images/calendar-view-3-views.png)

使用者按一下月份檢視的標頭，即可開啟年份檢視；按一下年份檢視的標頭，即可開啟十年份檢視。 使用者在十年份檢視中挑選某一年，即可返回年份檢視；挑選年份檢視中的某個月份，即可返回月份檢視。 標頭兩側的箭頭可用來往前或往回瀏覽月份、年份或十年。

## <a name="create-a-calendar-view"></a>建立行事曆檢視

此範例顯示如何建立簡單的行事曆檢視。

```xaml
<CalendarView/>
```

產生的行事曆檢視看起來像這樣：

![行事曆檢視的範例](images/controls_calendar_monthview.png)

### <a name="selecting-dates"></a>選取日期

根據預設， [SelectionMode](/uwp/api/windows.ui.xaml.controls.calendarview.selectionmode) 屬性是設定為 **Single** 。 這可讓使用者在行事曆中挑選單一日期。 將 SelectionMode 設定為 **None** 以停用日期選取功能。

將 SelectionMode 設定為 **Multiple** 以讓使用者選取多個日期。 您可以將 [DateTime](/dotnet/api/system.datetime)/[DateTimeOffset](/dotnet/api/system.datetimeoffset) 物件新增至 [SelectedDates](/uwp/api/windows.ui.xaml.controls.calendarview.selecteddates)集合，以選取多個日期，如下所示︰

```csharp
calendarView1.SelectedDates.Add(DateTimeOffset.Now);
calendarView1.SelectedDates.Add(new DateTime(1977, 1, 5));
```

使用者可以按一下或點選行事曆格線，以取消選取已選取的日期。

您可以處理 [SelectedDatesChanged](/uwp/api/windows.ui.xaml.controls.calendarview.selecteddateschanged) 事件，以在 [SelectedDates](/uwp/api/windows.ui.xaml.controls.calendarview.selecteddates) 集合變更時接收通知。

> [!NOTE]
> 如需日期值的重要資訊，請參閱＜日期和時間控制項＞文章中的 [DateTime 和 Calendar 值](date-and-time.md#datetime-and-calendar-values)。

### <a name="customizing-the-calendar-views-appearance"></a>自訂行事曆檢視的外觀

行事曆檢視是由兩個部分所組成：ControlTemplate 中定義的 XAML 元素，以及控制項直接呈現的視覺元素。
- 控制項範本中定義的 XAML 元素包括控制項周圍的框線、標頭、[上一步] 和 [下一步] 按鈕以及 DayOfWeek 元素。 與任何 XAML 控制項一樣，您可以設定這些元素的樣式以及重新套用範本。
- 行事曆格線是由 [CalendarViewDayItem](/uwp/api/Windows.UI.Xaml.Controls.CalendarViewDayItem) 物件所組成。 您無法設定這些元素的樣式或重新套用範本，但我們提供不同的屬性，讓您可以自訂它們的外觀。

下圖顯示行事曆月份檢視的組成元素。 如需詳細資訊，請參閱 [CalendarViewDayItem](/uwp/api/Windows.UI.Xaml.Controls.CalendarViewDayItem) 類別的「備註」。

![行事曆月份檢視的元素](images/calendar-view-month-elements.png)

您可以變更這個表格列出的屬性，以修改行事曆元素的外觀。

元素 | [內容]
--------|-----------
DayOfWeek | [DayOfWeekFormat](/uwp/api/windows.ui.xaml.controls.calendarview.dayofweekformat)
CalendarItem | [CalendarItemBackground](/uwp/api/windows.ui.xaml.controls.calendarview.calendaritembackground)、[CalendarItemBorderBrush](/uwp/api/windows.ui.xaml.controls.calendarview.calendaritemborderbrush)、[CalendarItemBorderThickness](/uwp/api/windows.ui.xaml.controls.calendarview.calendaritemborderthickness)、[CalendarItemForeground](/uwp/api/windows.ui.xaml.controls.calendarview.calendaritemforeground)
DayItem | [DayItemFontFamily](/uwp/api/windows.ui.xaml.controls.calendarview.dayitemfontfamily)、[DayItemFontSize](/uwp/api/windows.ui.xaml.controls.calendarview.dayitemfontsize)、[DayItemFontStyle](/uwp/api/windows.ui.xaml.controls.calendarview.dayitemfontstyle)、[DayItemFontWeight](/uwp/api/windows.ui.xaml.controls.calendarview.dayitemfontweight)、[HorizontalDayItemAlignment](/uwp/api/windows.ui.xaml.controls.calendarview.horizontaldayitemalignment)、[VerticalDayItemAlignment](/uwp/api/windows.ui.xaml.controls.calendarview.verticaldayitemalignment)、[CalendarViewDayItemStyle](/uwp/api/windows.ui.xaml.controls.calendarview.calendarviewdayitemstyle)
MonthYearItem (在年份和十年份檢視中，相當於 DayItem) | [MonthYearItemFontFamily](/uwp/api/windows.ui.xaml.controls.calendarview.monthyearitemfontfamily)、[MonthYearItemFontSize](/uwp/api/windows.ui.xaml.controls.calendarview.monthyearitemfontsize)、[MonthYearItemFontStyle](/uwp/api/windows.ui.xaml.controls.calendarview.monthyearitemfontstyle)、[MonthYearItemFontWeight](/uwp/api/windows.ui.xaml.controls.calendarview.monthyearitemfontweight)
FirstOfMonthLabel | [FirstOfMonthLabelFontFamily](/uwp/api/windows.ui.xaml.controls.calendarview.firstofmonthlabelfontfamily)、[FirstOfMonthLabelFontSize](/uwp/api/windows.ui.xaml.controls.calendarview.firstofmonthlabelfontsize)、[FirstOfMonthLabelFontStyle](/uwp/api/windows.ui.xaml.controls.calendarview.firstofmonthlabelfontstyle)、[FirstOfMonthLabelFontWeight](/uwp/api/windows.ui.xaml.controls.calendarview.firstofmonthlabelfontweight)、[HorizontalFirstOfMonthLabelAlignment](/uwp/api/windows.ui.xaml.controls.calendarview.horizontalfirstofmonthlabelalignment)、[VerticalFirstOfMonthLabelAlignment](/uwp/api/windows.ui.xaml.controls.calendarview.verticalfirstofmonthlabelalignment)、[IsGroupLabelVisible](/uwp/api/windows.ui.xaml.controls.calendarview.isgrouplabelvisible)
FirstofYearDecadeLabel (在年份和十年份檢視中，相當於 FirstOfMonthLabel) | [FirstOfYearDecadeLabelFontFamily](/uwp/api/windows.ui.xaml.controls.calendarview.firstofyeardecadelabelfontfamily)、[FirstOfYearDecadeLabelFontSize](/uwp/api/windows.ui.xaml.controls.calendarview.firstofyeardecadelabelfontsize)、[FirstOfYearDecadeLabelFontStyle](/uwp/api/windows.ui.xaml.controls.calendarview.firstofyeardecadelabelfontstyle)、[FirstOfYearDecadeLabelFontWeight](/uwp/api/windows.ui.xaml.controls.calendarview.firstofyeardecadelabelfontweight)
Visual State Manager | [FocusBorderBrush](/uwp/api/windows.ui.xaml.controls.calendarview.focusborderbrush)、[HoverBorderBrush](/uwp/api/windows.ui.xaml.controls.calendarview.hoverborderbrush)、[PressedBorderBrush](/uwp/api/windows.ui.xaml.controls.calendarview.pressedborderbrush)、[SelectedBorderBrush](/uwp/api/windows.ui.xaml.controls.calendarview.selectedborderbrush)、[SelectedForeground](/uwp/api/windows.ui.xaml.controls.calendarview.selectedforeground)、[SelectedHoverBorderBrush](/uwp/api/windows.ui.xaml.controls.calendarview.selectedhoverborderbrush)、[SelectedPressedBorderBrush](/uwp/api/windows.ui.xaml.controls.calendarview.selectedpressedborderbrush)
OutofScope | [IsOutOfScopeEnabled](/uwp/api/windows.ui.xaml.controls.calendarview.isoutofscopeenabled)、[OutOfScopeBackground](/uwp/api/windows.ui.xaml.controls.calendarview.outofscopebackground)、[OutOfScopeForeground](/uwp/api/windows.ui.xaml.controls.calendarview.outofscopeforeground)
今天 | [IsTodayHighlighted](/uwp/api/windows.ui.xaml.controls.calendarview.istodayhighlighted)、[TodayFontWeight](/uwp/api/windows.ui.xaml.controls.calendarview.todayfontweight)、[TodayForeground](/uwp/api/windows.ui.xaml.controls.calendarview.todayforeground)

 根據預設，月份檢視一次會顯示 6 週。 您可以設定 [NumberOfWeeksInView](/uwp/api/windows.ui.xaml.controls.calendarview.numberofweeksinview) 屬性，以變更顯示的週數。 顯示的週數最小值是 2；最大值是 8。

根據預設，年份和十年份檢視會顯示在 4x4 方格中。 若要變更列數或欄數，請呼叫 [SetYearDecadeDisplayDimensions](/uwp/api/windows.ui.xaml.controls.calendarview.setyeardecadedisplaydimensions) 並指定您需要的列數與欄數。 這樣會變更年份和十年份檢視的方格。

在這裡，年份和十年份檢視會設定成顯示在 3x4 方格中。

```csharp
calendarView1.SetYearDecadeDisplayDimensions(3, 4);
```

根據預設，行事曆檢視中最小的顯示日期為目前日期前 100 年，最大的顯示日期為目前日期的後 100 年。 您可以設定 [MinDate](/uwp/api/windows.ui.xaml.controls.calendarview.mindate) 和 [MaxDate](/uwp/api/windows.ui.xaml.controls.calendarview.maxdate) 屬性，以變更行事曆的最小和最大顯示日期。

```csharp
calendarView1.MinDate = new DateTime(2000, 1, 1);
calendarView1.MaxDate = new DateTime(2099, 12, 31);
```

### <a name="updating-calendar-day-items"></a>更新行事曆日期項目

行事曆中的每一天都是由 [CalendarViewDayItem](/uwp/api/Windows.UI.Xaml.Controls.CalendarViewDayItem) 物件來表示。 若要存取個別的日期項目，並使用它的屬性和方法，請處理 [CalendarViewDayItemChanging](/uwp/api/windows.ui.xaml.controls.calendarview.calendarviewdayitemchanging) 事件並使用事件引數的 Item 屬性來存取 CalendarViewDayItem。

您可以將 [CalendarViewDayItem.IsBlackout](/uwp/api/windows.ui.xaml.controls.calendarviewdayitem.isblackout) 屬性設定為 **true** ，以將行事曆中的某一個日期設定為無法選取。

您可以呼叫 [CalendarViewDayItem.SetDensityColors](/uwp/api/windows.ui.xaml.controls.calendarviewdayitem.setdensitycolors) 方法，以顯示一天中活動密度的內容相關資訊。 您可以為每一天顯示 0 到 10 個密度列，並設定每一列的色彩。

以下是行事曆中的某些日期項目。 第 1 天和第 2 天被封鎖。第 2 天、第 3 天和第 4 天設定了不同的密度列。

![含密度列的行事曆日期](images/calendar-view-density-bars.png)

### <a name="phased-rendering"></a>分階段的轉譯

行事曆檢視可以包含大量的 CalendarViewDayItem 物件。 為了讓 UI 保持回應，並允許順暢地瀏覽行事曆，行事曆檢視支援分階段的轉譯。 這樣可以讓您將日期項目分成數個階段來處理。 如果在所有階段都已完成之前，某一個日期移出檢視，則不會再花任何時間來處理及轉譯該項目。

此範例顯示排定約會的行事曆檢視分階段轉譯。
- 在階段 0 中，會轉譯預設日期項目。
- 在階段 1，您封鎖不能預約的日期。 這包含過去的日期、週日以及已完全預訂的日期。
- 在階段 2 中，您勾選針對該日期預訂的每個約會。 您為每個已確認的約會顯示綠色密度列，並為每個暫定的約會顯示藍色密度列。

此範例中的 `Bookings` 類別是來自虛構約會預約 App，而且不會顯示。

```xaml
<CalendarView CalendarViewDayItemChanging="CalendarView_CalendarViewDayItemChanging"/>
```

```csharp
private void CalendarView_CalendarViewDayItemChanging(CalendarView sender,
                                   CalendarViewDayItemChangingEventArgs args)
{
    // Render basic day items.
    if (args.Phase == 0)
    {
        // Register callback for next phase.
        args.RegisterUpdateCallback(CalendarView_CalendarViewDayItemChanging);
    }
    // Set blackout dates.
    else if (args.Phase == 1)
    {
        // Blackout dates in the past, Sundays, and dates that are fully booked.
        if (args.Item.Date < DateTimeOffset.Now ||
            args.Item.Date.DayOfWeek == DayOfWeek.Sunday ||
            Bookings.HasOpenings(args.Item.Date) == false)
        {
            args.Item.IsBlackout = true;
        }
        // Register callback for next phase.
        args.RegisterUpdateCallback(CalendarView_CalendarViewDayItemChanging);
    }
    // Set density bars.
    else if (args.Phase == 2)
    {
        // Avoid unnecessary processing.
        // You don't need to set bars on past dates or Sundays.
        if (args.Item.Date > DateTimeOffset.Now &&
            args.Item.Date.DayOfWeek != DayOfWeek.Sunday)
        {
            // Get bookings for the date being rendered.
            var currentBookings = Bookings.GetBookings(args.Item.Date);

            List<Color> densityColors = new List<Color>();
            // Set a density bar color for each of the days bookings.
            // It's assumed that there can't be more than 10 bookings in a day. Otherwise,
            // further processing is needed to fit within the max of 10 density bars.
            foreach (booking in currentBookings)
            {
                if (booking.IsConfirmed == true)
                {
                    densityColors.Add(Colors.Green);
                }
                else
                {
                    densityColors.Add(Colors.Blue);
                }
            }
            args.Item.SetDensityColors(densityColors);
        }
    }
}
```

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Xaml-Controls-Gallery) (英文) - 以互動式格式查看所有 XAML 控制項。

## <a name="related-articles"></a>相關文章

- [日期和時間控制項](date-and-time.md)
- [行事曆日期選擇器](calendar-date-picker.md)
- [日期選擇器](date-picker.md)
- [時間選擇器](time-picker.md)
