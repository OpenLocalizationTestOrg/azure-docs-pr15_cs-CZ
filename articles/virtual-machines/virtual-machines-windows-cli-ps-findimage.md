<properties
   pageTitle="Vyhledejte a vyberte obrázky OM Windows | Microsoft Azure"
   description="Naučte se při vytváření virtuálních počítačů Windows s nasazení modelu správce prostředků zjistit Publisheru, nabídky a SKU pro obrázky."
   services="virtual-machines-windows"
   documentationCenter=""
   authors="squillace"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"
   />

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure"
   ms.date="08/23/2016"
   ms.author="rasquill"/>

# <a name="navigate-and-select-windows-virtual-machine-images-in-azure-with-powershell-or-the-cli"></a>Vyhledejte a vyberte obrázky virtuálního počítače Windows Azure pomocí prostředí PowerShell nebo rozhraní příkazového řádku

Toto téma popisuje, jak můžete najít obrázek vydavatelé OM, nabídky, skladové jednotky a verze pro každé umístění nasazení může. Vytvořte příklad, jsou některé často používané obrázky OM Windows:

## <a name="table-of-commonly-used-windows-images"></a>Seznam citací běžně používaných Windows obrázky


| PublisherName                        | Nabídka                                 | SKU                         |
|:---------------------------------|:-------------------------------------------|:---------------------------------|:--------------------|
| MicrosoftDynamicsNAV             | DynamicsNAV                                | 2015                             |
| MicrosoftSharePoint              | MicrosoftSharePointServer                  | 2013                             |
| MicrosoftSQLServer               | SQL2014 WS2012R2                           | Pole organizace – optimalizované pro DW      |
| MicrosoftSQLServer               | SQL2014 WS2012R2                           | Pole organizace – optimalizované pro OLTP    |
| MicrosoftWindowsServer           | WindowsServer                              | 2012 R2 datacentru                  |
| MicrosoftWindowsServer           | WindowsServer                              | Datacentra 2012               |
| MicrosoftWindowsServer           | WindowsServer                              | 2008 R2 SP1 |
| MicrosoftWindowsServer           | WindowsServer                              | Windows Server Technical Preview |
| MicrosoftWindowsServerEssentials | WindowsServerEssentials                    | WindowsServerEssentials          |
| MicrosoftWindowsServerHPCPack    | WindowsServerHPCPack                       | 2012R2                           |


[AZURE.INCLUDE [virtual-machines-common-cli-ps-findimage](../../includes/virtual-machines-common-cli-ps-findimage.md)]
