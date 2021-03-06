---
title: 紋理區塊壓縮
description: 紋理的區塊壓縮 (BC) 支援在 Direct3D 11 中已進行擴充，內含 BC6H 和 BC7 演算法。
ms.assetid: 63506C46-BF14-464B-B20C-8B8F359E7AFE
keywords:
- 紋理區塊壓縮
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 06c132f23d22dc688f82ff7976bcb93108f2ee1c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158812"
---
# <a name="texture-block-compression"></a>紋理區塊壓縮


紋理的區塊壓縮 (BC) 支援在 Direct3D 11 中已進行擴充，內含 BC6H 和 BC7 演算法。 BC6H 支援高動態範圍色彩來源資料，而 BC7 使用較少的標準 RGB 來源資料成品，提供高於平均品質的壓縮。

如需關於 Direct3D 11 之前區塊壓縮演算法支援 (包括 BC1 到 BC5 格式的支援) 的相關詳細資訊，請參閱[區塊壓縮 (Direct3D 10)](/windows/desktop/direct3d10/d3d10-graphics-programming-guide-resources-block-compression)。

**檔案格式的注意事項：  ** BC6H 和 BC7 材質壓縮格式會使用 DDS 檔案格式來儲存壓縮的材質資料。 如需詳細資訊，請參閱[DDS 程式設計指南](/windows/desktop/direct3ddds/dx-graphics-dds-pguide)。

## <a name="span-idblock_compression_formats_supported_in_direct3d_11spanspan-idblock_compression_formats_supported_in_direct3d_11spanspan-idblock_compression_formats_supported_in_direct3d_11spanblock-compression-formats-supported-in-direct3d-11"></a><span id="Block_Compression_Formats_Supported_in_Direct3D_11"></span><span id="block_compression_formats_supported_in_direct3d_11"></span><span id="BLOCK_COMPRESSION_FORMATS_SUPPORTED_IN_DIRECT3D_11"></span>Direct3D 11 中支援的區塊壓縮格式


| 來源資料                                  | 資料壓縮解析度的最低需求                              | 建議格式 | 最低支援的功能層級 |
|----------------------------------------------|---------------------------------------------------------------------------|--------------------|---------------------------------|
| 帶有 Alpha 色板的三通道色彩       | 帶有 0 或 1 個位元 Alpha 的三色彩通道 (5 個位元：6 個位元：5 個位元)  | BC1                | Direct3D 9.1                    |
| 帶有 Alpha 色板的三通道色彩       | 帶有 4 個位元 Alpha 的三色彩通道 (5 個位元：6 個位元：5 個位元)         | BC2                | Direct3D 9.1                    |
| 帶有 Alpha 色板的三通道色彩       | 帶有 8 個位元 Alpha 的三色彩通道 (5 個位元：6 個位元：5 個位元)          | BC3                | Direct3D 9.1                    |
| 單一通道色彩                            | 單一色彩通道 (8 個位元)                                                | BC4                | Direct3D 10                     |
| 雙通道色彩                            | 兩個色彩通道 (8 個位元：8 個位元)                                        | BC5                | Direct3D 10                     |
| 三通道高動態範圍 (HDR) 色彩 | 三色通道 (16 位：16位：16位) "半" 浮點數\* | BC6H               | Direct3D 11                     |
| 三通道色彩，及選擇性的 Alpha 色板  | 帶有 0 到 8 個位元 Alpha 的三色彩通道 (每個通道 4 到 7 個位元)  | BC7                | Direct3D 11                     |

 

\*「半」浮點數是16位值，其中包含選擇性的正負號位、5位偏差指數，以及10或11位尾數。
## <a name="span-idbc1__bc2__and_b3_formatsspanspan-idbc1__bc2__and_b3_formatsspanspan-idbc1__bc2__and_b3_formatsspanbc1-bc2-and-b3-formats"></a><span id="BC1__BC2__and_B3_Formats"></span><span id="bc1__bc2__and_b3_formats"></span><span id="BC1__BC2__AND_B3_FORMATS"></span>BC1、BC2 和 B3 格式


BC1、BC2，以及 BC3 格式對等於 Direct3D 9 DXTn 紋理壓縮格式，並且各自對應到 Direct3D 10 BC1、BC2，及 BC3 格式。 所有功能層級都需要這三種格式的支援 (D3D \_ 功能 \_ 等級 \_ 9 \_ 1、d3d \_ 功能 \_ 層級 \_ 9 \_ 2、d3d \_ 功能 \_ 層級 \_ 9 \_ 3、d3d \_ 功能 \_ 等級 \_ 10 \_ 0、d3d \_ 功能 \_ 層級 \_ 10 \_ 1，以及 d3d \_ 功能 \_ 等級 \_ 11 \_ 0) 。

| 區塊壓縮格式 | DXGI 格式                                                                           | Direct3D 9 對等格式                               | 每個 4 x 4 像素區塊的位元組 |
|--------------------------|---------------------------------------------------------------------------------------|------------------------------------------------------------|---------------------------|
| BC1                      | DXGI \_ 格式 \_ BC1 \_ UNORM、dxgi \_ 格式 \_ BC1 \_ UNORM \_ SRGB、dxgi \_ 格式 \_ BC1 無格式 \_ | D3DFMT \_ DXT1，FourCC = "DXT1"                                | 8                         |
| BC2                      | DXGI \_ 格式 \_ BC2 \_ UNORM、dxgi \_ 格式 \_ BC2 \_ UNORM \_ SRGB、dxgi \_ 格式 \_ BC2 無格式 \_ | D3DFMT \_ DXT2 \* 、FourCC = "DXT2"、D3DFMT \_ DXT3、FOURCC = "DXT3" | 16                        |
| BC3                      | DXGI \_ 格式 \_ BC3 \_ UNORM、dxgi \_ 格式 \_ BC3 \_ UNORM \_ SRGB、dxgi \_ 格式 \_ BC3 無格式 \_ | D3DFMT \_ DXT4 \* 、FourCC = "DXT4"、D3DFMT \_ DXT5、FOURCC = "DXT5" | 16                        |

 

\*這些壓縮配置 (DXT2 和 DXT4) 不區分 Direct3D 9 預乘的 Alpha 格式與標準的 Alpha 格式。 此區別必須透過可程式化著色器於轉譯時間中進行處理。

## <a name="span-idbc4_and_bc5_formatsspanspan-idbc4_and_bc5_formatsspanspan-idbc4_and_bc5_formatsspanbc4-and-bc5-formats"></a><span id="BC4_and_BC5_Formats"></span><span id="bc4_and_bc5_formats"></span><span id="BC4_AND_BC5_FORMATS"></span>BC4 及 BC5 格式


| 區塊壓縮格式 | DXGI 格式                                                                     | Direct3D 9 對等格式 | 每個 4 x 4 像素區塊的位元組 |
|--------------------------|---------------------------------------------------------------------------------|------------------------------|---------------------------|
| BC4                      | DXGI \_ 格式 \_ BC4 \_ UNORM、dxgi \_ 格式 \_ BC4 \_ SNORM、dxgi \_ 格式 \_ BC4 無格式 \_ | FourCC="ATI1"                | 8                         |
| BC5                      | DXGI \_ 格式 \_ BC5 \_ UNORM、dxgi \_ 格式 \_ BC5 \_ SNORM、dxgi \_ 格式 \_ BC5 無格式 \_ | FourCC="ATI2"                | 16                        |

 

## <a name="span-idbc6h_formatspanspan-idbc6h_formatspanspan-idbc6h_formatspanbc6h-format"></a><span id="BC6H_Format"></span><span id="bc6h_format"></span><span id="BC6H_FORMAT"></span>BC6H 格式


如需關於此格式的詳細資訊，請參閱[BC6H 格式](/windows/desktop/direct3d11/bc6h-format)文件。

| 區塊壓縮格式 | DXGI 格式                                                                      | Direct3D 9 對等格式 | 每個 4 x 4 像素區塊的位元組 |
|--------------------------|----------------------------------------------------------------------------------|------------------------------|---------------------------|
| BC6H                     | DXGI \_ 格式 \_ BC6H \_ UF16、dxgi \_ 格式 \_ BC6H \_ SF16、dxgi \_ 格式 \_ BC6H 無格式 \_ | N/A                          | 16                        |

 

BC6H 格式針對每個 4 x 4 的像素區塊可以選擇不同的編碼模式。 共計有 14 個不同的編碼模式可以使用，每一種模式對於顯示紋理的最終視覺品質都有些許不同的取捨。 根據來源內容選擇或配合的品質等級，模式的選擇可允許硬體進行更快速的解碼，然而這也會大幅增加搜尋空間的複雜度。

## <a name="span-idbc7_formatspanspan-idbc7_formatspanspan-idbc7_formatspanbc7-format"></a><span id="BC7_Format"></span><span id="bc7_format"></span><span id="BC7_FORMAT"></span>BC7 格式


如需關於此格式的詳細資訊，請參閱[BC7 格式](/windows/desktop/direct3d11/bc7-format)文件。

| 區塊壓縮格式 | DXGI 格式                                                                           | Direct3D 9 對等格式 | 每個 4 x 4 像素區塊的位元組 |
|--------------------------|---------------------------------------------------------------------------------------|------------------------------|---------------------------|
| BC7                      | DXGI \_ 格式 \_ BC7 \_ UNORM、dxgi \_ 格式 \_ BC7 \_ UNORM \_ SRGB、dxgi \_ 格式 \_ BC7 無格式 \_ | N/A                          | 16                        |

 

BC7 格式針對每個 4 x 4 的像素區塊可以選擇不同的編碼模式。 共計有 8 個不同的編碼模式可以使用，每一種模式對於顯示紋理的最終視覺品質都有些許不同的取捨。 根據來源內容選擇或配合的品質等級，模式的選擇可允許硬體進行更快速的解碼，然而這也會大幅增加搜尋空間的複雜度。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[附錄](appendix.md)

[紋理](/windows/desktop/direct3d11/overviews-direct3d-11-resources-textures)

 

 