---
ms.assetid: ''
description: 本文將說明如何連線到遠端攝影機並取得 MediaFrameSourceGroup，以從每個相機取出畫面格。
title: 連線到遠端照相機
ms.date: 04/19/2019
ms.topic: article
ms.custom: 19H1
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1fae76aea28ffb63f6cb0ad5af8c5eb6e3fac6e4
ms.sourcegitcommit: 8171695ade04a762f19723f0b88e46e407375800
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/05/2020
ms.locfileid: "89494364"
---
# <a name="connect-to-remote-cameras"></a>連線到遠端照相機

本文說明如何連接到一或多個遠端攝影機，以及取得可讓您從每個相機讀取畫面格的 [**MediaFrameSourceGroup**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceGroup) 物件。 如需從媒體來源讀取框架的詳細資訊，請參閱 [使用 MediaFrameReader 處理媒體框架](process-media-frames-with-mediaframereader.md)。 如需與裝置配對的詳細資訊，請參閱 [配對裝置](../devices-sensors/pair-devices.md)。

> [!NOTE] 
> 從 Windows 10 1903 版開始，可以使用本文中討論的功能。

## <a name="create-a-devicewatcher-class-to-watch-for-available-remote-cameras"></a>建立 DeviceWatcher 類別以監看是否有可用的遠端攝影機

[**DeviceWatcher**](/uwp/api/windows.devices.enumeration.devicewatcher)類別會監視您應用程式可用的裝置，並在新增或移除裝置時通知您的應用程式。 藉由呼叫[**DeviceInformation**](/uwp/api/windows.devices.enumeration.deviceinformation.createwatcher#Windows_Devices_Enumeration_DeviceInformation_CreateWatcher_System_String_)來取得**DeviceWatcher**的實例，並傳遞 ADVANCED Query 語法 (AQS 識別您要監視之裝置類型的) 字串。 指定網路攝影機裝置的 AQS 字串如下所示：

```syntax
@"System.Devices.InterfaceClassGuid:=""{B8238652-B500-41EB-B4F3-4234F7F5AE99}"" AND System.Devices.InterfaceEnabled:=System.StructuredQueryType.Boolean#True"
```

> [!NOTE] 
> Helper 方法 [**MediaFrameSourceGroup**](/uwp/api/windows.media.capture.frames.mediaframesourcegroup.getdeviceselector) 會傳回 AQS 字串，該字串會監視本機連線和遠端網路攝影機。 若只要監視網路攝影機，您應該使用如上所示的 AQS 字串。

當您藉由呼叫[**start**](/uwp/api/windows.devices.enumeration.devicewatcher.start)方法來啟動傳回的**DeviceWatcher**時，它會為每個目前可用的網路攝影機引發[**新增**](/uwp/api/windows.devices.enumeration.devicewatcher.added)的事件。 在您藉由呼叫 [**stop**](/uwp/api/windows.devices.enumeration.devicewatcher.stop)來停止監看員之前，將會在新的網路攝影機裝置可用時引發 **新增** 的事件，而且當相機裝置變成無法使用時，將會引發 [**已移除**](/uwp/api/windows.devices.enumeration.devicewatcher.removed) 的事件。

傳遞至 **新增** 和 **移除** 事件處理常式的事件引數分別為 [**DeviceInformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) 或 [**DeviceInformationUpdate**](/uwp/api/windows.devices.enumeration.deviceinformationupdate) 物件。 這些物件的每一個都有 **Id** 屬性，它是引發事件之網路攝影機的識別碼。 將此識別碼傳遞至 [**MediaFrameSourceGroup. FromIdAsync**](/uwp/api/windows.media.capture.frames.mediaframesourcegroup.fromidasync) 方法，以取得您可以用來從相機取出框架的 [**MediaFrameSourceGroup**](/uwp/api/windows.media.capture.frames.mediaframesourcegroup.fromidasync) 物件。

## <a name="remote-camera-pairing-helper-class"></a>遠端攝影機配對協助程式類別

下列範例顯示的 helper 類別會使用**DeviceWatcher**來建立及更新**MediaFrameSourceGroup**物件的**ObservableCollection** ，以支援將資料系結至攝影機清單。 一般的應用程式會將 **MediaFrameSourceGroup** 包裝在自訂模型類別中。 請注意，helper 類別會維護應用程式 [**CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher) 的參考，並更新 [**RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) 呼叫內的攝影機集合，以確保 ui 執行緒上已更新系結至集合的 ui。

此外，這個範例也會處理已**加入**和**移除**的事件以外的[**DeviceWatcher**](/uwp/api/windows.devices.enumeration.devicewatcher.updated)事件。 在 **更新** 的處理常式中，會從移除相關聯的遠端相機裝置，然後將其新增回集合。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/RemoteCameraPairingHelper.cs" id="SnippetRemoteCameraPairingHelper":::

```cppwinrt
#include <winrt/Windows.Devices.Enumeration.h>
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Media.Capture.Frames.h>
#include <winrt/Windows.UI.Core.h>
using namespace winrt;
using namespace winrt::Windows::Devices::Enumeration;
using namespace winrt::Windows::Foundation::Collections;
using namespace winrt::Windows::Media::Capture::Frames;
using namespace winrt::Windows::UI::Core;

struct RemoteCameraPairingHelper
{
    RemoteCameraPairingHelper(CoreDispatcher uiDispatcher) :
        m_dispatcher(uiDispatcher)
    {
        m_remoteCameraCollection = winrt::single_threaded_observable_vector<MediaFrameSourceGroup>();
        auto remoteCameraAqs =
            LR"(System.Devices.InterfaceClassGuid:=""{B8238652-B500-41EB-B4F3-4234F7F5AE99}"")"
            LR"(AND System.Devices.InterfaceEnabled:=System.StructuredQueryType.Boolean#True)";
        m_watcher = DeviceInformation::CreateWatcher(remoteCameraAqs);
        m_watcherAddedAutoRevoker = m_watcher.Added(winrt::auto_revoke, { this, &RemoteCameraPairingHelper::Watcher_Added });
        m_watcherRemovedAutoRevoker = m_watcher.Removed(winrt::auto_revoke, { this, &RemoteCameraPairingHelper::Watcher_Removed });
        m_watcherUpdatedAutoRevoker = m_watcher.Updated(winrt::auto_revoke, { this, &RemoteCameraPairingHelper::Watcher_Updated });
        m_watcher.Start();
    }
    ~RemoteCameraPairingHelper()
    {
        m_watcher.Stop();
    }
    IObservableVector<MediaFrameSourceGroup> FrameSourceGroups()
    {
        return m_remoteCameraCollection;
    }
    winrt::fire_and_forget Watcher_Added(DeviceWatcher /* sender */, DeviceInformation args)
    {
        co_await AddDeviceAsync(args.Id());
    }
    winrt::fire_and_forget Watcher_Removed(DeviceWatcher /* sender */, DeviceInformationUpdate args)
    {
        co_await RemoveDevice(args.Id());
    }
    winrt::fire_and_forget Watcher_Updated(DeviceWatcher /* sender */, DeviceInformationUpdate args)
    {
        co_await RemoveDevice(args.Id());
        co_await AddDeviceAsync(args.Id());
    }
    Windows::Foundation::IAsyncAction AddDeviceAsync(winrt::hstring id)
    {
        auto group = co_await MediaFrameSourceGroup::FromIdAsync(id);
        if (group)
        {
            co_await m_dispatcher;
            m_remoteCameraCollection.Append(group);
        }
    }
    Windows::Foundation::IAsyncAction RemoveDevice(winrt::hstring id)
    {
        co_await m_dispatcher;

        uint32_t ix{ 0 };
        for (auto const&& item : m_remoteCameraCollection)
        {
            if (item.Id() == id)
            {
                m_remoteCameraCollection.RemoveAt(ix);
                break;
            }
            ++ix;
        }
    }

private:
    CoreDispatcher m_dispatcher{ nullptr };
    DeviceWatcher m_watcher{ nullptr };
    IObservableVector<MediaFrameSourceGroup> m_remoteCameraCollection;
    DeviceWatcher::Added_revoker m_watcherAddedAutoRevoker;
    DeviceWatcher::Removed_revoker m_watcherRemovedAutoRevoker;
    DeviceWatcher::Updated_revoker m_watcherUpdatedAutoRevoker;
};
```

## <a name="related-topics"></a>相關的主題

* [相機](camera.md)
* [使用 MediaCapture 進行基本相片、視訊和音訊的擷取](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [相機畫面範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFrames)
* [使用 MediaFrameReader 處理媒體畫面](process-media-frames-with-mediaframereader.md)
