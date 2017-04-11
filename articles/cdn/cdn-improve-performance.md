<properties
    pageTitle="Zlepšení výkonu zkomprimováním souborů v Azure CDN | Microsoft Azure"
    description="Zjistěte, jak zlepšit rychlost přenosu souboru a zvyšují výkon načítání stránek zkomprimováním souborů v Azure CDN."
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
    ms.date="07/28/2016"
    ms.author="casoper"/>

# <a name="improve-performance-by-compressing-files"></a>Zlepšení výkonu zkomprimováním souborů

Komprese je jednoduchý a efektivní způsob ke zlepšení rychlost přenosu souboru a zvýšit výkon načítání stránek tak, že zmenšení velikosti souboru odesílá ze serveru. Snižuje náklady na šířku pásma a poskytuje další neodpovídá prostředí pro uživatele.

Existují dva způsoby, jak povolit kompresi:

- Povolit kompresi na server origin, ve kterém bude písmena CDN procházejí komprimované soubory a doručit klientů vyžadujících je komprimované soubory.
- Můžete povolit komprese přímo serverech CDN okraje, ve kterých se písmena CDN komprimovat soubory a slouží k koncoví uživatelé, i když nebudou komprimovány serverem origin.

> [AZURE.IMPORTANT] Změny konfigurace CDN chvíli se rozšíří prostřednictvím sítě.  <b>Azure CDN z Akamai</b> profilů šíření obvykle provede za v části jednu minutu.  Pro <b>Azure CDN z Verizon</b> profily obvykle zobrazí se vaše změny platit do 90 minut.  Pokud jste nastavili komprese koncového bodu CDN poprvé, měli byste zvážit čekání 1 – 2 hodin abyste měli jistotu, které jste nastavení rozšíří do POP před Poradce při potížích s komprese

## <a name="enabling-compression"></a>Povolení komprese

> [AZURE.NOTE] Úrovně standardních a Premium CDN poskytují stejnou funkci komprese, ale v uživatelském rozhraní se liší.  Další informace o rozdílech mezi standardních a Premium CDN úrovní najdete v tématu [Přehled CDN Azure](cdn-overview.md).

### <a name="standard-tier"></a>Standardní vrstvy

> [AZURE.NOTE] V této části platí pro **Azure CDN standardní z Verizon** a **Azure CDN standardní z Akamai** profil.

1. Profil zásuvné CDN klikněte na CDN koncový bod, který chcete spravovat.

    ![Koncové body zásuvné CDN profilu](./media/cdn-file-compression/cdn-endpoints.png)

    Otevře se zásuvné CDN koncového bodu.

2. Klikněte na tlačítko **Konfigurace** .

    ![Tlačítko Spravovat zásuvné CDN profilu](./media/cdn-file-compression/cdn-config-btn.png)

    Konfigurace CDN zásuvné otevře.

3. Zapněte možnost **Komprese**.

    ![Možnosti komprese CDN](./media/cdn-file-compression/cdn-compress-standard.png)

4. Použijte výchozí typy nebo upravit seznam odebíráním nebo přidání typů souborů.
    
    > [AZURE.TIP] Při možné, se nedoporučuje použít kompresi mu tuhle zkomprimovanou formátech, jako je ZIP MP3, MP4, JPG, atd.
    
5. Po provedení změn klikněte na tlačítko **Uložit** .

### <a name="premium-tier"></a>Premium osy

> [AZURE.NOTE] V této části platí pro **Azure CDN Premium z Verizon** profily.

1. Na zásuvné CDN profilu klikněte na tlačítko **Spravovat** .

    ![Tlačítko Spravovat zásuvné CDN profilu](./media/cdn-file-compression/cdn-manage-btn.png)

    Na portálu Správa CDN otevře.

2. Přejděte myší na kartě **Velké HTTP** a potom přejděte myší na plovoucí panel **Nastavení mezipaměti** .  Klikněte na **Komprese**.

    Zobrazí se možnosti komprese.

    ![Komprese souborů](./media/cdn-file-compression/cdn-compress-files.png)

3. Povolte kompresi kliknutím na přepínací tlačítko **Komprese povolena** .  Zadejte typy MIME, který chcete komprimovat jako seznam oddělený středníkem (bez mezer) v textovém poli **Typy souborů** .
        
    > [AZURE.TIP] Při možné, se nedoporučuje použít kompresi mu tuhle zkomprimovanou formátech, jako je ZIP MP3, MP4, JPG, atd. 

4. Po provedení změn klikněte na tlačítko **Aktualizovat** .


## <a name="compression-rules"></a>Komprese pravidel

V této tabulce popisují chování komprese Azure CDN pro každý scénář.

> [AZURE.IMPORTANT] Pro **Azure CDN z Verizon** (Standard a Premium) pouze vhodné soubory komprimovány.  Způsobilé pro kompresi, musíte souboru:
>
> - Být větší než 128 bajtů.
> - Být menší než 1 MB.
> 
> Pro **Azure CDN z Akamai**se dá použít pro kompresi všechny soubory.
>
> Ve všech produktech Azure CDN musí být soubor typu MIME, který byl [nakonfigurován pro kompresi](#enabling-compression).
>
> Profily **Azure CDN z Verizon** (Standard a Premium) podporují **gzip** **Uprostřed zúžené**nebo **bzip2** kódování.  Profily **Azure CDN z Akamai** podporují pouze **gzip** kódování.
>
> Koncové body **Azure CDN z Akamai** vždy požádat soubory **gzip** zakódovaný origin bez ohledu na požadavek klienta.

### <a name="compression-disabled-or-file-is-ineligible-for-compression"></a>Komprese zakázané nebo souboru je způsobilé pro komprese

|Požadovaný formát klienta (přes kódování přijmout záhlaví)|Formát souborů v mezipaměti|CDN odpověď na klienta|Poznámky|
|----------------|-----------|------------|-----|
|Komprimovat|Komprimovat|Komprimovat|   |
|Komprimovat|Nekomprimované|Nekomprimované|    | 
|Komprimovat|Není v mezipaměti|Komprimovaná nebo nekomprimované|Závisí na origin odpověď|
|Nekomprimované|Komprimovat|Nekomprimované|    |
|Nekomprimované|Nekomprimované|Nekomprimované|    |   
|Nekomprimované|Není v mezipaměti|Nekomprimované|     |

### <a name="compression-enabled-and-file-is-eligible-for-compression"></a>Komprese povolena a soubor je oprávněn pro komprese

|Požadovaný formát klienta (přes kódování přijmout záhlaví)|Formát souborů v mezipaměti|CDN odpověď na klienta|Poznámky|
|----------------|-----------|------------|-----|
|Komprimovat|Komprimovat|Komprimovat|CDN transcodes mezi podporované formáty|
|Komprimovat|Nekomprimované|Komprimovat|CDN provádí komprese|
|Komprimovat|Není v mezipaměti|Komprimovat|CDN provádí kompresi origin chybovou nekomprimované.  **Azure CDN z Verizon** předat nekomprimované souboru na první žádost a pak zkomprimovat a mezipaměti souboru pro následující požadavky.  Soubory s `Cache-Control: no-cache` záhlaví budou nikdy komprimovány. 
|Nekomprimované|Komprimovat|Nekomprimované|CDN provádí dekomprese|
|Nekomprimované|Nekomprimované|Nekomprimované|     |  
|Nekomprimované|Není v mezipaměti|Nekomprimované|     |

## <a name="media-services-cdn-compression"></a>Mediální služby CDN komprese

Media Services CDN povolena streamování koncové body, komprese aktivované ve výchozím nastavení pro následující typy obsahu: aplikace/vnd.ms-sstr xml, application/dash+xml,application/vnd.apple.mpegurl, aplikace/f4m + xml. Můžete nelze povolit nebo zakázat komprese pro uvedené typy pomocí portálu Azure.  

## <a name="see-also"></a>Viz taky
- [Poradce při potížích s kompresi souborů CDN](cdn-troubleshoot-compression.md)    
