<properties 
    pageTitle="Jak přesměrujete zařízení USB v Azure RemoteApp | Microsoft Azure" 
    description="Informace o používání přesměrování pro zařízení USB v Azure RemoteApp." 
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



# <a name="how-do-you-redirect-usb-devices-in-azure-remoteapp"></a>Jak přesměrujete zařízení USB v Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.

Přesměrování zařízení umožňuje uživatelům pomocí zařízení USB připojená k počítači nebo tabletu s aplikacemi Web apps v Azure RemoteApp. Například pokud sdílené Skype prostřednictvím Azure RemoteApp vaši uživatelé muset nebudou moct používat své zařízení kamery.

Než budete pokračovat, zkontrolujte, že si přečíst informace o přesměrování USB při [Použití přesměrování v Azure RemoteApp](remoteapp-redirection.md). Ale se doporučuje nusbdevicestoredirect:s: * nebude fungovat u webové kamery USB a nefunguje u některých USB tiskárny nebo multifunkčních zařízení USB. Návrh a z bezpečnostních důvodů Azure RemoteApp správce musí povolit přesměrování identifikátor GUID třídy zařízení nebo ID instance zařízení před pomocí těchto zařízení můžou vaši uživatelé.

I když tento článek pojednává o přesměrování webové kamery, můžete podobný přístup přesměrovat USB tiskárny a další multifunkčních zařízení USB, které nejsou přesměrovat **nusbdevicestoredirect:s:*** příkaz.

## <a name="redirection-options-for-usb-devices"></a>Možnosti přesměrování pro zařízení USB
Azure RemoteApp používá velmi podobné mechanismy pro přesměrování zařízení USB jako těch, které jsou k dispozici pro vzdálené plochy. Základní technologii můžete vybrat metodu správné přesměrování pro dané zařízení vytěžit maximum obou nejvyšší úrovně a zařízení USB technologie RemoteFX pomocí přesměrování **usbdevicestoredirect:s:** příkaz. Existují čtyři prvky na tento příkaz:

| Pořadí zpracování | Parametr           | Popis                                                                                                                |
|------------------|---------------------|----------------------------------------------------------------------------------------------------------------------------|
| 1                | *                   | Slouží k výběru všechna zařízení, které nejsou převzít uceleném přesměrování. Poznámka: záměrné * nefunguje k webové kamery USB.  |
|                  | {Třídy zařízení GUID} | Slouží k výběru všechna zařízení, které odpovídají třídě nastavení zadané zařízení.                                                           |
|                  | USB\InstanceID      | Vybere zařízení USB určeným pro dané instance ID.                                                                  |
| 2                | – ID USB\Instance    | Odebere nastavení přesměrování pro zadané zařízení.                                                                 |

## <a name="redirecting-a-usb-device-by-using-the-device-class-guid"></a>Přesměrování zařízení USB pomocí identifikátor GUID třídy zařízení
Existují dva způsoby, jak najít třídy zařízení GUID, který se dá použít pro přesměrování. 

První možnost je použít [Systémem definovaná zařízení nastavením třídy dostupné dodavatelům](https://msdn.microsoft.com/library/windows/hardware/ff553426.aspx). Vyberte předmětu, která nejvíc odpovídá zařízení připojené k místním počítači. Pro digitální fotoaparáty zřejmě zařízení pro zpracování obrázků předmětu nebo třídy zachytit videozařízení. Musíte udělat vyzkoušet třídy zařízení chcete najít správnou třídy GUID, se kterými spolupracuje místně připojené zařízení USB (v našem případě webové kamery).

Lepší způsob nebo druhou možnost, je tímto postupem najít identifikátor GUID třídy konkrétní zařízení:

1. Otevřete Správce zařízení, vyhledejte zařízení, které budete přesměrováni a pravým tlačítkem myši a potom otevřete dialogové okno Vlastnosti.
![Otevřete Správce zařízení](./media/remoteapp-usbredir/ra-devicemanager.png)
2. Na kartě **Podrobnosti** vyberte vlastnost **Guid předmětu**. Hodnota, která se zobrazí je identifikátor GUID třídy pro tento typ zařízení.
![Vlastnosti kamery](./media/remoteapp-usbredir/ra-classguid.png)
3. Použijte hodnotu identifikátor Guid třídy přesměrovat zařízení, která odpovídají ho.

Příklad:

        Set-AzureRemoteAppCollection -CollectionName <collection name> -CustomRdpProperty "nusbdevicestoredirect:s:<Class Guid value>"

Zkombinováním více přesměrování zařízení ve stejné rutiny. Příklad: přesměrovat místní úložiště a webové kamery USB, rutina vypadá takto:

        Set-AzureRemoteAppCollection -CollectionName <collection name> -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:<Class Guid value>"

Pokud nastavíte přesměrování zařízení podle předmětu GUID všechna zařízení, které odpovídají dané třídy GUID v určené kolekci přesměrováni. Pokud existuje víc počítačů v místní síti, které mají stejné webové kamery USB, je spustit jednu rutina přesměrovat všechny webové kamery.

## <a name="redirecting-a-usb-device-by-using-the-device-instance-id"></a>Přesměrování zařízení USB pomocí ID instance zařízení

Pokud chcete mít přesnější kontrolu jemně odstupňovaná a požadujete přesměrování na zařízení, můžete použít parametr **USB\InstanceID** přesměrování.

Nejtěžší část tento způsob je vyhledání ID instance zařízení USB Musíte mít přístup k počítači a konkrétní zařízení USB. Pak postupujte podle těchto kroků:

1. Povolit přesměrování zařízení v relace vzdálené plochy podle popisu v [jak použiju vlastní zařízení a prostředky v relaci vzdálené plochy?](http://windows.microsoft.com/en-us/windows7/How-can-I-use-my-devices-and-resources-in-a-Remote-Desktop-session)
2. Otevřete připojení ke vzdálené ploše a klikněte na **Zobrazit možnosti**.
3. Klepnutím na tlačítko Uložit aktuální nastavení připojení do souboru RDP **Uložit jako** .  
    ![Nastavení uložit jako soubor RDP](./media/remoteapp-usbredir/ra-saveasrdp.png)
4. Vyberte název souboru a jeho umístění, třeba "MyConnection.rdp" a "Tento PC\Documents" a soubor uložte.
5. Otevřete soubor MyConnection.rdp v textovém editoru a najděte ID instance zařízení, které chcete přesměrovat.

Teď použijte instance ID následující rutinu:

    Set-AzureRemoteAppCollection -CollectionName <collection name> -CustomRdpProperty "nusbdevicestoredirect:s: USB\<Device InstanceID value>"



### <a name="help-us-help-you"></a>Pomozte nám vám pomůžou 
Věděli jste, že kromě hodnocení v tomto článku a vyjádřit dolů pod, můžete provádět změny samotný článek? Něco chybí? Něco špatně? Můžu zapsat něco, co je právě matoucí? Posunutí nahoru a klikněte na **příkaz Upravit v GitHub** ke změnám – můžou být chodily do nám k revizi a potom po jsme odhlášení k nim, uvidíte změny a vylepšení tady.