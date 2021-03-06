---
description: 本主題示範如何將 WRL 程式碼移植到其在 C++/WinRT 中的對等項目。
title: 從 WRL 移到 C++/WinRT
ms.date: 05/30/2018
ms.topic: article
keywords: Windows 10, uwp, 標準, c++, cpp, winrt, 投影, 連接埠, 移轉, WRL
ms.localizationpriority: medium
ms.openlocfilehash: f7e7000a70a71f8ad90fd38c74d6e29882b4cbaa
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157254"
---
# <a name="move-to-cwinrt-from-wrl"></a>從 WRL 移到 C++/WinRT
本主題示範如何將 [Windows 執行階段 C++ 範本庫 (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) 程式碼移植到其在 [C++/WinRT](./intro-to-using-cpp-with-winrt.md) 中的對等項目。

移植 C+/WinRT 中的第一個步驟是手動新增 C++/WinRT 支援您的專案 (請參閱 [Visual Studio 支援 C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package))。 若要這麼做，請將 [Microsoft.Windows.CppWinRT NuGet 套件](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/)安裝到您的專案中。 在 Visual Studio 中開啟專案，按一下 [專案]  \>[管理 NuGet 套件...]  \>[瀏覽]  、在搜尋方塊中輸入或貼上 **Microsoft.Windows.CppWinRT**、在搜尋結果中選取項目，然後按一下 [安裝]  以安裝適用於該專案的套件。 該項變更的一個效果是專案中支援的 [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) 為關閉。 如果您在專案中使用 C++/CX，則您也可以讓支援保持關閉並更新 C++/CX 程式碼為 C++/WinRT (查看[從 C++/CX 移到 C++/WinRT](move-to-winrt-from-cx.md))。 或您可以將支援切換回來 (專案屬性中，**C/C++** \> **一般** \>  **使用 Windows 執行階段延伸** \> **是 (/ZW)** )，並優先著重在移植您的 WRL 程式碼。 C++/CX 和 C++/WinRT 程式碼可以共存在相同的專案中，但 XAML 編譯器支援與 Windows 執行階段元件除外 (請參閱[從 C++/CX 移到 C++/WinRT](move-to-winrt-from-cx.md))。

將專案屬性**一般** \> **目標平台版本**設置為 10.0.17134.0 (Windows 10 版本 1803) 或更高版本。

在您先行編譯的標頭檔案 (通常是 `pch.h`) 中，包含 `winrt/base.h`。

```cppwinrt
#include <winrt/base.h>
```

如果包含任何 C++/WinRT 投影 Windows API 標頭 (例如，`winrt/Windows.Foundation.h`)，則您不需要像這樣明確包含 `winrt/base.h`，因為它會自動為您包含。

## <a name="porting-wrl-com-smart-pointers-microsoftwrlcomptr"></a>移植 WRL COM 智慧型指標 ([Microsoft: 110:: WRL::ComPtr](/cpp/windows/comptr-class))
移植任何使用 **Microsoft::WRL::ComPtr\<T\>** 的程式碼以使用 [**winrt::com_ptr\<T\>** ](/uwp/cpp-ref-for-winrt/com-ptr)。 以下是變更前後的程式碼範例。 在「之後」  版本中，[**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput-function) 成員函式擷取基礎原始指標，如此一來便可以設定它。

```cpp
ComPtr<IDXGIAdapter1> previousDefaultAdapter;
DX::ThrowIfFailed(m_dxgiFactory->EnumAdapters1(0, &previousDefaultAdapter));
```

```cppwinrt
winrt::com_ptr<IDXGIAdapter1> previousDefaultAdapter;
winrt::check_hresult(m_dxgiFactory->EnumAdapters1(0, previousDefaultAdapter.put()));
```

> [!IMPORTANT]
> 如果您有已安置好的 [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) (其內部原始指標已有目標)，但您想將其重新安置，以指向不同目標，那麼您必須先對其指派 `nullptr`&mdash;如下列程式碼範例所示。 如果您沒有這麼做，已安置好的 **com_ptr** 將會藉由宣稱內部指標不是 Null，來提出錯誤並引起您的注意 (當您呼叫 [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput-function) 或 [**com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput_void-function) 時)。

```cppwinrt
winrt::com_ptr<IDXGISwapChain1> m_pDXGISwapChain1;
...
// We execute the code below each time the window size changes.
m_pDXGISwapChain1 = nullptr; // Important because we're about to re-seat 
winrt::check_hresult(
    m_pDxgiFactory->CreateSwapChainForHwnd(
        m_pCommandQueue.get(), // For Direct3D 12, this is a pointer to a direct command queue, and not to the device.
        m_hWnd,
        &swapChainDesc,
        nullptr,
        nullptr,
        m_pDXGISwapChain1.put())
);
```

在下一個範例中 (在 *之後* 版本)，[**com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput_void-function) 成員函式擷取基礎原始指標做為無效指標的指標。

```cpp
ComPtr<ID3D12Debug> debugController;
if (SUCCEEDED(D3D12GetDebugInterface(IID_PPV_ARGS(&debugController))))
{
    debugController->EnableDebugLayer();
}
```

```cppwinrt
winrt::com_ptr<ID3D12Debug> debugController;
if (SUCCEEDED(D3D12GetDebugInterface(__uuidof(debugController), debugController.put_void())))
{
    debugController->EnableDebugLayer();
}
```

使用 [**com_ptr::get**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrget-function) 取代 **ComPtr::Get**。

```cpp
m_d3dDevice->CreateDepthStencilView(m_depthStencil.Get(), &dsvDesc, m_dsvHeap->GetCPUDescriptorHandleForHeapStart());
```

```cppwinrt
m_d3dDevice->CreateDepthStencilView(m_depthStencil.get(), &dsvDesc, m_dsvHeap->GetCPUDescriptorHandleForHeapStart());
```

當您想要將基礎原始指標傳遞至期待一個 **IUnknown** 指標的函式，請使用 [**winrt::get_unknown**](/uwp/cpp-ref-for-winrt/get-unknown) 函式，下一個範例做為示範。

```cpp
ComPtr<IDXGISwapChain1> swapChain;
DX::ThrowIfFailed(
    m_dxgiFactory->CreateSwapChainForCoreWindow(
        m_commandQueue.Get(),
        reinterpret_cast<IUnknown*>(m_window.Get()),
        &swapChainDesc,
        nullptr,
        &swapChain
    )
);
```

```cppwinrt
winrt::agile_ref<winrt::Windows::UI::Core::CoreWindow> m_window; 
winrt::com_ptr<IDXGISwapChain1> swapChain;
winrt::check_hresult(
    m_dxgiFactory->CreateSwapChainForCoreWindow(
        m_commandQueue.get(),
        winrt::get_unknown(m_window.get()),
        &swapChainDesc,
        nullptr,
        swapChain.put()
    )
);
```

## <a name="porting-a-wrl-module-microsoftwrlmodule"></a>移植 WRL 模組 (Microsoft: 110:: WRL::Module)
您可以逐漸將 C++/WinRT 程式碼新增至使用 WRL 實作元件的現有投影，且會持續支援現有的 WRL 類別。 本章節示範方式。

如果您在 Visual Studio 中建立新的 **Windows 執行階段元件 (C++/WinRT)** 專案類型並組建，則為您產生檔案 `Generated Files\module.g.cpp`。 該檔案包含兩個的實用 C++/WinRT 函式（列於下方）的定義，您可以將其複製並新增到您的專案。 這些函式為 **WINRT_CanUnloadNow** 和 **WINRT_GetActivationFactory**，如您所見，他們有條件的呼叫 WRL 來支援您，不論您在移植的哪個階段。

```cppwinrt
HRESULT WINRT_CALL WINRT_CanUnloadNow()
{
#ifdef _WRL_MODULE_H_
    if (!::Microsoft::WRL::Module<::Microsoft::WRL::InProc>::GetModule().Terminate())
    {
        return S_FALSE;
    }
#endif

    if (winrt::get_module_lock())
    {
        return S_FALSE;
    }

    winrt::clear_factory_cache();
    return S_OK;
}

HRESULT WINRT_CALL WINRT_GetActivationFactory(HSTRING classId, void** factory)
{
    try
    {
        *factory = nullptr;
        wchar_t const* const name = WINRT_WindowsGetStringRawBuffer(classId, nullptr);

        if (0 == wcscmp(name, L"MoveFromWRLTest.Class"))
        {
            *factory = winrt::detach_abi(winrt::make<winrt::MoveFromWRLTest::factory_implementation::Class>());
            return S_OK;
        }

#ifdef _WRL_MODULE_H_
        return ::Microsoft::WRL::Module<::Microsoft::WRL::InProc>::GetModule().GetActivationFactory(classId, reinterpret_cast<::IActivationFactory**>(factory));
#else
        return winrt::hresult_class_not_available().to_abi();
#endif
    }
    catch (...) { return winrt::to_hresult(); }
}
```

一旦您在專案中取得這些函式，而不是直接呼叫 [**Module::GetActivationFactory**](/cpp/windows/module-getactivationfactory-method)，請呼叫 **WINRT_GetActivationFactory**（其內部呼叫 WRL 函式）。 以下是變更前後的程式碼範例。

```cpp
HRESULT WINAPI DllGetActivationFactory(_In_ HSTRING activatableClassId, _Out_ ::IActivationFactory **factory)
{
    auto & module = Microsoft::WRL::Module<Microsoft::WRL::InProc>::GetModule();
    return module.GetActivationFactory(activatableClassId, factory);
}
```

```cppwinrt
HRESULT __stdcall WINRT_GetActivationFactory(HSTRING activatableClassId, void** factory);
HRESULT WINAPI DllGetActivationFactory(_In_ HSTRING activatableClassId, _Out_ ::IActivationFactory **factory)
{
    return WINRT_GetActivationFactory(activatableClassId, reinterpret_cast<void**>(factory));
}
```

呼叫 **WINRT_CanUnloadNow**（ 其內部呼叫 WRL 函式），而非直接呼叫 [**Module::Terminate**](/cpp/windows/module-terminate-method)。 以下是變更前後的程式碼範例。

```cpp
HRESULT __stdcall DllCanUnloadNow(void)
{
    auto &module = Microsoft::WRL::Module<Microsoft::WRL::InProc>::GetModule();
    HRESULT hr = (module.Terminate() ? S_OK : S_FALSE);
    if (hr == S_OK)
    {
        hr = ...
    }
    return hr;
}
```

```cppwinrt
HRESULT __stdcall WINRT_CanUnloadNow();
HRESULT __stdcall DllCanUnloadNow(void)
{
    HRESULT hr = WINRT_CanUnloadNow();
    if (hr == S_OK)
    {
        hr = ...
    }
    return hr;
}
```

## <a name="important-apis"></a>重要 API
* [winrt::com_ptr 結構範本](/uwp/cpp-ref-for-winrt/com-ptr)
* [winrt::Windows::Foundation::IUnknown 結構](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)

## <a name="related-topics"></a>相關主題
* [C++/WinRT 的簡介](intro-to-using-cpp-with-winrt.md)
* [從 C++/CX 移到 C++/WinRT](move-to-winrt-from-cx.md)
* [Windows 執行階段 C++ 範本庫 (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl)