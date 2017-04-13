
<properties
    pageTitle="Migrace uživatelských dat z Azure RemoteApp | Microsoft Azure"
    description="Naučte se migrovat uživatelských dat a odhlášení z Azure RemoteApp."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="how-to-migrate-data-into-and-out-of-azure-remoteapp"></a>Jak migrovat data do a z Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.

Můžete mnoho různých nástrojů a metody přenosu [uživatelská data](remoteapp-upd.md) do a z Azure RemoteApp. Tady je několik způsobů:

- Kopírování a vkládání pomocí schránky sdílení
- Kopírování souborů a dat na souborovém serveru
- Kopírování souborů na OneDrive pro firmy pomocí prohlížeče
- Kopírování souborů pomocí přesměrování

>[AZURE.NOTE] 
> Není možné povolit OneDrive pro firmy nebo spotř synchronizace agenti – budou v Azure RemoteApp [nejsou podporované](remoteapp-onedrive.md) .

## <a name="use-copy-and-paste-in-file-explorer"></a>Použít kopírování a vkládání v Průzkumníku souborů

Kopírování a vkládání pomocí schránky aktivované RemoteApp nasazení [ve výchozím nastavení](remoteapp-redirection.md). To umožňuje uživatelům kopírovat soubory mezi místním počítači a RemoteApp aplikace. Často prostřednictvím běžné používání aplikací v RemoteApp, uživatelé si uložili souborů na jejich UPDs - přesunutí dat z RemoteApp je snadné:

1. [Publikování Průzkumníka souborů jako aplikace](remoteapp-publish.md) v kolekci RemoteApp. (Vezměte v úvahu, že se jedná o Správce úloh.)
2. Přímé uživatelů a spusťte aplikaci Průzkumníka souborů, které jste publikovali, který umožňuje kopírujte a vkládejte soubory do jejich UDP a zrušení.

## <a name="upload-files-and-data-to-a-file-server-by-using-standard-network-file-copy"></a>Nahrajte soubory a data na souborovém serveru pomocí kopírování souborů standardní sítě

Často organizace umožňuje servery soubor uložit obecné data. Pokud znáte název serveru nebo umístění, vaši uživatelé můžete procházet místní síti pro server a potom zkopírujte svoje soubory, podobně jako výše. Znovu budete chtít publikujete RemoteApp Průzkumníka souborů a sdílejte ho s jinými uživateli.

>[AZURE.NOTE] 
> Souborového serveru musí být v síti směrovatelné RemoteApp nasazené do.

## <a name="copy-files-to-onedrive-for-business"></a>Kopírování souborů na Onedrivu pro firmy
I když není možné povolit OneDrive pro firmy synchronizace agent v RemoteApp, můžete kopírovat soubory ze svého UDP na OneDrive pro firmy prostřednictvím prohlížeče. 

1. Publikování Průzkumníka souborů na RemoteApp a sdělte uživatelům přístup k souborům pomocí této aplikace. 
2. Je nejjednodušší přenos souborů, pokud jsou komprimovány, tak, aby uživatelé měli vytvořit zip soubor, který obsahuje všechny soubory, přejděte na OneDrive pro firmy.
3. Požádejte uživatele, přejděte na portál Office 365 a přejděte na OneDrive nahrát soubor ZIP.

## <a name="copy-files-by-using-drive-redirection"></a>Kopírování souborů pomocí přesměrování jednotky

Pokud jste povolili [přesměrování jednotky](remoteapp-redirection.md), jste již vytvořili namapované jednotky pro uživatele. V takovém případě můžete svoje soubory na tato jednotka zip a uložte je do místního počítače.