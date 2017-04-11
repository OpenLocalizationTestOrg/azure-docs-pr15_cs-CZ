<properties
    pageTitle="Přehled Azure CDN | Microsoft Azure"
    description="Zjistěte, co je síť pro doručování obsahu Azure (CDN) a jak se používá při vysokorychlostní obsah tak, že ukládání do mezipaměti objektů BLOB a statický obsah."
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
    ms.topic="hero-article"
    ms.date="09/30/2016"
    ms.author="casoper"/>

# <a name="overview-of-the-azure-content-delivery-network-cdn"></a>Základní informace o síť pro doručování Azure obsahu (CDN)

> [AZURE.NOTE] Tento dokument popisuje, co Azure obsahu Delivery Network (CDN), jak to funguje a funkce každý produkt Azure CDN.  Pokud chcete tyto informace přeskočit a přejít přímo do kurz vytvoření koncového bodu CDN, přečtěte si článek [Použití CDN Azure](cdn-create-new-endpoint.md).  Pokud chcete zobrazit aktuální umístění uzel CDN, najdete v článku [Azure CDN POP umístění](cdn-pop-locations.md).

Síť pro doručování obsahu Azure (CDN) ukládá statické webového obsahu na strategicky umístěné umístění pro doručování obsahu uživatelům poskytnout maximální výkon.  CDN nabízí vývojáři globální řešení pro doručování obsahu vysokorychlostní tak, že ukládání do mezipaměti obsah na fyzické uzly po celém světě. 

Výhody používání CDN na prostředky webu mezipaměti, patří:

- Lepší výkon a uživatel zaznamenat pro koncové uživatele, zvlášť když pomocí aplikací, kde jsou více opakované potřebný k načtení obsah.
- Velké měřítko pro lepší úchyt pro okamžité velkém zatížení, třeba na začátku události spuštění produktu
- Distribuce koncových uživatelů a podávání obsah ze strany serverů, méně přenosem původem.


## <a name="how-it-works"></a>Jak to funguje

![Přehled CDN](./media/cdn-overview/cdn-overview.png)

1. Uživatel (Alice) požaduje souboru (nazývané také aktivum) pomocí adresy URL s názvem zvláštní domény, například `<endpointname>.azureedge.net`.  DNS přesměruje žádost o ty nejcennější provádění umístění bodu o stavu (POP).  Obvykle jde POP zeměpisně nejblíže k uživatele.

2. Pokud servery edge v POP není soubor v mezipaměti, edge serveru požaduje soubor z původ.  Původ můžou být webovou aplikaci Azure, Azure cloudové služby, účet Azure úložiště nebo libovolnou veřejně přístupný webový server.

3. Původ vrátí soubor edge serveru, včetně volitelné záhlaví HTTP popisující souboru Time-to-Live (TTL).

4. Edge serveru mezipaměti a vrátí soubor původní o synchronizaci adresářů (Alice).  Soubor zůstane v mezipaměti na edge serveru, dokud vyprší jejich platnost hodnotu TTL.  Pokud původ nezadali TTL, je výchozí hodnota TTL sedmi dnů.

5. Dalším uživatelům potom požádat stejného souboru pomocí tohoto stejné adresy URL a mohou být směrovány do této stejné POP.

6. Pokud hodnota TTL souboru nevypršela, edge serveru vrátí soubor z mezipaměti.  Výsledkem rychlejší a další neodpovídá uživatelského prostředí.


## <a name="azure-cdn-features"></a>Funkce Azure CDN

Existují tři Azure CDN produkty: **Azure CDN standardní z Akamai** **Azure CDN standardní z Verizon**a **Azure CDN Premium z Verizon**.  V následující tabulce jsou uvedeny funkce dostupné pro každý produkt.

|       | Standardní Akamai | Standardní Verizon | Premium Verizon |
|-------|-----------------|------------------|-----------------|
| Snadno integrace se službou Azure služeb třeba [úložiště](cdn-create-a-storage-account-with-cdn.md), [Cloudové služby](cdn-cloud-service-with-cdn.md), [Web Apps](../app-service-web/cdn-websites-with-cdn.md)a [Media Services](../media-services/media-services-portal-manage-streaming-endpoints.md) | **& #x 2713;** | **& #x 2713;** | **& #x 2713;**|
| Správa prostřednictvím [rozhraní REST API](https://msdn.microsoft.com/library/mt634456.aspx), [.NET](./cdn-app-dev-net.md), [Node.js](./cdn-app-dev-node.md)nebo [Powershellu](./cdn-manage-powershell.md). | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| Podpora HTTPS | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| Vyrovnávání zatížení | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| Ochrana [DENIAL](https://www.us-cert.gov/ncas/tips/ST04-015) | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| Dvou zásobníku IPv4 a IPv6 | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| [Podpora název vlastní domény](cdn-map-content-to-custom-domain.md) | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| [Ukládání do mezipaměti řetězce dotazu](cdn-query-string.md) | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| [Filtrování GEO](cdn-restrict-access-by-country.md) |  | **& #x 2713;** | **& #x 2713;** |
| [Jak rychle odstranit](cdn-purge-endpoint.md) | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| [Před načítání materiálů](cdn-preload-endpoint.md) |  | **& #x 2713;** | **& #x 2713;** |
| [Základní analýzy](cdn-analyze-usage-patterns.md) |  | **& #x 2713;** | **& #x 2713;** |
| [Podpora protokolu HTTP/2](https://msdn.microsoft.com/library/mt762901.aspx) | **& #x 2713;**  |  |  |
| [Rozšířené HTTP sestavy](cdn-advanced-http-reports.md) | | | **& #x 2713;** |
| [Data v reálném stat](cdn-real-time-stats.md) | | | **& #x 2713;** |
| [Data v reálném upozornění](cdn-real-time-alerts.md) | | | **& #x 2713;** |
| [Modul pro doručování obsahu přizpůsobitelné, na základě pravidel pro](cdn-rules-engine.md) | | | **& #x 2713;** |
| Nastavení mezipaměti/záhlaví (pomocí [pravidel modulu](cdn-rules-engine.md))  | | | **& #x 2713;** |
| Adresa URL přesměrování/revize (pomocí [pravidel modulu](cdn-rules-engine.md)) | | | **& #x 2713;** |
| Pravidla mobilních zařízení (pomocí [pravidel modulu](cdn-rules-engine.md))  | | | **& #x 2713;** |

>[AZURE.TIP] Existuje k funkcím, které se mají zobrazit v Azure CDN?  [Sdělte nám svůj názor](https://feedback.azure.com/forums/169397-cdn). 

## <a name="next-steps"></a>Další kroky

Začínáme s CDN, najdete v článku [Použití CDN Azure](./cdn-create-new-endpoint.md).

Pokud jste zákazníkem existující CDN, můžete teď spravovat CDN koncových bodů prostřednictvím [portálu Microsoft Azure](https://portal.azure.com) nebo pomocí [Powershellu](cdn-manage-powershell.md).

Zobrazíte CDN v akci, podívejte se [video z našich 2016 vytvořit relace](https://azure.microsoft.com/documentation/videos/build-2016-leveraging-the-new-azure-cdn-apis-to-build-wicked-fast-applications/).

Zjistěte, jak můžete automatizovat Azure CDN s [.NET](./cdn-app-dev-net.md) nebo [Node.js](./cdn-app-dev-node.md).

Ceny informace, najdete v článku [CDN ceny](https://azure.microsoft.com/pricing/details/cdn/).
