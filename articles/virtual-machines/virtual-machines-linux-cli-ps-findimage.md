<properties
   pageTitle="Vyberte Linux OM obrázků se rozhraní příkazového řádku Azure | Microsoft Azure"
   description="Zjistěte, jak rozhodnout, publisher, nabídky a SKU pro obrázky s při vytváření virtuálních počítačů Linux s nasazení modelu správce prostředků."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="squillace"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"
   />

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="08/23/2016"
   ms.author="rasquill"/>

# <a name="select-linux-vm-images-with-the-azure-cli"></a>Vyberte Linux OM obrázků se rozhraní příkazového řádku Azure

Toto téma popisuje, jak najít vydavatelé, nabídky, skladové jednotky a verze pro každé umístění nasazení může. Přiřadit příklad jsou některé často používané Linux OM obrázky:

**Seznam citací běžně používaných Linux obrázky**


| PublisherName                        | Nabídka                                 | SKU                         |
|:---------------------------------|:-------------------------------------------|:---------------------------------|:--------------------|
| RedHat                           | RHEL                                       | 7.2                              |
| credativ                         | Debian                                     | 8                                | 
| SUSE                             | openSUSE                                   | 13.2                             |
| SUSE                             | SLES                                       | 12 SP1                           |
| OpenLogic                        | CentOS                                     | 7.1                              |
| Kanonický                        | UbuntuServer                               | 14.04.4-LTS                      |
| Jádro operačního systému                           | Jádro operačního systému                                     | Stabilní                           |


[AZURE.INCLUDE [virtual-machines-common-cli-ps-findimage](../../includes/virtual-machines-common-cli-ps-findimage.md)]
