<properties
    pageTitle="Řízení chování žádostí o řetězce dotazu mezipaměti CDN Azure | Microsoft Azure"
    description="Azure CDN řetězec dotazu ukládání do mezipaměti ovládací prvky v čem se soubory v mezipaměti, pokud obsahují řetězce dotazu"
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

#<a name="controlling-caching-behavior-of-cdn-requests-with-query-strings"></a>Řízení chování mezipaměti požadavků CDN řetězce dotazu

> [AZURE.SELECTOR]
- [Standardní](cdn-query-string.md)
- [Azure CDN Premium z Verizon](cdn-query-string-premium.md)

##<a name="overview"></a>Základní informace

Ukládání do mezipaměti ovládací prvky v čem se soubory do mezipaměti, které obsahují řetězce dotazu řetězce dotazu.

> [AZURE.IMPORTANT] Produkty standardních a Premium CDN obsahují stejný dotaz řetězec funkci mezipaměti, ale v uživatelském rozhraní se liší.  Tento dokument popisuje rozhraní pro **Azure CDN standardní z Akamai** a **Azure CDN standardní z Verizon**.  Dotaz řetězec ukládání do mezipaměti s **Azure CDN Premium z Verizon**, najdete v článku [řízení chování ukládání do mezipaměti CDN požadavky řetězce dotazu - Premium](cdn-query-string-premium.md).

Existují tři způsoby:

- **Ignorovat řetězce dotazu**: Jedná se o výchozí režim.  Uzel hrany CDN předá řetězce dotazu z o synchronizaci adresářů původ na první žádost a mezipaměti majetku.  Všechny následující požadavky na této materiálů, které jsou podávané množství od okraje uzel bude ignorovat řetězce dotazu do mezipaměti materiálů vypršení platnosti.
- **Ukládání do mezipaměti přemostění pro adresu URL řetězce dotazu**: V tomto režimu řetězce dotazu nejsou do mezipaměti ukládány požadavky na uzel hrany CDN.  Uzel hrany načte majetku přímo z původ a předá o synchronizaci adresářů s každý požadavek.
- **Každý jedinečnou adresu URL do mezipaměti**: Tento režim pokládá jedinečné majetku s vlastním mezipaměti každý požadavek s řetězci dotazu.  Například odpověď od počátku žádost o *foo.ashx?q=bar* by cached na uzel okraje a pro další mezipaměti tuto stejnou řetězcem dotaz vrací.  Žádost o *foo.ashx?q=somethingelse* by mezipaměti jako samostatný majetku s vlastním času s dynamickými.

##<a name="changing-query-string-caching-settings-for-standard-cdn-profiles"></a>Změna nastavení pro standardní CDN profilů mezipaměti řetězce dotazu

1. Profil zásuvné CDN klikněte na CDN koncový bod, který chcete spravovat.

    ![Koncové body zásuvné CDN profilu](./media/cdn-query-string/cdn-endpoints.png)

    Otevře se zásuvné CDN koncového bodu.

2. Klikněte na tlačítko **Konfigurace** .

    ![Tlačítko Spravovat zásuvné CDN profilu](./media/cdn-query-string/cdn-config-btn.png)

    Konfigurace CDN zásuvné otevře.

3. Vyberte nastavení z rozevíracího seznamu **řetězce dotazu chování mezipaměti** .

    ![Řetězec dotazu CDN možnosti ukládání do mezipaměti](./media/cdn-query-string/cdn-query-string.png)

4. Po výběru, klikněte na tlačítko **Uložit** .

> [AZURE.IMPORTANT] Změny nastavení nemusí být ihned zobrazeny jako vyžaduje čas pro registraci se rozšíří prostřednictvím CDN.  <b>Azure CDN z Akamai</b> profilů šíření dokončí obvykle do jedné minuty.  <b>Azure CDN z Verizon</b> profilů šíření dokončí obvykle do 90 minut, ale v některých případech může trvat déle.
