---
title: 建立深度緩衝區裝置資源
description: 了解如何建立必要的 Direct3D 裝置資源，以支援陰影體的深度測試。
ms.assetid: 86d5791b-1faa-17e4-44a8-bbba07062756
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, 遊戲, Direct3D, 深度緩衝區
ms.localizationpriority: medium
ms.openlocfilehash: 0032d77bb8d572229ea77df736c807a0a85e9ecb
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175372"
---
# <a name="create-depth-buffer-device-resources"></a>建立深度緩衝區裝置資源




了解如何建立必要的 Direct3D 裝置資源，以支援陰影體的深度測試。 [逐步解說：使用 Direct3D 11 中的深度緩衝區實作陰影體](implementing-depth-buffers-for-shadow-mapping.md)第一部分。

## <a name="resources-youll-need"></a>所需資源


轉譯陰影體的深度圖需要下列 Direct3D 裝置相關的資源：

-   深度圖的資源 (緩衝區)
-   資源的深度樣板檢視與著色器資源檢視
-   比較取樣器狀態物件
-   光源 POV 矩陣的常數緩衝區
-   轉譯陰影圖的檢視區 (通常是正方形檢視區)
-   啟用正面消除的轉譯狀態物件
-   如果您尚未使用轉譯狀態物件，您將需要它才能切換回背面消除。

請注意，建立這些資源必須包含在裝置相關的資源建立常式中，舉例來說，如果安裝新的裝置驅動程式時，或使用者將您的應用程式移至附加到不同圖形介面卡的監視器時，轉譯器才能重建這些資源。

## <a name="check-feature-support"></a>檢查功能支援


在建立深度對應之前，請在 Direct3D 裝置上呼叫 [**CheckFeatureSupport**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-checkfeaturesupport) 方法，要求 **D3D11 \_ 功能 \_ D3D9 \_ 陰影 \_ 支援**，並提供 [**D3D11 \_ 功能 \_ 資料 \_ D3D9 \_ 陰影 \_ 支援**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_feature_data_d3d9_shadow_support) 結構。

```cpp
D3D11_FEATURE_DATA_D3D9_SHADOW_SUPPORT isD3D9ShadowSupported;
ZeroMemory(&isD3D9ShadowSupported, sizeof(isD3D9ShadowSupported));
pD3DDevice->CheckFeatureSupport(
    D3D11_FEATURE_D3D9_SHADOW_SUPPORT,
    &isD3D9ShadowSupported,
    sizeof(D3D11_FEATURE_D3D9_SHADOW_SUPPORT)
    );

if (isD3D9ShadowSupported.SupportsDepthAsTextureWithLessEqualComparisonFilter)
{
    // Init shadow map resources

```

如果這項功能不受支援，請勿嘗試載入針對著色器模型4層級 9 \_ x （呼叫範例比較函式）編譯的著色器。 在許多案例中，缺乏此功能的支援通常表示 GPU 是傳統裝置，其中的驅動程式未經更新，且無法支援 WDDM 1.2 或以上版本。 如果裝置至少支援功能層級 10 \_ 0，則您可以改為載入針對著色器模型 4 0 編譯的範例比較著色器 \_ 。

## <a name="create-depth-buffer"></a>建立深度緩衝區


首先，嘗試使用較高精確度的深度格式來建立深度圖。 先設定相符的著色器資源檢視屬性。 如果資源建立失敗，例如因為裝置記憶體過低或硬體不支援該格式，請嘗試較低精確度的格式，並變更為符合的屬性。

如果您只需要低精確度的深度格式（例如，在中型解決方案 Direct3D 功能層級 9 1 的裝置上轉譯時），則這個步驟是選擇性的 \_ 。

```cpp
D3D11_TEXTURE2D_DESC shadowMapDesc;
ZeroMemory(&shadowMapDesc, sizeof(D3D11_TEXTURE2D_DESC));
shadowMapDesc.Format = DXGI_FORMAT_R24G8_TYPELESS;
shadowMapDesc.MipLevels = 1;
shadowMapDesc.ArraySize = 1;
shadowMapDesc.SampleDesc.Count = 1;
shadowMapDesc.BindFlags = D3D11_BIND_SHADER_RESOURCE | D3D11_BIND_DEPTH_STENCIL;
shadowMapDesc.Height = static_cast<UINT>(m_shadowMapDimension);
shadowMapDesc.Width = static_cast<UINT>(m_shadowMapDimension);

HRESULT hr = pD3DDevice->CreateTexture2D(
    &shadowMapDesc,
    nullptr,
    &m_shadowMap
    );
```

接著建立資源檢視。 在深度樣板檢視上將 MIP 圖塊設成零，在著色器資源檢視上將 MIP 層級設為 1。 兩者都有 TEXTURE2D 的材質維度，而且兩者都需要使用相符的 [**DXGI \_ 格式**](/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format)。

```cpp
D3D11_DEPTH_STENCIL_VIEW_DESC depthStencilViewDesc;
ZeroMemory(&depthStencilViewDesc, sizeof(D3D11_DEPTH_STENCIL_VIEW_DESC));
depthStencilViewDesc.Format = DXGI_FORMAT_D24_UNORM_S8_UINT;
depthStencilViewDesc.ViewDimension = D3D11_DSV_DIMENSION_TEXTURE2D;
depthStencilViewDesc.Texture2D.MipSlice = 0;

D3D11_SHADER_RESOURCE_VIEW_DESC shaderResourceViewDesc;
ZeroMemory(&shaderResourceViewDesc, sizeof(D3D11_SHADER_RESOURCE_VIEW_DESC));
shaderResourceViewDesc.ViewDimension = D3D11_SRV_DIMENSION_TEXTURE2D;
shaderResourceViewDesc.Format = DXGI_FORMAT_R24_UNORM_X8_TYPELESS;
shaderResourceViewDesc.Texture2D.MipLevels = 1;

hr = pD3DDevice->CreateDepthStencilView(
    m_shadowMap.Get(),
    &depthStencilViewDesc,
    &m_shadowDepthView
    );

hr = pD3DDevice->CreateShaderResourceView(
    m_shadowMap.Get(),
    &shaderResourceViewDesc,
    &m_shadowResourceView
    );
```

## <a name="create-comparison-state"></a>建立比較狀態


現在要建立比較取樣器狀態物件。 功能層級 9 \_ 1 只支援 \_ \_ 較不 \_ 相等的 D3D11 比較。 [支援硬體範圍的陰影圖](target-a-range-of-hardware.md)中會深入說明篩選選項 - 或者您可直接挑選快速陰影圖的點篩選。

請注意，您可以指定 D3D11 \_ 材質 \_ 位址 \_ 邊界位址模式，它會在功能層級 9 \_ 1 的裝置上運作。 這適用於像素著色器，它在進行深度測試之前，不會測試像素是否位於光源的檢視範圍中。 為每個邊界指定 0 或 1，您就可以控制是否要讓超出光源檢視範圍的像素通過深度測試，像素因而照亮或處於陰影中。

在功能層級 9 \_ 1 上，必須設定下列必要值： **MinLOD** 設定為零、 **MaxLOD** 設定為 **D3D11 \_ FLOAT32 \_ MAX**，而 **MaxAnisotropy** 設定為零。

```cpp
D3D11_SAMPLER_DESC comparisonSamplerDesc;
ZeroMemory(&comparisonSamplerDesc, sizeof(D3D11_SAMPLER_DESC));
comparisonSamplerDesc.AddressU = D3D11_TEXTURE_ADDRESS_BORDER;
comparisonSamplerDesc.AddressV = D3D11_TEXTURE_ADDRESS_BORDER;
comparisonSamplerDesc.AddressW = D3D11_TEXTURE_ADDRESS_BORDER;
comparisonSamplerDesc.BorderColor[0] = 1.0f;
comparisonSamplerDesc.BorderColor[1] = 1.0f;
comparisonSamplerDesc.BorderColor[2] = 1.0f;
comparisonSamplerDesc.BorderColor[3] = 1.0f;
comparisonSamplerDesc.MinLOD = 0.f;
comparisonSamplerDesc.MaxLOD = D3D11_FLOAT32_MAX;
comparisonSamplerDesc.MipLODBias = 0.f;
comparisonSamplerDesc.MaxAnisotropy = 0;
comparisonSamplerDesc.ComparisonFunc = D3D11_COMPARISON_LESS_EQUAL;
comparisonSamplerDesc.Filter = D3D11_FILTER_COMPARISON_MIN_MAG_MIP_POINT;

// Point filtered shadows can be faster, and may be a good choice when
// rendering on hardware with lower feature levels. This sample has a
// UI option to enable/disable filtering so you can see the difference
// in quality and speed.

DX::ThrowIfFailed(
    pD3DDevice->CreateSamplerState(
        &comparisonSamplerDesc,
        &m_comparisonSampler_point
        )
    );
```

## <a name="create-render-states"></a>建立轉譯狀態


現在要建立轉譯狀態，您可以用於啟用正面消除。 請注意，功能層級 9 \_ 1 裝置需要將 **DepthClipEnable** 設為 **true**。

```cpp
D3D11_RASTERIZER_DESC drawingRenderStateDesc;
ZeroMemory(&drawingRenderStateDesc, sizeof(D3D11_RASTERIZER_DESC));
drawingRenderStateDesc.CullMode = D3D11_CULL_BACK;
drawingRenderStateDesc.FillMode = D3D11_FILL_SOLID;
drawingRenderStateDesc.DepthClipEnable = true; // Feature level 9_1 requires DepthClipEnable == true
DX::ThrowIfFailed(
    pD3DDevice->CreateRasterizerState(
        &drawingRenderStateDesc,
        &m_drawingRenderState
        )
    );
```

建立轉譯狀態，您可以用於啟用背面消除。 如果您的轉譯程式碼已開啟背面消除，則可略過此步驟。

```cpp
D3D11_RASTERIZER_DESC shadowRenderStateDesc;
ZeroMemory(&shadowRenderStateDesc, sizeof(D3D11_RASTERIZER_DESC));
shadowRenderStateDesc.CullMode = D3D11_CULL_FRONT;
shadowRenderStateDesc.FillMode = D3D11_FILL_SOLID;
shadowRenderStateDesc.DepthClipEnable = true;

DX::ThrowIfFailed(
    pD3DDevice->CreateRasterizerState(
        &shadowRenderStateDesc,
        &m_shadowRenderState
        )
    );
```

## <a name="create-constant-buffers"></a>建立常數緩衝區


記得建立用於從光源視角轉譯的常數緩衝區。 您也可以使用此常數緩衝區來指定著色器的光源位置。 在點狀光源使用透視矩陣，並在方向光源 (如日光) 使用正交矩陣。

```cpp
DX::ThrowIfFailed(
    m_deviceResources->GetD3DDevice()->CreateBuffer(
        &viewProjectionConstantBufferDesc,
        nullptr,
        &m_lightViewProjectionBuffer
        )
    );
```

填入常數緩衝區資料。 在初始化期間更新一次常數緩衝區；如果在先前的畫面之後變更了光線值，則再更新一次。

```cpp
{
    XMMATRIX lightPerspectiveMatrix = XMMatrixPerspectiveFovRH(
        XM_PIDIV2,
        1.0f,
        12.f,
        50.f
        );

    XMStoreFloat4x4(
        &m_lightViewProjectionBufferData.projection,
        XMMatrixTranspose(lightPerspectiveMatrix)
        );

    // Point light at (20, 15, 20), pointed at the origin. POV up-vector is along the y-axis.
    static const XMVECTORF32 eye = { 20.0f, 15.0f, 20.0f, 0.0f };
    static const XMVECTORF32 at = { 0.0f, 0.0f, 0.0f, 0.0f };
    static const XMVECTORF32 up = { 0.0f, 1.0f, 0.0f, 0.0f };

    XMStoreFloat4x4(
        &m_lightViewProjectionBufferData.view,
        XMMatrixTranspose(XMMatrixLookAtRH(eye, at, up))
        );

    // Store the light position to help calculate the shadow offset.
    XMStoreFloat4(&m_lightViewProjectionBufferData.pos, eye);
}
```

```cpp
context->UpdateSubresource(
    m_lightViewProjectionBuffer.Get(),
    0,
    NULL,
    &m_lightViewProjectionBufferData,
    0,
    0
    );
```

## <a name="create-a-viewport"></a>建立檢視區


您需要個別的檢視區以轉譯為陰影圖。 檢視區不是裝置型的資源；您可在程式碼中的任意位置建立。 在建立陰影圖時一起建立檢視區，有利於將檢視區維度與陰影圖維度保持一致。

```cpp
// Init viewport for shadow rendering
ZeroMemory(&m_shadowViewport, sizeof(D3D11_VIEWPORT));
m_shadowViewport.Height = m_shadowMapDimension;
m_shadowViewport.Width = m_shadowMapDimension;
m_shadowViewport.MinDepth = 0.f;
m_shadowViewport.MaxDepth = 1.f;
```

在此逐步解說的下個部分，您將了解如何藉由[轉譯為深度緩衝區](render-the-shadow-map-to-the-depth-buffer.md)來建立陰影圖。

 

 