<properties
   pageTitle="Vložené Microsoft Power BI – připojování ke zdroji dat"
   description="Power BI vložený, připojení ke zdrojům dat"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="connect-to-a-data-source"></a>Připojení ke zdroji dat

S **Power BI vložený**můžete je vložit sestavy do vlastní aplikace. Po vložení sestavy Power BI do aplikace sestavy se připojí k podkladová data **importováním** kopii dat nebo **přímo připojení** ke zdroji dat pomocí **DirectQuery**.

Této tabulce najdete rozdíly mezi používáním **Import** a **DirectQuery**.

|Import | DirectQuery
|---|---
|Tabulky, sloupce *a data* jsou importované nebo zkopírována do sestavy datovou sadu. Změnám zdrojových dat zobrazíte musíte aktualizovat nebo importovat, dokončeno, aktuální datovou sadu znovu.|Pouze *tabulek a sloupců* jsou importovat nebo zkopírována do sestavy datovou sadu. Vždy zobrazit nejaktuálnějšími daty.
S Power BI vložený jste DirectQuery pomocí cloudové zdroje dat, ale ne místního zdroje dat v současné době.

## <a name="benefits-of-using-directquery"></a>Výhody používání DirectQuery

Při použití **DirectQuery**existují dva hlavní výhody:

   -    **DirectQuery** umožňuje vytvářet vizualizace přes velkých sad dat, kde v opačném případě bude nebude proveditelné prvním importu všechna data.
   -    Základní změny dat můžete vyžadují aktualizaci dat a pro některé sestavy můžete třeba zobrazit aktuální data vyžadují přenosy velkých dat, vytváření nebude proveditelné znovu import dat. Naopak **DirectQuery** sestavy vždy použít aktuální data.

## <a name="limitations-of-directquery"></a>Omezení DirectQuery

   Existuje několik omezení použití **DirectQuery**:

   -    Všechny tabulky musí pocházet z jedné databáze.
   -    Pokud je příliš složitý dotaz, dojde k chybě. K odstranění chyby musí Refaktorovat dotazu tak, aby byl méně složitých. Pokud quuery musí být složité, musíte se k importu dat místo použití **DirectQuery**.
   -    Filtrování relace se omezí na jediný směr, nikoli obou směrech.
   -    Nemůžete změnit datový typ sloupce.
   -    Ve výchozím nastavení jsou umístěny omezení na výrazy jazyka DAX v míry povoleno. V tématu [DirectQuery a míry](#measures).

<a name="measures"/>
## <a name="directquery-and-measures"></a>DirectQuery a měr

Abyste měli jistotu, že dotazů odeslaných podkladovém zdroji dat přijatelného výkonu, omezení podléhají míry. Při použití **Power BI Desktop**, zkušení uživatelé můžete obejít toto omezení výběrem **Soubor > Možnosti a nastavení > Možnosti**. V dialogovém okně **Možnosti** zvolte **DirectQuery**a vyberte možnost **povolit neomezený míry v režimu DirectQuery**. Pokud tato možnost vybrána, lze použít výraz jazyka DAX, která platí pro míru. Uživatelé musí být vědom; ale, že některé výrazy, provádět dobře při importu dat může způsobit velice pomalá dotazy ke zdroji back-end v režimu **DirectQuery** . 

## <a name="see-also"></a>Viz taky
- [Začínáme s Microsoft Power BI vložený](power-bi-embedded-get-started.md)
- [Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)
