<properties
    pageTitle="Používání přesměrování v Azure RemoteApp | Microsoft Azure"
    description="Zjistěte, jak konfigurovat a používat přesměrování v RemoteApp"
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

# <a name="using-redirection-in-azure-remoteapp"></a>Používání přesměrování v Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.

Přesměrování zařízení umožňuje uživatelům vzájemně vzdálené aplikace pomocí zařízení připojené k jejich místního počítače, telefonu nebo tabletu. Například pokud Skype prostřednictvím Azure RemoteApp vaší uživatel musí nainstalovanou na svém PC pro práci s Skype kameru. Toto je platí pro tiskárny, reproduktory, monitory a příslušenství připojení oblast USB.

RemoteApp využívá vzdálené plochy RDP (Protocol) a technologie RemoteFX přesměrování.

## <a name="what-redirection-is-enabled-by-default"></a>K čemu přesměrování aktivované ve výchozím nastavení?
Při použití RemoteApp následující přesměrování spuštěných ve výchozím nastavení. Nastavení RDP zobrazovat informace v závorkách.

- Přehrávat zvuky v místním počítači (**pro funkci Přehrát v tomto počítači**). (audiomode:i:0)
- Zachycení zvuk z místního počítače a pošlete ji ve vzdáleném počítači (**záznam z tohoto počítače**). (audiocapturemode:i:1)
- Používat místní tiskárny (redirectprinters:i:1)
- Porty COM (redirectcomports:i:1)
- Čipová karta zařízení (redirectsmartcards:i:1)
- Schránka (možnost kopírování a vkládání) (redirectclipboard:i:1)
- Vymazat, typ písma vyrovnání (Povolit nástroj vyrovnání písma: i:1)
- Přesměrujte všechny podporované přehrávání a zařízení zapojte zařízení. (devicestoredirect:s: *)

## <a name="what-other-redirection-is-available"></a>K čemu přesměrování je k dispozici?
Ve výchozím nastavení jsou zakázaná dvě možnosti přesměrování:

- Přesměrování jednotky (mapování jednotky): jednotky místního počítače se stanou namapované v vzdálenou relaci. Když pracujete v vzdálenou relaci díky uložení nebo otevření souborů z místních pevných discích.
- USB přesměrování: používáte zařízení USB připojených k místním počítači v rámci vzdálenou relaci.

## <a name="change-your-redirection-settings-in-remoteapp"></a>Změna nastavení přesměrování v RemoteApp
Změnit nastavení přesměrování zařízení pro kolekci pomocí prostředí PowerShell Microsoft Azure SDK. Po instalaci nového prostředí PowerShell a SDK, nejprve ji nakonfigurujte pro správu předplatného podle popisu v [tom, jak nainstalovat a nakonfigurovat Azure Powershellu](../powershell-install-configure.md).

Potom použijte příkaz podobnou následující nastavení vlastních vlastností RDP:

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:*"

(Poznámka: Tento *n* slouží jako oddělovač mezi jednotlivých vlastností.)

Pokud chcete získat seznam jaké vlastní vlastnosti RDP nakonfigurovány, spusťte následující rutinu. Všimněte si, že pouze vlastních vlastností se zobrazí jako výstup a nikoli vlastnosti výchozí:  

    Get-AzureRemoteAppCollection -CollectionName <collection name>

Pokud nastavíte vlastní vlastnosti je nutné zadat všech vlastních vlastností pokaždé, když; nastavení v opačném případě vrátí zakázáno.   

### <a name="common-examples"></a>Běžné příklady
Chcete-li povolit přesměrování jednotky použijte následující rutinu:  

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*”

Tahle rutina slouží k povolení USB a jednotky přesměrování:

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:*"

Tahle rutina slouží zakážete sdílení schránky:  

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "redirectclipboard:i:0”

> [AZURE.IMPORTANT] Nezapomeňte úplně odhlásit všem uživatelům kolekce (a nikoli pouze odpojit) sdílejícím změny. Abyste měli jistotu, že uživatelé úplně odhlášeni, přejděte na kartu **relací** v kolekci Azure portálu a odhlášení všem uživatelům, kteří jsou to od ní odpojilo nebo se přihlásit. Někdy může trvat několik sekund pro místní jednotky zobrazíte v Průzkumníkovi v rámci relace.

## <a name="change-usb-redirection-settings-on-your-windows-client"></a>Změnit nastavení přesměrování USB na klienta systému Windows

Pokud chcete použít USB přesměrování na počítači, který se připojuje k RemoteApp, je potřeba 2 akce, které potřebují problémy. 1 – váš správce musí povolit přesměrování USB na úrovni kolekce pomocí prostředí PowerShell Azure. 2 – v každém zařízení místo, kam chcete použít přesměrování USB budete muset povolit zásad skupiny, který umožňuje ho. Tento krok bude potřeba udělat pro každý uživatel, který chcete použít USB přesměrování.

> [AZURE.NOTE] Přesměrování USB s Azure RemoteApp je podporována pouze pro počítače se systémem Windows.

### <a name="enable-usb-redirection-for-the-remoteapp-collection"></a>Povolit přesměrování USB kolekce RemoteApp
Chcete-li povolit přesměrování USB na úrovni kolekce použijte následující rutinu:

    Set-AzureRemoteAppCollection -CollectionName <collection_name> -CustomRdpProperty "nusbdevicestoredirect:s:*"

### <a name="enable-usb-redirection-for-the-client-computer"></a>Povolit přesměrování USB klientského počítače

Konfigurace nastavení přesměrování USB ve vašem počítači:

1. Otevřete Editoru místních zásad skupiny (GPEDIT. NÁMOŘNÍ). (Z příkazového řádku spusťte gpedit.msc.)
2. Otevřete **počítač Konfigurace počítače\Zásady\Šablony správu\Součásti Windows\Vzdálená plocha\Hostitel ploše Client\RemoteFX USB přesměrování zařízení**.
3. Poklikejte na **RDP povolit přesměrování dalších podporovaných zařízení USB technologie RemoteFX z tohoto počítače**.
4. Vyberte **Povolit**a potom vyberte **správce a uživatele v oprávnění technologie RemoteFX USB přesměrování**.
5. Otevřete okno příkazového řádku s oprávněními správce a spusťte tento příkaz:

        gpupdate /force
6. Restartujte počítač.

Vytvoření a použití zásad přesměrování USB pro všechny počítače ve vaší doméně můžete také použít nástroj Správa zásad skupiny:

1. Přihlaste se k řadiče domény jako správce domény.
2. Spusťte konzolu Správa zásad skupiny. (Klikněte na **Start > Nástroje pro správu > Správa zásad skupiny**.)
3. Přejděte na domény nebo organizační jednotku, u kterého chcete vytvořit zásady.
4. Klikněte pravým tlačítkem na **Výchozí zásady domény**a potom klikněte na **Upravit**.
5. Otevřete **počítač Konfigurace počítače\Zásady\Šablony správu\Součásti Windows\Vzdálená plocha\Hostitel ploše Client\RemoteFX USB přesměrování zařízení**.
6. Poklikejte na **RDP povolit přesměrování dalších podporovaných zařízení USB technologie RemoteFX z tohoto počítače**.
7. Vyberte **Povolit**a potom vyberte **správce a uživatele v oprávnění technologie RemoteFX USB přesměrování**.
8. Klikněte na **OK**.  
