
<properties
    pageTitle="Nahrání vlastního obrázku pro Azure RemoteApp | Microsoft Azure"
    description="Zjistěte, jak nahrát vlastního obrázku pro Azure RemoteApp"
    services="remoteapp"
    documentationCenter=""
    authors="ericorman"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="ericor" />



# <a name="upload-a-custom-image-for-azure-remoteapp"></a>Nahrání vlastního obrázku pro Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.

Teď, když jste vytvořili vlastní šablonu obrázek nebo dokument aktualizují se změnami, budete muset nahrát tento obrázek do knihovny Azure RemoteApp obrázek. Pomocí těchto kroků.


## <a name="before-you-start"></a>Než začnete

1.      Ověřte, že vlastní obrázek splňuje [požadavky na obrázek](remoteapp-imagereqs.md) a [požadavky aplikace](remoteapp-appreqs.md).
2.      Nainstalujte [modul Azure Powershellu](../powershell-install-configure.md).

## <a name="step-by-step-on-how-to-upload-custom-image"></a>Krok za krokem o tom, jak nahrát vlastní obrázek

1.      Otevřete portál Správa Azure a přejděte na stránku RemoteApp.
2.      Na kartě **obrázky šablon** na tlačítko **Uložit** v dolní části stránky.
4.      Zadejte popisný název obrázek a určete umístění účtu úložiště. Zkontrolujte, zda že je umístěný na stejném místě jako kolekci RemoteApp nebo na místo, kde chcete vytvořit.
5.      Po zobrazení výzvy, stáhněte si skriptu do místního počítače.
6.      Zkopírujte parametry příkazu do textového pole do schránky.
7.      Otevřete okno zvýšenými oprávněními prostředí Windows PowerShell.
8.      Z okna zvýšenými prostředí Windows PowerShell přejděte k adresáři stejné místo, kam jste stáhli skript.
9.      Vložte zkopírovaný příkaz a stiskněte klávesu **Enter**.

    Proces nahrávání začne a doba trvání může záviset na spousta faktorů, včetně rychlosti sítě a velikosti obrázku

11.    Pokud odeslání neproběhne úspěšně z důvodu přerušení sítě nebo nějaké položky tak, vždy vrátíte k odeslání obrázku, kterou jste začali. Nahrávání pokračovat, spusťte skript znovu pomocí stejném příkazovém řádku.

> [AZURE.WARNING] Nikdy upravit skriptu nahrávání. Kontroly pro konkrétní provedena zajistit, aby obrázek splňuje požadavky na obrázek a požadavky aplikace.

## <a name="common-problems"></a>Běžné problémy

- Zkontrolujte, jestli že používat Windows PowerShell, ne Azure Powershellu. Budete potřebovat k instalaci modulu Azure PowerShell, protože některé moduly jsou potřeba během nahrávání.
- Nikdy pozměnit skript, ověření existují pro pohodlný.
- Když soubor virtuální pevný disk zablokovaný při nahrávání, zkopírujte soubor nebo ho můžete přesunout do nového umístění a pokus o nahrávání znovu. Může být některé Windows proces, který brání nahrát.  
