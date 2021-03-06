---
title: ServiceCredentials
ms.date: 03/30/2017
ms.assetid: 9c780793-4785-46f7-add9-ac1ebeadb614
ms.openlocfilehash: d9563bd3bfe067ad83bfa03e7c6375a9db933f14
ms.sourcegitcommit: 0be8a279af6d8a43e03141e349d3efd5d35f8767
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/18/2019
ms.locfileid: "59772097"
---
# <a name="servicecredentials"></a>ServiceCredentials
ServiceCredentials  
  
## <a name="syntax"></a>语法  
  
```csharp
class ServiceCredentials : Behavior  
{  
  string ClientCertificate;  
  string IssuedTokenAuthentication;  
  string Peer;  
  string SecureConversationAuthentication;  
  string ServiceCertificate;  
  string UserNameAuthentication;  
  string WindowsAuthentication;  
};  
```  
  
## <a name="methods"></a>方法  
 ServiceCredentials 类不定义任何方法。  
  
## <a name="properties"></a>属性  
 ServiceCredentials 类具有下列属性：  
  
### <a name="clientcertificate"></a>ClientCertificate  
 数据类型：String  
  
 访问类型：只读  
  
 此服务的客户端证书身份验证和配置设置。  
  
### <a name="issuedtokenauthentication"></a>IssuedTokenAuthentication  
 数据类型：String  
  
 访问类型：只读  
  
 当前颁发的此服务的令牌身份验证设置。  
  
### <a name="peer"></a>对等  
 数据类型：String  
  
 访问类型：只读  
  
 对等传输终结点要使用的当前凭据身份验证和配置设置。  
  
### <a name="secureconversationauthentication"></a>SecureConversationAuthentication  
 数据类型：String  
  
 访问类型：只读  
  
 指定当前安全对话设置。  
  
### <a name="servicecertificate"></a>ServiceCertificate  
 数据类型：String  
  
 访问类型：只读  
  
 与此服务关联的证书。  
  
### <a name="usernameauthentication"></a>UserNameAuthentication  
 数据类型：String  
  
 访问类型：只读  
  
 此服务的用户名/密码设置。  
  
### <a name="windowsauthentication"></a>WindowsAuthentication  
 数据类型：String  
  
 访问类型：只读  
  
 此服务的 Windows 身份验证设置。  
  
## <a name="requirements"></a>要求  
  
|MOF|已在 Servicemodel.mof 中声明。|  
|---------|-----------------------------------|  
|命名空间|已在 root\ServiceModel 中定义|  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Description.ServiceCredentials>
