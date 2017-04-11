<properties
    pageTitle="Poradce při potížích s kompresi souborů v Azure CDN | Microsoft Azure"
    description="Řešení problémů s Azure CDN kompresi souborů."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="casoper"/>
    
# <a name="troubleshooting-cdn-file-compression"></a>Poradce při potížích s kompresi souborů CDN

Tento článek vám Poradce při potížích s [CDN kompresi souborů](cdn-improve-performance.md)pomůže.

Pokud potřebujete další pomoc kdykoli v tomto článku můžete kontaktovat Azure odborníků na [MSDN Azure a fórech přetečení zásobníku](https://azure.microsoft.com/support/forums/). Můžete taky můžete poslat taky incident Azure podpory. Přejděte na [Web podpory Azure](https://azure.microsoft.com/support/options/) a klikněte na **Získat podporu**.

## <a name="symptom"></a>Příznaku

Komprese pro koncový bod je povolený, ale soubory se vrací nekomprimované.

>[AZURE.TIP] Chcete-li zkontrolovat, že jestli vaše soubory se vrací mu tuhle zkomprimovanou, budete muset použijte nástroj jako [Fiddler](http://www.telerik.com/fiddler) nebo webového prohlížeče [nástrojů pro vývojáře](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/).  Záhlaví odpovědí zaškrtněte HTTP vráceny režim cached CDN obsahu.  Pokud je záhlaví s názvem `Content-Encoding` s hodnotou **gzip** **bzip2**a **Uprostřed zúžené**obsahu komprimovány.
>
>![Obsah kódování záhlaví](./media/cdn-troubleshoot-compression/cdn-content-header.png)

## <a name="cause"></a>Příčina

Existuje několik možných příčin, včetně:

- Požadovaný obsah není vhodný pro kompresi.
- Komprese není povolený typ požadovaný soubor.
- Žádost HTTP neobsahuje záhlaví žádosti o typu platné komprese.

## <a name="troubleshooting-steps"></a>Návody na řešení potíží

> [AZURE.TIP] Stejně jako u nasadíte nové koncové body, změny konfigurace CDN dobu trvat, než se rozšíří prostřednictvím sítě.  Obvykle změny se projeví v rámci 90 minut.  Pokud jste nastavili komprese koncového bodu CDN poprvé, měli byste zvážit čekání 1 – 2 hodin abyste měli jistotu, které jste nastavení rozšíří do body POP komprese. 

### <a name="verify-the-request"></a>Ověření žádosti

Nejdřív jsme by měl provést kontrolu snadné vhodnosti na žádost.  Pomocí webového prohlížeče [nástrojů pro vývojáře](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/) zobrazíte právě žádosti.

- Ověření odesílání žádost o svoji adresu URL koncového bodu `<endpointname>.azureedge.net`a ne vaše origin.
- Ověřte žádost obsahuje záhlaví **Kódování přijmout** a hodnota této záhlaví **gzip** **Uprostřed zúžené**nebo **bzip2**.

> [AZURE.NOTE] Profily **Azure CDN z Akamai** podporují pouze **gzip** kódování.

![Záhlaví požadavků CDN](./media/cdn-troubleshoot-compression/cdn-request-headers.png)

### <a name="verify-compression-settings-standard-cdn-profile"></a>Zkontrolujte nastavení komprese (standardní CDN profil)

> [AZURE.NOTE] Tento krok platí jenom při profilu CDN je **Azure CDN standardní z Verizon** nebo **Azure CDN standardní z Akamai** profilu. 

Přejděte na koncový bod [Azure portál](https://portal.azure.com) a klikněte na tlačítko **Konfigurovat** .

- Ověřte, zda je povolen komprese.
- Ověřte, zda typ MIME pro příslušný obsah na komprimovat je součástí seznamu mu tuhle zkomprimovanou formátů.

![Nastavení komprese CDN](./media/cdn-troubleshoot-compression/cdn-compression-settings.png)

### <a name="verify-compression-settings-premium-cdn-profile"></a>Zkontrolujte nastavení komprese (Premium CDN profil)

> [AZURE.NOTE] Tento krok platí jenom při profilu CDN je profil aplikace **Azure CDN Premium z Verizon** .

Přejděte na koncový bod [Azure portál](https://portal.azure.com) a klikněte na tlačítko **Spravovat** .  Otevře se doplňkové portálu.  Přejděte myší na kartě **Velké HTTP** a potom přejděte myší na plovoucí panel **Nastavení mezipaměti** .  Klikněte na tlačítko **Komprese**. 

- Ověřte, zda je povolen komprese.
- Ověření, že seznam **Typů souborů** obsahuje seznam oddělenými čárkami (bez mezer) typů MIME.
- Ověřte, zda typ MIME pro příslušný obsah na komprimovat je součástí seznamu mu tuhle zkomprimovanou formátů.

![Nastavení komprese premium CDN](./media/cdn-troubleshoot-compression/cdn-compression-settings-premium.png)

### <a name="verify-the-content-is-cached"></a>Ověření, že cached obsahu

> [AZURE.NOTE] Tento krok platí jenom při profilu CDN je profil aplikace **Azure CDN z Verizon** (standardní nebo Premium).

Pomocí nástrojů pro vývojáře webového prohlížeče, zaškrtněte políčko záhlaví odpověď zajistit, že v oblasti, kde jsou požadovány mezipaměti.

- Zaškrtněte políčko **Server** odpověď záhlaví.  Záhlaví by měl s formátem **platformu (POP/serveru ID)**, jak je vidět v následujícím příkladu.
- Zaškrtněte políčko odpověď záhlaví **X-mezipaměti** .  Záhlaví by si přečíst **PŘÍSTUPŮ**.  

![Záhlaví CDN odpovědí](./media/cdn-troubleshoot-compression/cdn-response-headers.png)

### <a name="verify-the-file-meets-the-size-requirements"></a>Ověřte, že soubor splňuje požadavky na velikost

> [AZURE.NOTE] Tento krok platí jenom při profilu CDN je profil aplikace **Azure CDN z Verizon** (standardní nebo Premium).

Způsobilé pro kompresi souboru musí splňovat následující kritéria velikost:

- Větší než 128 bajtů.
- Menší než 1 MB.

### <a name="check-the-request-at-the-origin-server-for-a-via-header"></a>Zkontrolujte požadavek na zdrojový server **prostřednictvím** záhlaví

Záhlaví **prostřednictvím** protokolu HTTP označuje k webovému serveru, proxy server předávaných žádost.  Microsoft IIS webových serverů ve výchozím nastavení není komprimovat odpovědi při žádosti obsahuje záhlaví **prostřednictvím**  Chcete-li toto nastavení změnit, postupujte takto:

- **Služby IIS 6**: [nastavit HcNoCompressionForProxies = "FALSE" ve vlastnostech metabázi](https://msdn.microsoft.com/library/ms525390.aspx)
- **IIS 7 a až**: [Nastavení **noCompressionForHttp10** a **noCompressionForProxies** NEPRAVDA v konfigurace serveru](http://www.iis.net/configreference/system.webserver/httpcompression)

