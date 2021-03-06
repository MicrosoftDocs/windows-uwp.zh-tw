---
ms.assetid: E2A1200C-9583-40FA-AE4D-C9E6F6C32BCF
title: 將工作項目提交至執行緒集區
description: 了解如何透過將工作項目提交至執行緒集區，以使用個別的執行緒來執行工作。
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, 執行緒, 執行緒集區
ms.localizationpriority: medium
ms.openlocfilehash: 3576f907e4ab601013d22fe9ae7697e0ec523ce4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155202"
---
# <a name="submit-a-work-item-to-the-thread-pool"></a>將工作項目提交至執行緒集區

\[ 已更新 Windows 10 上的 UWP 應用程式。 如 Windows 8. x 文章，請參閱[archive](/previous-versions/windows/apps/mt244353(v=win.10))封存\]

<b>重要 API</b>

-   [**RunAsync**](/uwp/api/windows.system.threading.threadpool.runasync)
-   [**IAsyncAction**](/uwp/api/Windows.Foundation.IAsyncAction)

了解如何透過將工作項目提交至執行緒集區，以使用個別的執行緒來執行工作。 工作若還要好一段時間才能完成，可使用這種方式讓 UI 保持回應，還可用來以並行方式完成多個工作。

## <a name="create-and-submit-the-work-item"></a>建立及提交工作項目

透過呼叫 [**RunAsync**](/uwp/api/windows.system.threading.threadpool.runasync) 建立工作項目。 提供委派進行工作 (您可以使用 Lambda 或委派函式)。 請注意，**RunAsync** 會傳回 [**IAsyncAction**](/uwp/api/Windows.Foundation.IAsyncAction) 物件；請將這個物件儲存起來以在下個步驟中使用。

提供三個版本的 [**RunAsync**](/uwp/api/windows.system.threading.threadpool.runasync)，讓您可以選擇性地指定工作項目的優先順序，並控制是否與其他工作項目同時執行。

>[!NOTE]
>使用 [**CoreDispatcher RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) 來存取 UI 執行緒，並顯示工作專案的進度。

下列範例會建立一個工作項目，並且提供 Lambda 來執行工作：

```csharp
// The nth prime number to find.
const uint n = 9999;

// A shared pointer to the result.
// We use a shared pointer to keep the result alive until the
// thread is done.
ulong nthPrime = 0;

// Simulates work by searching for the nth prime number. Uses a
// naive algorithm and counts 2 as the first prime number.
IAsyncAction asyncAction = Windows.System.Threading.ThreadPool.RunAsync(
    (workItem) =>
{
    uint  progress = 0; // For progress reporting.
    uint  primes = 0;   // Number of primes found so far.
    ulong i = 2;        // Number iterator.

    if ((n >= 0) && (n <= 2))
    {
        nthPrime = n;
        return;
    }

    while (primes < (n - 1))
    {
        if (workItem.Status == AsyncStatus.Canceled)
        {
            break;
        }

        // Go to the next number.
        i++;

        // Check for prime.
        bool prime = true;
        for (uint j = 2; j < i; ++j)
        {
            if ((i % j) == 0)
            {
                prime = false;
                break;
            }
        };

        if (prime)
        {
            // Found another prime number.
            primes++;

            // Report progress at every 10 percent.
            uint temp = progress;
            progress = (uint)(10.0*primes/n);

            if (progress != temp)
            {
                String updateString;
                updateString = "Progress to " + n + "th prime: "
                    + (10 * progress) + "%\n";

                // Update the UI thread with the CoreDispatcher.
                CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(
                    CoreDispatcherPriority.High,
                    new DispatchedHandler(() =>
                {
                    UpdateUI(updateString);
                }));
            }
        }
    }

    // Return the nth prime number.
    nthPrime = i;
});

// A reference to the work item is cached so that we can trigger a
// cancellation when the user presses the Cancel button.
m_workItem = asyncAction;
```

```cppwinrt
// The nth prime number to find.
const unsigned int n{ 9999 };

unsigned long nthPrime{ 0 };

// Simulates work by searching for the nth prime number. Uses a
// naive algorithm and counts 2 as the first prime number.

// A reference to the work item is cached so that we can trigger a
// cancellation when the user presses the Cancel button.
m_workItem = Windows::System::Threading::ThreadPool::RunAsync([&](Windows::Foundation::IAsyncAction const& workItem)
{
    unsigned int progress = 0; // For progress reporting.
    unsigned int primes = 0;   // Number of primes found so far.
    unsigned long int i = 2;   // Number iterator.

    if ((n >= 0) && (n <= 2))
    {
        nthPrime = n;
        return;
    }

    while (primes < (n - 1))
    {
        if (workItem.Status() == Windows::Foundation::AsyncStatus::Canceled)
        {
            break;
        }

        // Go to the next number.
        i++;

        // Check for prime.
        bool prime = true;
        for (unsigned int j = 2; j < i; ++j)
        {
            if ((i % j) == 0)
            {
                prime = false;
                break;
            }
        };

        if (prime)
        {
            // Found another prime number.
            primes++;

            // Report progress at every 10 percent.
            unsigned int temp = progress;
            progress = static_cast<unsigned int>(10.f*primes / n);

            if (progress != temp)
            {
                std::wstringstream updateString;
                updateString << L"Progress to " << n << L"th prime: " << (10 * progress) << std::endl;

                // Update the UI thread with the CoreDispatcher.
                Windows::ApplicationModel::Core::CoreApplication::MainView().CoreWindow().Dispatcher().RunAsync(
                    Windows::UI::Core::CoreDispatcherPriority::High,
                    Windows::UI::Core::DispatchedHandler([&]()
                {
                    UpdateUI(updateString.str());
                }));
            }
        }
    }
    // Return the nth prime number.
    nthPrime = i;
});
```

```cpp
// The nth prime number to find.
const unsigned int n = 9999;

// A shared pointer to the result.
// We use a shared pointer to keep the result alive until the
// thread is done.
std::shared_ptr<unsigned long> nthPrime = std::make_shared<unsigned long int>(0);

// Simulates work by searching for the nth prime number. Uses a
// naive algorithm and counts 2 as the first prime number.
auto workItem = ref new Windows::System::Threading::WorkItemHandler(
    \[this, n, nthPrime](IAsyncAction^ workItem)
{
    unsigned int progress = 0; // For progress reporting.
    unsigned int primes = 0;   // Number of primes found so far.
    unsigned long int i = 2;   // Number iterator.

    if ((n >= 0) && (n <= 2))
    {
        *nthPrime = n;
        return;
    }

    while (primes < (n - 1))
    {
        if (workItem->Status == AsyncStatus::Canceled)
       {
           break;
       }

       // Go to the next number.
       i++;

       // Check for prime.
       bool prime = true;
       for (unsigned int j = 2; j < i; ++j)
       {
           if ((i % j) == 0)
           {
               prime = false;
               break;
           }
       };

       if (prime)
       {
           // Found another prime number.
           primes++;

           // Report progress at every 10 percent.
           unsigned int temp = progress;
           progress = static_cast<unsigned int>(10.f*primes / n);

           if (progress != temp)
           {
               String^ updateString;
               updateString = "Progress to " + n + "th prime: "
                   + (10 * progress).ToString() + "%\n";

               // Update the UI thread with the CoreDispatcher.
               CoreApplication::MainView->CoreWindow->Dispatcher->RunAsync(
                   CoreDispatcherPriority::High,
                   ref new DispatchedHandler([this, updateString]()
               {
                   UpdateUI(updateString);
               }));
           }
       }
   }
   // Return the nth prime number.
   *nthPrime = i;
});

auto asyncAction = ThreadPool::RunAsync(workItem);

// A reference to the work item is cached so that we can trigger a
// cancellation when the user presses the Cancel button.
m_workItem = asyncAction;
```

呼叫 [**RunAsync**](/uwp/api/windows.system.threading.threadpool.runasync) 之後，工作項目會排入執行緒集區，然後在有執行緒可用時執行。 執行緒集區工作項目會以非同步方式執行，執行順序不拘，以確保工作項目可以獨立運作。

請注意，工作項目會檢查 [**IAsyncInfo.Status**](/uwp/api/windows.foundation.iasyncinfo.status) 屬性，並在取消工作項目時結束。

## <a name="handle-work-item-completion"></a>處理工作項目的完成

透過設定工作項目的 [**IAsyncAction.Completed**](/uwp/api/windows.foundation.iasyncaction.completed) 屬性，提供完成處理常式。 提供委派 (您可以使用 Lambda 或委派函式) 處理工作項目的完成。 例如，使用 [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) 存取 UI 執行緒及顯示結果。

下列範例以在步驟 1 提交之工作項目的結果更新 UI：

```cpp
asyncAction->Completed = ref new AsyncActionCompletedHandler(
    \[this, n, nthPrime](IAsyncAction^ asyncInfo, AsyncStatus asyncStatus)
{
    if (asyncStatus == AsyncStatus::Canceled)
    {
        return;
    }

    String^ updateString;
    updateString = "\n" + "The " + n + "th prime number is "
        + (*nthPrime).ToString() + ".\n";

    // Update the UI thread with the CoreDispatcher.
    CoreApplication::MainView->CoreWindow->Dispatcher->RunAsync(
        CoreDispatcherPriority::High,
        ref new DispatchedHandler([this, updateString]()
    {
        UpdateUI(updateString);
    }));
});
```

```cppwinrt
m_workItem.Completed([&](Windows::Foundation::IAsyncAction const& asyncInfo, Windows::Foundation::AsyncStatus const& asyncStatus)
{
    if (asyncStatus == Windows::Foundation::AsyncStatus::Canceled)
    {
        return;
    }

    std::wstringstream updateString;
    updateString << std::endl << L"The " << n << L"th prime number is " << nthPrime << std::endl;

    // Update the UI thread with the CoreDispatcher.
    Windows::ApplicationModel::Core::CoreApplication::MainView().CoreWindow().Dispatcher().RunAsync(
        Windows::UI::Core::CoreDispatcherPriority::High,
        Windows::UI::Core::DispatchedHandler([&]()
    {
        UpdateUI(updateString.str());
    }));
});
```

```c#
asyncAction.Completed = new AsyncActionCompletedHandler(
    (IAsyncAction asyncInfo, AsyncStatus asyncStatus) =>
{
    if (asyncStatus == AsyncStatus.Canceled)
    {
        return;
    }

    String updateString;
    updateString = "\n" + "The " + n + "th prime number is "
        + nthPrime + ".\n";

    // Update the UI thread with the CoreDispatcher.
    CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(
        CoreDispatcherPriority.High,
        new DispatchedHandler(()=>
    {
        UpdateUI(updateString);
    }));
});
```

請注意，完成處理常式在分派 UI 更新之前，會檢查是否已取消工作項目。

## <a name="summary-and-next-steps"></a>摘要和後續步驟

若要深入瞭解，請從本快速入門的程式碼，建立針對 Windows 8.1 所撰寫的 [ThreadPool 工作專案範例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Thread%20pool%20sample) ，並重複使用 win \_ unap Windows 10 應用程式中的原始程式碼。

## <a name="related-topics"></a>相關主題

* [將工作項目提交至執行緒集區](submit-a-work-item-to-the-thread-pool.md)
* [使用執行緒集區的最佳做法](best-practices-for-using-the-thread-pool.md)
* [使用計時器提交工作項目](use-a-timer-to-submit-a-work-item.md)
 