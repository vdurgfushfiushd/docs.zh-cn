---
title: 如何：更改 X.509 证书私钥的加密提供程序
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- cryptographic provider [WCF], changing
- cryptographic provider [WCF]
ms.assetid: b4254406-272e-4774-bd61-27e39bbb6c12
ms.openlocfilehash: 9d7216b3aed89dc88737cc346386d6b03929fe60
ms.sourcegitcommit: 0be8a279af6d8a43e03141e349d3efd5d35f8767
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/18/2019
ms.locfileid: "59295570"
---
# <a name="how-to-change-the-cryptographic-provider-for-an-x509-certificates-private-key"></a>如何：更改 X.509 证书私钥的加密提供程序
本主题说明如何更改用于提供 X.509 证书的私钥的加密提供程序以及如何将提供程序集成到 Windows Communication Foundation (WCF) 安全框架。 有关使用证书的详细信息，请参阅[Working with Certificates](../../../../docs/framework/wcf/feature-details/working-with-certificates.md)。  
  
 WCF 安全框架提供了一种引入新的安全令牌类型，如中所述[如何：创建自定义令牌](../../../../docs/framework/wcf/extending/how-to-create-a-custom-token.md)。 还可以使用自定义令牌替换系统提供的现有令牌类型。  
  
 在本主题中，系统提供的 X.509 安全令牌替换为自定义 X.509 令牌，此自定义令牌提供了一种不同的证书私钥的实现。 当实际私钥是由默认 Windows 加密提供程序之外的其他加密提供程序提供时，这是非常有用的。 可选的加密提供程序的一个示例是硬件安全模块，该模块执行所有私钥相关的加密操作并且没有将私钥存储在内存中，从而提高了系统的安全性。  
  
 下面的示例仅供演示使用。 它不会替换默认的 Windows 加密提供程序，而只是演示在哪种情况下可以集成这样一个提供程序。  
  
## <a name="procedures"></a>过程  
 每个具有一个或多个关联的安全密钥的安全令牌都必须实现 <xref:System.IdentityModel.Tokens.SecurityToken.SecurityKeys%2A> 属性，以便从此安全令牌实例返回一个密钥集合。 如果此令牌为 X.509 安全令牌，则集合包含一个 <xref:System.IdentityModel.Tokens.X509AsymmetricSecurityKey> 类的实例，此类表示与证书关联的公钥和私钥。 若要替换用于提供证书密钥的默认的加密提供程序，请创建此类的一个新实现。  
  
#### <a name="to-create-a-custom-x509-asymmetric-key"></a>创建自定义 X.509 非对称密钥  
  
1. 定义一个从 <xref:System.IdentityModel.Tokens.X509AsymmetricSecurityKey> 类派生的新类。  
  
2. 重写 <xref:System.IdentityModel.Tokens.SecurityKey.KeySize%2A> 只读属性。 此属性返回证书的公钥/私钥对的实际密钥大小。  
  
3. 重写 <xref:System.IdentityModel.Tokens.SecurityKey.DecryptKey%2A> 方法。 通过使用证书的私钥对称密钥进行解密的 WCF 安全框架调用此方法。 （此密钥之前使用证书的公钥进行加密。）  
  
4. 重写 <xref:System.IdentityModel.Tokens.AsymmetricSecurityKey.GetAsymmetricAlgorithm%2A> 方法。 若要获取的实例的 WCF 安全框架调用此方法<xref:System.Security.Cryptography.AsymmetricAlgorithm>表示证书的私有或公共密钥，具体取决于参数的加密提供程序的类传递给该方法。  
  
5. 可选。 重写 <xref:System.IdentityModel.Tokens.AsymmetricSecurityKey.GetHashAlgorithmForSignature%2A> 方法。 如果需要 <xref:System.Security.Cryptography.HashAlgorithm> 类的另一个实现，请重写此方法。  
  
6. 重写 <xref:System.IdentityModel.Tokens.AsymmetricSecurityKey.GetSignatureFormatter%2A> 方法。 此方法返回与证书私钥关联的 <xref:System.Security.Cryptography.AsymmetricSignatureFormatter> 类的实例。  
  
7. 重写 <xref:System.IdentityModel.Tokens.SecurityKey.IsSupportedAlgorithm%2A> 方法。 此方法用于指示安全密钥实现是否支持某个特定的加密算法。  
  
     [!code-csharp[c_CustomX509Token#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customx509token/cs/source.cs#1)]
     [!code-vb[c_CustomX509Token#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customx509token/vb/source.vb#1)]  
  
 以下过程显示了如何将集成自定义 X.509 非对称安全密钥实现 WCF 安全框架使用在前面过程中创建以替换系统提供的 X.509 安全令牌。  
  
#### <a name="to-replace-the-system-provided-x509-security-token-with-a-custom-x509-asymmetric-security-key-token"></a>将系统提供的 X.509 安全令牌替换为自定义 X.509 非对称安全密钥令牌  
  
1. 创建一个自定义 X.509 安全令牌，此令牌返回自定义 X.509 非对称安全密钥而不是系统提供的安全密钥，如下面的示例所示。 有关自定义安全令牌的详细信息，请参阅[如何：创建自定义令牌](../../../../docs/framework/wcf/extending/how-to-create-a-custom-token.md)。  
  
     [!code-csharp[c_CustomX509Token#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customx509token/cs/source.cs#2)]
     [!code-vb[c_CustomX509Token#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customx509token/vb/source.vb#2)]  
  
2. 创建一个自定义安全令牌提供程序以返回一个自定义 X.509 安全令牌，如下面的示例所示。 有关自定义安全令牌提供程序的详细信息，请参阅[如何：创建自定义安全令牌提供程序](../../../../docs/framework/wcf/extending/how-to-create-a-custom-security-token-provider.md)。  
  
     [!code-csharp[c_CustomX509Token#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customx509token/cs/source.cs#3)]
     [!code-vb[c_CustomX509Token#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customx509token/vb/source.vb#3)]  
  
3. 如果自定义安全密钥需要在发起方端使用，请创建一个自定义客户端安全令牌管理器和若干自定义客户端凭据类，如下面的示例所示。 有关自定义客户端凭据和客户端安全令牌管理器的详细信息，请参阅[演练：创建自定义客户端和服务凭据](../../../../docs/framework/wcf/extending/walkthrough-creating-custom-client-and-service-credentials.md)。  
  
     [!code-csharp[c_CustomX509Token#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customx509token/cs/source.cs#4)]
     [!code-vb[c_CustomX509Token#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customx509token/vb/source.vb#4)]  
  
     [!code-csharp[c_CustomX509Token#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customx509token/cs/source.cs#6)]
     [!code-vb[c_CustomX509Token#6](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customx509token/vb/source.vb#6)]  
  
4. 如果自定义安全密钥需要在接收方端使用，请创建一个自定义服务安全令牌管理器和若干自定义服务凭据，如下面的示例所示。 有关自定义服务凭据和服务安全令牌管理器的详细信息，请参阅[演练：创建自定义客户端和服务凭据](../../../../docs/framework/wcf/extending/walkthrough-creating-custom-client-and-service-credentials.md)。  
  
     [!code-csharp[c_CustomX509Token#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customx509token/cs/source.cs#5)]
     [!code-vb[c_CustomX509Token#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customx509token/vb/source.vb#5)]  
  
     [!code-csharp[c_CustomX509Token#7](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customx509token/cs/source.cs#7)]
     [!code-vb[c_CustomX509Token#7](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customx509token/vb/source.vb#7)]  
  
## <a name="see-also"></a>请参阅

- <xref:System.IdentityModel.Tokens.X509AsymmetricSecurityKey>
- <xref:System.IdentityModel.Tokens.AsymmetricSecurityKey>
- <xref:System.IdentityModel.Tokens.SecurityKey>
- <xref:System.Security.Cryptography.AsymmetricAlgorithm>
- <xref:System.Security.Cryptography.HashAlgorithm>
- <xref:System.Security.Cryptography.AsymmetricSignatureFormatter>
- [演练：创建自定义客户端和服务凭据](../../../../docs/framework/wcf/extending/walkthrough-creating-custom-client-and-service-credentials.md)
- [如何：创建自定义安全令牌身份验证器](../../../../docs/framework/wcf/extending/how-to-create-a-custom-security-token-authenticator.md)
- [如何：创建自定义安全令牌提供程序](../../../../docs/framework/wcf/extending/how-to-create-a-custom-security-token-provider.md)
- [如何：创建自定义令牌](../../../../docs/framework/wcf/extending/how-to-create-a-custom-token.md)
