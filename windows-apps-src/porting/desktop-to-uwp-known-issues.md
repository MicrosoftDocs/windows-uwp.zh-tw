---
author: awkoren
Description: "此文章包含傳統型轉 UWP 橋接器的已知問題。"
Search.Product: eADQiWindows 10XVcnh
title: "傳統型橋接器的已知問題"
translationtype: Human Translation
ms.sourcegitcommit: 537c6a3d4559da4673b68c3ab5bdddb612760849
ms.openlocfilehash: d02921247bd77d59bbb09037a4ced8d3967c33b2

---
# 傳統型橋接器的已知問題

此文章包含傳統型轉 UWP 橋接器的已知問題。

## 藍色畫面，錯誤碼 0x139 (KERNEL_SECURITY_CHECK_FAILURE)

從 Windows 市集安裝或啟動特定 App 後，電腦可能會發生下列錯誤而意外地重新開機：**0x139 (KERNEL\_SECURITY\_CHECK\_ FAILURE)**。

已知受影響的 App 包括 Kodi、JT2Go、Ear Trumpet、Teslagrad 和其他 App。

2016 年 10 月 27 日已發佈一 個 [Windows 更新 (版本 14393.351 - KB3197954)](https://support.microsoft.com/kb/3197954)，其中包含可處理此問題的重要修正。 如果您發生此問題，請更新您的電腦。 如果您因為電腦在可以登入之前就重新開機而無法更新電腦，您應該使用系統還原將您的系統復原至安裝受影響 App 之前的時間點。 如需如何使用系統還原的資訊，請參閱 [Windows10 中的復原選項](https://support.microsoft.com/en-us/help/12415/windows-10-recovery-options)。 

如果更新無法修正問題，或您不確定如何復原電腦，請連絡 [Microsoft 支援服務](https://support.microsoft.com/contactus/)。 

如果您是開發人員，建議您避免在不包含此更新的 Windows 版本上安裝傳統型橋接器 App。 請注意，這麼做將會使您的 App 無法提供給尚未安裝更新的使用者。 若要將您 App 的可用性限制為僅提供給已安裝此更新的使用者，請如下所示修改您的 AppxManifest.xml 檔案：

```<TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14393.351" MaxVersionTested="10.0.14393.351"/>```

關於 Windows Update 的詳細資料，請參閱： 
* https://support.microsoft.com/3197954
* https://support.microsoft.com/help/12387/windows-10-update-history


<!--HONumber=Nov16_HO1-->

