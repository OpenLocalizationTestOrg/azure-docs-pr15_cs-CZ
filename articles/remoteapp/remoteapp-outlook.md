<properties
    pageTitle="Používání Outlooku v Azure RemoteApp | Microsoft Azure" 
    description="Zjistěte, jak konfigurovat a používat aplikace Outlook v Azure RemoteApp | Microsoft Azure"
    services="remoteapp"
    documentationCenter=""
    authors="pavithir"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/15/2016"
    ms.author="elizapo" />

# <a name="using-microsoft-outlook-in-azure-remoteapp"></a>Pomocí aplikace Microsoft Outlook v Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.

Azure RemoteApp podporuje O365 Microsoft Outlooku. Přečtěte si další informace o [Office funguje v Azure RemoteApp](remoteapp-officesubscription.md). Existuje několik doporučených nastavení pro aplikaci Outlook při použití v Azure RemoteApp.

## <a name="cached-mode"></a>Režim Cached
Režim Cached je doporučená konfigurace pomocí aplikace Outlook v Azure RemoteApp. Při konfiguraci účtu Outlooku 2013 použít režim Cached Exchange funguje Outlooku 2013 z místní kopii poštovní schránky uživatele Microsoft Exchange, který je uložený offline datový soubor aplikace (OST) na počítači uživatele, spolu s Offline adresu knize (adresáře offline). Režim cached poštovní schránku a adresáře offline se automaticky aktualizují pravidelně služby O365. Další informace o [rozdílech mezi mezipaměti a online režimu](https://technet.microsoft.com/library/jj683103.aspx).

Při nastavení účtu nebo změnou nastavení účtu může uživatel vybrat **Režim Cached Exchange** nebo v **Online režimu** . Můžete taky nasadíte režim jedné z nich pomocí nástroje pro vlastní nastavení Office (OCT) nebo zásad skupiny.  

Přečtěte si [Podrobné informace o povolení režimu s mezipamětí](https://technet.microsoft.com/library/c6f4cad9-c918-420e-bab3-8b49e1885034#proc).

## <a name="search"></a>Hledání
Pomocí vyhledávání aplikace Outlook v Azure RemoteApp má omezení. Azure RemoteApp používá fondu VMs tak, aby zahrnoval uživatelských relací. Indexování hledání závisí na ID počítače, které se liší u různých VMs. Je možné, že pokaždé, když uživatel přihlásí Azure RemoteApp, budou se přesměrují do nového OM. Který bude znamenat, pokud jsme Povolit místní vyhledávání, indexování spustí každé změně ID počítače (Pokud je uživatel se na různé OM). V závislosti na velikosti. Soubor OST indexování může trvat delší dobu dokončení a vyčerpat prostředky potřebné pro jiné aplikace. Vyhledávání pouze nebude pomalé ale nemusí výsledky. Pomocí účtu profilu v režimu Online bude tento problém vyřešit, ale celkový výkon by dojít kvůli nedostatku místní mezipaměti (viz výše uvedený odkaz Další informace o rozdílech mezi režim cached a online). Bohužel indexované/místní vyhledávání se nedá zakázat a online hledání nelze povolit ve výchozím nastavení v Outlooku 2013.
