---
title: IAssemblyCacheItem 接口
ms.date: 03/30/2017
api_name:
- IAssemblyCacheItem
api_location:
- fusion.dll
api_type:
- COM
f1_keywords:
- IAssemblyCacheItem
helpviewer_keywords:
- IAssemblyCacheItem interface [.NET Framework fusion]
ms.assetid: ccc9387a-9f44-4f4f-bf8f-0ea6d2afa21b
topic_type:
- apiref
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: fba17a2ffad9220acdbc79726efe0d3d4184978a
ms.sourcegitcommit: 0be8a279af6d8a43e03141e349d3efd5d35f8767
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/18/2019
ms.locfileid: "59188547"
---
# <a name="iassemblycacheitem-interface"></a>IAssemblyCacheItem 接口
表示全局程序集缓存中的单个程序集。  
  
## <a name="methods"></a>方法  
  
|方法|描述|  
|------------|-----------------|  
|[AbortItem 方法](../../../../docs/framework/unmanaged-api/fusion/iassemblycacheitem-abortitem-method.md)|允许在全局程序集缓存中的程序集以进行发布之前执行清理操作。|  
|[Commit 方法](../../../../docs/framework/unmanaged-api/fusion/iassemblycacheitem-commit-method.md)|提交对内存的缓存程序集引用。|  
|[CreateStream 方法](../../../../docs/framework/unmanaged-api/fusion/iassemblycacheitem-createstream-method.md)|创建具有指定的名称和格式的流。|  
  
## <a name="requirements"></a>要求  
 **平台：** 请参阅[系统需求](../../../../docs/framework/get-started/system-requirements.md)。  
  
 **标头：** Fusion.h  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>请参阅

- [合成接口](../../../../docs/framework/unmanaged-api/fusion/fusion-interfaces.md)
- [全局程序集缓存](../../../../docs/framework/app-domains/gac.md)
- [IAssemblyCache 接口](../../../../docs/framework/unmanaged-api/fusion/iassemblycache-interface.md)
