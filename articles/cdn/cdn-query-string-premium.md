<properties
    pageTitle="Řízení Azure CDN Premium z Verizon ukládání do mezipaměti chování žádostí o řetězce dotazu | Microsoft Azure"
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

#<a name="controlling-caching-behavior-of-cdn-requests-with-query-strings---premium"></a>Řízení chování mezipaměti požadavků CDN řetězce dotazu - Premium

> [AZURE.SELECTOR]
- [Standardní](cdn-query-string.md)
- [Azure CDN Premium z Verizon](cdn-query-string-premium.md)

##<a name="overview"></a>Základní informace

Ukládání do mezipaměti ovládací prvky v čem se soubory do mezipaměti, které obsahují řetězce dotazu řetězce dotazu.

> [AZURE.IMPORTANT] Produkty standardních a Premium CDN obsahují stejný dotaz řetězec funkci mezipaměti, ale v uživatelském rozhraní se liší.  Tento dokument popisuje rozhraní pro **Azure CDN Premium z Verizon**.  Dotaz řetězec ukládání do mezipaměti s **Azure CDN standardní z Akamai** a **Azure CDN standardní z Verizon**, najdete v článku [řízení chování ukládání do mezipaměti CDN požadavky řetězce dotazu](cdn-query-string.md).

Existují tři způsoby:

- **Standardní mezipaměti**: Jedná se o výchozí režim.  Uzel hrany CDN předá řetězce dotazu z o synchronizaci adresářů původ na první žádost a mezipaměti majetku.  Všechny následující požadavky na této materiálů, které jsou podávané množství od okraje uzel bude ignorovat řetězce dotazu do mezipaměti materiálů vypršení platnosti.
- **bez mezipaměti**: V tomto režimu nejsou cached žádosti o řetězce dotazu na uzel CDN okraj.  Uzel hrany načte majetku přímo z původ a předá o synchronizaci adresářů s každý požadavek.
- **jedinečný mezipaměti**: Tento režim pokládá jedinečné majetku s vlastním mezipaměti každý požadavek s řetězci dotazu.  Například odpověď od počátku žádost o *foo.ashx?q=bar* by cached na uzel okraje a pro další mezipaměti tuto stejnou řetězcem dotaz vrací.  Žádost o *foo.ashx?q=somethingelse* by mezipaměti jako samostatný majetku s vlastním času s dynamickými.

##<a name="changing-query-string-caching-settings-for-premium-cdn-profiles"></a>Změna nastavení pro premium CDN profilů mezipaměti řetězce dotazu

1. Na zásuvné CDN profilu klikněte na tlačítko **Spravovat** .

    ![Tlačítko Spravovat zásuvné CDN profilu](./media/cdn-query-string-premium/cdn-manage-btn.png)

    Na portálu Správa CDN otevře.

2. Přejděte myší na kartě **Velké HTTP** a potom přejděte myší na plovoucí panel **Nastavení mezipaměti** .  Klikněte na **ukládání do mezipaměti řetězce dotazu**.

    Zobrazí se možnosti ukládání do mezipaměti řetězce dotazu.

    ![Řetězec dotazu CDN možnosti ukládání do mezipaměti](./media/cdn-query-string-premium/cdn-query-string.png)

3. Po výběru, klikněte na tlačítko **Aktualizovat** .


> [AZURE.IMPORTANT] Změny nastavení nemusí být ihned zobrazeny jako vyžaduje čas pro registraci se rozšíří prostřednictvím CDN.  <b>Azure CDN z Verizon</b> profilů šíření dokončí obvykle do 90 minut, ale v některých případech může trvat déle.
