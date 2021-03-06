---
title: 在代码中使用 SSL 证书
description: 了解如何在代码中使用客户端证书。 使用客户端证书向远程资源进行身份验证，或使用它们运行加密任务。
ms.topic: article
ms.date: 11/04/2019
ms.reviewer: yutlin
ms.custom: seodec18
ms.openlocfilehash: d783b61c372c7d0f8cca13106bf297ab9b55c424
ms.sourcegitcommit: 265f1d6f3f4703daa8d0fc8a85cbd8acf0a17d30
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2019
ms.locfileid: "74671896"
---
# <a name="use-an-ssl-certificate-in-your-code-in-azure-app-service"></a>在代码中使用 SSL 证书 Azure App Service

在应用程序代码中，你可以访问[添加到应用服务的公共或私有证书](configure-ssl-certificate.md)。 应用代码可充当客户端和访问需要证书身份验证的外部服务，或者可能需要执行加密任务。 本操作方法指南演示如何在应用程序代码中使用公共或私有证书。

在代码中使用证书的这种方法将使用应用服务中的 SSL 功能，这要求你的应用位于**基本**层或更高级别。 如果你的应用处于 "**免费**" 或 "**共享**" 层，你可以[在应用存储库中包含该证书文件](#load-certificate-from-file)。

让应用服务管理 SSL 证书时，可以分开维护证书和应用程序代码，并保护敏感数据。

## <a name="prerequisites"></a>必备组件

按照本操作方法指南操作：

- [创建应用服务应用](/azure/app-service/)
- [将证书添加到应用](configure-ssl-certificate.md)

## <a name="find-the-thumbprint"></a>查找指纹

在 <a href="https://portal.azure.com" target="_blank">Azure 门户</a>的左侧菜单中，选择“应用服务” > “\<app-name>”。

在应用程序的左侧导航栏中，选择 " **TLS/SSL 设置**"，然后选择 "**私钥证书（.pfx）** " 或 "**公钥证书（.cer）** "。

查找要使用的证书，并复制指纹。

![复制证书指纹](./media/configure-ssl-certificate/create-free-cert-finished.png)

## <a name="make-the-certificate-accessible"></a>使证书可访问

若要在应用代码中访问证书，请在<a target="_blank" href="https://shell.azure.com" >Cloud Shell</a>中运行以下命令，将其指纹添加到 `WEBSITE_LOAD_CERTIFICATES` 应用设置：

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings WEBSITE_LOAD_CERTIFICATES=<comma-separated-certificate-thumbprints>
```

若要使所有证书都可访问，请将值设置为 `*`。

## <a name="load-certificate-in-windows-apps"></a>在 Windows 应用中加载证书

`WEBSITE_LOAD_CERTIFICATES` 应用设置使指定证书可供 windows 证书存储中的 Windows 托管应用程序访问，位置取决于[定价层](overview-hosting-plans.md)：

- **隔离**层-位于[本地 Machine\My](/windows-hardware/drivers/install/local-machine-and-current-user-certificate-stores)。 
- 所有其他层-在[当前 User\My](/windows-hardware/drivers/install/local-machine-and-current-user-certificate-stores)中。

在C#代码中，可以通过证书指纹访问证书。 以下代码加载具有指纹 `E661583E8FABEF4C0BEF694CBC41C28FB81CD870` 的证书。

```csharp
using System;
using System.Security.Cryptography.X509Certificates;

...
X509Store certStore = new X509Store(StoreName.My, StoreLocation.CurrentUser);
certStore.Open(OpenFlags.ReadOnly);
X509Certificate2Collection certCollection = certStore.Certificates.Find(
                            X509FindType.FindByThumbprint,
                            // Replace below with your certificate's thumbprint
                            "E661583E8FABEF4C0BEF694CBC41C28FB81CD870",
                            false);
// Get the first cert with the thumbprint
if (certCollection.Count > 0)
{
    X509Certificate2 cert = certCollection[0];
    // Use certificate
    Console.WriteLine(cert.FriendlyName);
}
certStore.Close();
...
```

在 Java 代码中，可以使用 "使用者公用名" 字段从 "Windows-MY" 存储区访问证书（请参阅[公钥证书](https://en.wikipedia.org/wiki/Public_key_certificate)）。 下面的代码演示如何加载私钥证书：

```java
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.bind.annotation.RequestMapping;
import java.security.KeyStore;
import java.security.cert.Certificate;
import java.security.PrivateKey;

...
KeyStore ks = KeyStore.getInstance("Windows-MY");
ks.load(null, null); 
Certificate cert = ks.getCertificate("<subject-cn>");
PrivateKey privKey = (PrivateKey) ks.getKey("<subject-cn>", ("<password>").toCharArray());

// Use the certificate and key
...
```

对于不支持或未提供对 Windows 证书存储区的支持的语言，请参阅[从文件中加载证书](#load-certificate-from-file)。

## <a name="load-certificate-in-linux-apps"></a>在 Linux 应用中加载证书

`WEBSITE_LOAD_CERTIFICATES` 应用设置使 Linux 托管应用（包括自定义容器应用）可以访问指定的证书作为文件。 这些文件位于以下目录中：

- 专用证书-`/var/ssl/private` （`.p12` 文件）
- 公共证书-`/var/ssl/certs` （`.der` 文件）

证书文件名是证书指纹。 以下C#代码演示了如何在 Linux 应用中加载公共证书。

```csharp
using System;
using System.Security.Cryptography.X509Certificates;

...
var bytes = System.IO.File.ReadAllBytes("/var/ssl/certs/<thumbprint>.der");
var cert = new X509Certificate2(bytes);

// Use the loaded certificate
```

若要查看如何从 node.js、PHP、Python、Java 或 Ruby 中的文件加载 SSL 证书，请参阅相应语言或 web 平台的文档。

## <a name="load-certificate-from-file"></a>从文件加载证书

例如，如果需要加载手动上传的证书文件，则最好使用[FTPS](deploy-ftp.md)而不是[Git](deploy-local-git.md)来上载证书。 你应将敏感数据（如私有证书）从源代码管理中排除。

> [!NOTE]
> 即使您从文件中加载证书，Windows 上的 ASP.NET 和 ASP.NET Core 也必须访问证书存储区。 若要在 Windows .NET 应用中加载证书文件，请在<a target="_blank" href="https://shell.azure.com" >Cloud Shell</a>中使用以下命令加载当前用户配置文件：
>
> ```azurecli-interactive
> az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings WEBSITE_LOAD_USER_PROFILE=1
> ```
>
> 在代码中使用证书的这种方法将使用应用服务中的 SSL 功能，这要求你的应用位于**基本**层或更高级别。

以下C#示例从应用中的相对路径加载公共证书：

```csharp
using System;
using System.Security.Cryptography.X509Certificates;

...
var bytes = System.IO.File.ReadAllBytes("~/<relative-path-to-cert-file>");
var cert = new X509Certificate2(bytes);

// Use the loaded certificate
```

若要查看如何从 node.js、PHP、Python、Java 或 Ruby 中的文件加载 SSL 证书，请参阅相应语言或 web 平台的文档。

## <a name="more-resources"></a>更多资源

* [使用 SSL 绑定保护自定义 DNS 名称](configure-ssl-bindings.md)
* [实施 HTTPS](configure-ssl-bindings.md#enforce-https)
* [实施 TLS 1.1/1.2](configure-ssl-bindings.md#enforce-tls-versions)
* [常见问题解答：应用服务证书](https://docs.microsoft.com/azure/app-service/faq-configuration-and-management/)
