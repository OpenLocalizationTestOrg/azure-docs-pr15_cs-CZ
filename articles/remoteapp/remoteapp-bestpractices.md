<properties
    pageTitle="Doporučené postupy Azure RemoteApp | Microsoft Azure"
    description="Doporučené postupy pro konfiguraci a používání Azure RemoteApp."
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

# <a name="best-practices-for-configuring-and-using-azure-remoteapp"></a>Doporučené postupy pro konfiguraci a práce s nimi Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.

Tyto informace můžete nakonfigurovat a používat Azure RemoteApp efektivní.

## <a name="connectivity"></a>Připojení


- Vždy používejte nejnovější verzi klienta. Použití starších klientů může způsobit problémy s připojením a jiných jejich funkčnost bude omezena prostředí. Povolení automatické aktualizace aplikace pro zařízení s zajistí vždy instalace nejnovějších klienta.
- Vždy používejte stabilní a spolehlivé připojení k Internetu k dispozici.  
- Použití podporuje jen proxy připojení připojení optimální výkon.  SOCKS proxy server nepodporuje.

## <a name="applications"></a>Aplikace


- Uložte a zavřete aplikace RemoteApp po dokončení s aplikací. Není zavření aplikace může způsobit ztrátu dat.
- Ověření vlastní aplikace před použitím v Azure RemoteApp. Platí to i pro zajištění pracovat na platformě více relací a nechcete používat nepotřebných zdroje, jako jsou paměti a využití procesoru, který může starve jiným uživatelem ve stejné kolekce. Informace stáhněte si a zkontrolujte [Aplikace kompatibilitou doporučené postupy pro vzdálené plochy](http://www.dabcc.com/resources/Application%20Compatibility%20Best%20Practices%20for%20Remote%20Desktop%20Services.pdf).

## <a name="configuration-and-management"></a>Konfigurace a správy


- Aktuálnost obrázky šablon, instalaci aktualizací a jiných důležité opravy podle potřeby. Zajistíte tím, že jako Azure RemoteApp automatického měřítek plnit kapacity je opravené jednotlivé instance.  
- Ujistěte se, že nasazení služby Active Directory Federation Services (AD FS) zabezpečené a spolehlivé. V opačném případě může selhat ověřování klienta, zabrání uživatelům přistupovat k Azure RemoteApp.
- Nakonfigurujte obrázky šablon nainstalovaných aplikací, role nebo funkce tak, aby byly příslušnosti. Neměli spolehnout všechny instance tohoto virtuálních počítačích ve službě RemoteApp v trvalý stav.
    - Obsahují všechna uživatelská data v profilech uživatelů nebo jiných úložišť externí služby, jako místní souboru sdílené složky nebo OneDrive.
    - Uložte sdílené data úložišť externí služby, jako je místní soubor sdílené složky nebo na Onedrivu.
    - Konfigurace nastavení systémového na obrázek šablony a ne na jednotlivé virtuálních počítačích ve službě.
    - Vypnutí automatických aktualizací pro publikované aplikace – místo toho použít ruční na obrázek šablony a otestujte je před nasazením ze šablony.
