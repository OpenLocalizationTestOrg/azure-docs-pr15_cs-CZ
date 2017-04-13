<properties
   pageTitle="Začínáme s Microsoft Power BI vložený"
   description="Power BI vložený, přidejte do aplikace business intelligence interaktivní sestavy v Power BI"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="get-started-with-microsoft-power-bi-embedded"></a>Začínáme s Microsoft Power BI vložený

**Power BI vložený** je Azure služba, která umožňuje vývojářům aplikací interaktivní sestavy Power BI můžete přidávat do své vlastní aplikace. **Power BI vložený** označené jako pracuje s existujícími aplikacemi, aniž by musel vzhled nebo změna uživatelů tak, jak přihlášení.

Materiály pro **Microsoft Power BI vložený** jsou k dispozici prostřednictvím rozhraní [API ARM Azure](https://msdn.microsoft.com/library/mt712306.aspx). V tomto případě zdrojů zřizujete je **Shromažďování pracovního prostoru v Power BI**.

![](media\power-bi-embedded-get-started\introduction.png)

## <a name="create-a-workspace-collection"></a>Vytvoření kolekce pracovního prostoru
**Pracovní prostor kolekce** je nejvyšší úrovně Azure zdrojů a kontejneru obsah, který bude vložen v aplikaci. **Pracovní prostor kolekce** lze vytvořit dvěma způsoby:

   -    Ručně pomocí portálu Azure
   -    Programově pomocí rozhraní API Manager(ARM) zdroje Azure

Podívejme se pokynů k vytváření **Kolekce pracovního prostoru** pomocí portálu Azure.

   1.   Otevřete a přihlaste se k **Portálu Azure**: [http://portal.azure.com](http://portal.azure.com).

   2.   Klikněte na **+ Nový** na horním panelu.

       ![](media\power-bi-embedded-get-started\create-workspace-1.png)

   3.   V části **dat a analýzy** na **Power BI vložený**.
   4.   Na **Zásuvné vytváření**zadejte požadované informace. **Ceny**najdete v článku [Power BI vložený ceny](http://go.microsoft.com/fwlink/?LinkID=760527).

       ![](media\power-bi-embedded-get-started\create-workspace-2.png)

   5. Klikněte na **vytvořit**.

**Pracovní prostor kolekce** bude chvíli trvat. Po dokončení jste se dostali do **Zásuvné kolekce pracovního prostoru**.

   ![](media\power-bi-embedded-get-started\create-workspace-3.png)

**Vytvoření zásuvné** s informacemi, že budete muset volat rozhraní API, která vytváření pracovních prostorů a nasazení obsahu na ně.

<a name="view-access-keys"/>
## <a name="view-power-bi-api-access-keys"></a>Klávesy se šipkami přístup k rozhraní API zobrazení Power BI

Jedním z nejdůležitějších použitelné informace potřebné k volat REST API Power BI je **Přístupových kláves z verze**. To byla použita pro vytvoření **tokeny aplikace** , které slouží k ověření vašich požadavků rozhraní API. Vaše **Přístupových kláves z verze**zobrazíte kliknutím na **Přístupových kláves z verze** na **Zásuvné nastavení**. Další informace o **aplikaci tokenů**najdete v článku [Authenticating a autorizaci s Power BI vložený](power-bi-embedded-app-token-flow.md).

   ![](media\power-bi-embedded-get-started\access-keys.png)

Můžete se spojkou "oznámení, že máte dva klíče.

   ![](media\power-bi-embedded-get-started\access-keys-2.png)

Zkopírujte klávesy a byly ukládány bezpečně v aplikaci. Je velmi důležité, že nakládáte klávesy stejně jako heslo, protože budete poskytnout přístup k celému obsahu ve vaší **Kolekci pracovního prostoru**.

Když jsou uvedeny dva klíče, jen jednu klávesu potřebné za určitou dobu. Druhý klíč slouží klávesy můžete obnovit pravidelně bez přerušení přístup ke službě.

Teď, když máte instanci Power BI pro aplikace a **Přístupové klávesy**, můžete importovat sestavy do vlastní aplikaci. Předtím, než se naučíte, jak importovat sestavy, v další části popisuje vytvoření datové sady Power BI a sestav můžete vložit do aplikace pro.

## <a name="create-power-bi-datasets-and-reports-to-embed-into-an-app"></a>Vytváření datových sad Power BI a sestav můžete vložit do aplikace

Teď, když jste vytvořili instanci Power BI pro aplikace a máte **Přístupových kláves z verze**, je potřeba vytvořit datové sady Power BI a sestav, které chcete vložit. Datové sady a sestavy můžete vytvořený s využitím **Power BI Desktop**. Můžete si stáhnout [bezplatné Power BI Desktop pro](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/). Případně můžete rychle začít, můžete si stáhnout [maloobchodní analýzy vzorku PBIX] (http://go.microsoft.com/fwlink/?LinkID=780547). Další informace o tom, jak používat **Power BI Desktop**najdete v tématu [Začínáme s Power BI Desktop](https://powerbi.microsoft.com/en-us/guided-learning/powerbi-learning-0-2-get-started-power-bi-desktop).

S **Power BI Desktop**připojíte se ke zdroji dat pomocí importu kopii data do **Power BI Desktop** nebo připojením k zdroj dat pomocí **DirectQuery**.

Této tabulce najdete rozdíly mezi používáním **Import** a **DirectQuery**.

|Import | DirectQuery
|---|---
|Tabulky, sloupce *a data* jsou importovány nebo zkopírována do **Power BI Desktop**. Při práci s vizualizacemi **Power BI Desktop** dotazy kopii data. Pokud chcete zobrazit všechny změny, které došlo k podkladová data, aktualizace nebo importovat, dokončeno, aktuální datovou sadu znovu.|Pouze *tabulek a sloupců* jsou importovat nebo zkopírována do **Power BI Desktop**. Při práci s vizualizacemi **Power BI Desktop** dotazy podkladovém zdroji dat, což znamená, že si prohlížíte vždy aktuální data.

Další informace o připojení ke zdroji dat najdete v tématu [připojení ke zdroji dat](power-bi-embedded-connect-datasource.md).

Po uložení prezentace v **Power BI Desktop**se vytvoří soubor PBIX. Tento soubor obsahuje sestavy. Kromě toho při importu dat PBIX obsahuje úplnou sadu nebo používáte **DirectQuery**, můžete PBIX obsahuje pouze datovou sadu schéma. Programově nasadit PBIX do pracovního prostoru pomocí rozhraní [Power BI Import API](https://msdn.microsoft.com/library/mt711504.aspx).

> [AZURE.NOTE] **Power BI vložený** má další rozhraní API pro změnu server a databázi, která váš datovou sadu přejdete a nastavit přihlašovací údaje účtu služby, využívající datové připojení k databázi. V tématu [SetAllConnections příspěvku](https://msdn.microsoft.com/library/mt711505.aspx) a [oprava brány zdroje](https://msdn.microsoft.com/library/mt711498.aspx).

## <a name="next-steps"></a>Další kroky
V předchozích krocích jste vytvořili kolekce pracovní prostor a první sestavy a datové sady. Teď je čas se dozvíte, jak napsat kód pro **Power BI vložený**. K vám pomůžou začít, jsme vytvořili ukázkové web appu: [Začínáme s vzorku](power-bi-embedded-get-started-sample.md). Vzorku vám ukáže, jak chcete:

  - Poskytování obsahu
      - Vytvoření pracovního prostoru aplikace Groove
      - Import souboru PBIX
      - Aktualizujte připojení řetězce a nastavit přihlašovací údaje pro vaše datové sady.

  - Bezpečné vložení sestavy

## <a name="see-also"></a>Viz taky
- [Začínáme s ukázkovými](power-bi-embedded-get-started-sample.md)
- [Ověřování a autorizace s Power BI vložený](power-bi-embedded-app-token-flow.md)
- [Power BI desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)
