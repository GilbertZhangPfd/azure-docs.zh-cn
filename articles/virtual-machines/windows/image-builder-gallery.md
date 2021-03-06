---
title: 将 Azure 映像生成器用于 Windows Vm 的映像库（预览）
description: 通过 Azure 映像生成器和共享映像库创建 Windows VM 映像。
author: cynthn
ms.author: cynthn
ms.date: 05/02/2019
ms.topic: article
ms.service: virtual-machines-windows
manager: gwallace
ms.openlocfilehash: 1d9763ccc5f5967b9fc9932a11fff655e6120fd0
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/10/2019
ms.locfileid: "74976071"
---
# <a name="preview-create-a-windows-image-and-distribute-it-to-a-shared-image-gallery"></a>预览：创建 Windows 映像并将其分发给共享映像库 

本文介绍如何使用 Azure 映像生成器在[共享映像库](shared-image-galleries.md)中创建映像版本，并在全局分布映像。

我们将使用一个 json 模板来配置映像。 我们使用的 json 文件是： [helloImageTemplateforWinSIG](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/1_Creating_a_Custom_Win_Shared_Image_Gallery_Image/helloImageTemplateforWinSIG.json)。 

若要将映像分发到共享图像库，模板将使用[sharedImage](../linux/image-builder-json.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#distribute-sharedimage)作为模板的 `distribute` 部分的值。

> [!IMPORTANT]
> Azure 映像生成器目前为公共预览版。
> 此预览版在提供时没有附带服务级别协议，不建议将其用于生产工作负荷。 某些功能可能不受支持或者受限。 有关详细信息，请参阅 [Microsoft Azure 预览版补充使用条款](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)。

## <a name="register-the-features"></a>注册功能
若要在预览期间使用 Azure 映像生成器，需要注册新功能。

```azurecli-interactive
az feature register --namespace Microsoft.VirtualMachineImages --name VirtualMachineTemplatePreview
```

检查功能注册的状态。

```azurecli-interactive
az feature show --namespace Microsoft.VirtualMachineImages --name VirtualMachineTemplatePreview | grep state
```

检查你的注册。

```azurecli-interactive
az provider show -n Microsoft.VirtualMachineImages | grep registrationState
az provider show -n Microsoft.Storage | grep registrationState
az provider show -n Microsoft.Compute | grep registrationState
```

如果未注册，请运行以下内容：

```azurecli-interactive
az provider register -n Microsoft.VirtualMachineImages
az provider register -n Microsoft.Storage
az provider register -n Microsoft.Compute
```

## <a name="set-variables-and-permissions"></a>设置变量和权限 

我们将重复使用某些信息，因此我们将创建一些变量来存储该信息。 将变量的值（如 `username` 和 `vmpassword`）替换为自己的信息。

```azurecli-interactive
# Resource group name - we are using ibsigRG in this example
sigResourceGroup=myIBWinRG
# Datacenter location - we are using West US 2 in this example
location=westus
# Additional region to replicate the image to - we are using East US in this example
additionalregion=eastus
# name of the shared image gallery - in this example we are using myGallery
sigName=my22stSIG
# name of the image definition to be created - in this example we are using myImageDef
imageDefName=winSvrimages
# image distribution metadata reference name
runOutputName=w2019SigRo
# User name and password for the VM
username="azureuser"
vmpassword="passwordfortheVM"
```

为订阅 ID 创建一个变量。 可以使用 `az account show | grep id`获取此。

```azurecli-interactive
subscriptionID="Subscription ID"
```

创建资源组。

```azurecli-interactive
az group create -n $sigResourceGroup -l $location
```


向 Azure 映像生成器授予在该资源组中创建资源的权限。 `--assignee` 值是映像生成器服务的应用注册 ID。 

```azurecli-interactive
az role assignment create \
    --assignee cf32a0cc-373c-47c9-9156-0db11f6a6dfc \
    --role Contributor \
    --scope /subscriptions/$subscriptionID/resourceGroups/$sigResourceGroup
```


## <a name="create-an-image-definition-and-gallery"></a>创建映像定义和库

若要将图像生成器用于共享图像库，需要具有现有的映像库和映像定义。 映像生成器将不会为您创建映像库和映像定义。

如果还没有要使用的库和图像定义，请首先创建它们。 首先，创建一个映像库。

```azurecli-interactive
az sig create \
    -g $sigResourceGroup \
    --gallery-name $sigName
```

然后，创建映像定义。

```azurecli-interactive
az sig image-definition create \
   -g $sigResourceGroup \
   --gallery-name $sigName \
   --gallery-image-definition $imageDefName \
   --publisher corpIT \
   --offer myOffer \
   --sku 2019 \
   --os-type Windows
```


## <a name="download-and-configure-the-json"></a>下载并配置 json

下载 json 模板并将其配置为你的变量。

```azurecli-interactive
curl https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/1_Creating_a_Custom_Win_Shared_Image_Gallery_Image/helloImageTemplateforWinSIG.json -o helloImageTemplateforWinSIG.json
sed -i -e "s/<subscriptionID>/$subscriptionID/g" helloImageTemplateforWinSIG.json
sed -i -e "s/<rgName>/$sigResourceGroup/g" helloImageTemplateforWinSIG.json
sed -i -e "s/<imageDefName>/$imageDefName/g" helloImageTemplateforWinSIG.json
sed -i -e "s/<sharedImageGalName>/$sigName/g" helloImageTemplateforWinSIG.json
sed -i -e "s/<region1>/$location/g" helloImageTemplateforWinSIG.json
sed -i -e "s/<region2>/$additionalregion/g" helloImageTemplateforWinSIG.json
sed -i -e "s/<runOutputName>/$runOutputName/g" helloImageTemplateforWinSIG.json
```

## <a name="create-the-image-version"></a>创建映像版本

下一部分将在库中创建映像版本。 

将映像配置提交到 Azure 映像生成器服务。

```azurecli-interactive
az resource create \
    --resource-group $sigResourceGroup \
    --properties @helloImageTemplateforWinSIG.json \
    --is-full-object \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateforWinSIG01
```

启动映像生成。

```azurecli-interactive
az resource invoke-action \
     --resource-group $sigResourceGroup \
     --resource-type  Microsoft.VirtualMachineImages/imageTemplates \
     -n helloImageTemplateforWinSIG01 \
     --action Run 
```

创建图像并将其复制到这两个区域可能需要一段时间。 等待此部分完成，然后再继续创建 VM。


## <a name="create-the-vm"></a>创建 VM

使用 Azure 映像生成器创建的映像版本创建 VM。

```azurecli-interactive
az vm create \
  --resource-group $sigResourceGroup \
  --name aibImgWinVm001 \
  --admin-username $username \
  --admin-password $vmpassword \
  --image "/subscriptions/$subscriptionID/resourceGroups/$sigResourceGroup/providers/Microsoft.Compute/galleries/$sigName/images/$imageDefName/versions/latest" \
  --location $location
```


## <a name="verify-the-customization"></a>验证自定义
使用创建 VM 时设置的用户名和密码创建与 VM 的远程桌面连接。 在 VM 中，打开 cmd 提示符并键入：

```console
dir c:\
```

应该会看到一个名为 `buildActions` 的目录，该目录是在映像自定义期间创建的。


## <a name="clean-up-resources"></a>清理资源
如果现在想要尝试重新自定义映像版本来创建同一映像的新版本，请**跳过此步骤**，并继续[使用 Azure 映像生成器创建另一个映像版本](image-builder-gallery-update-image-version.md)。


这将删除已创建的映像以及所有其他资源文件。 请确保在删除资源之前已经完成了此部署。

删除映像库资源时，需要先删除所有映像版本，然后才能删除用于创建它们的映像定义。 若要删除库，首先需要删除库中的所有图像定义。

删除图像生成器模板。

```azurecli-interactive
az resource delete \
    --resource-group $sigResourceGroup \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateforWinSIG01
```

获取映像生成器创建的映像版本，此版本始终以 `0.`开头，然后删除映像版本

```azurecli-interactive
sigDefImgVersion=$(az sig image-version list \
   -g $sigResourceGroup \
   --gallery-name $sigName \
   --gallery-image-definition $imageDefName \
   --subscription $subscriptionID --query [].'name' -o json | grep 0. | tr -d '"')
az sig image-version delete \
   -g $sigResourceGroup \
   --gallery-image-version $sigDefImgVersion \
   --gallery-name $sigName \
   --gallery-image-definition $imageDefName \
   --subscription $subscriptionID
```   


删除映像定义。

```azurecli-interactive
az sig image-definition delete \
   -g $sigResourceGroup \
   --gallery-name $sigName \
   --gallery-image-definition $imageDefName \
   --subscription $subscriptionID
```

删除库。

```azurecli-interactive
az sig delete -r $sigName -g $sigResourceGroup
```

删除该资源组。

```azurecli-interactive
az group delete -n $sigResourceGroup -y
```

## <a name="next-steps"></a>后续步骤

若要了解如何更新所创建的映像版本，请参阅[使用 Azure 映像生成器创建另一个映像版本](image-builder-gallery-update-image-version.md)。